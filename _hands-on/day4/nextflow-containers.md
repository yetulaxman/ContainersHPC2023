---
topic: nextflow
title: Tutorial3 - Nextflow example with containers 
---

## Tutorial 3: Nextflow pipeline with containers and other useful features

In this tutorial, you will learn nextflow script that uses 
 - singularity container
 - user-defined profiles
 - visualisation capabilities
 - batch job script to deploy a pipeline on Puhti

Containerised applications are highly portable and reproducible for scientific applications. Fortunately, Nextflow smoothly supports integration with popular containers ( e.g., [Docker](https://www.nextflow.io/docs/latest/docker.html) and [Singularity](https://www.nextflow.io/docs/latest/singularity.html)) to provide a light-weight virtualisation layer for running software applications. You can either create your own Docker/Singularity image or download pre-existing one from a container registry. Please note that you can only work with *Singularity* containers on Puhti as *docker* containers require prevelized access which CSC users **don't** have it on Puhti.

When working with Nextflow scripts using containers, pay attention to the following things:
- Nextflow usually takes care of mounting work directory for that process on container image. Other files/directories need to be mounted through run time options if needed.
- Required input files are staged in and out automatically via Nextflow mechanisms.
- Nextflow process gets executed in a container environment

### Launch an example nextflow application 

Let's download material needed for this tutorial from github as shown below:

```nextflow
cd /scratch/project_xxxx/$USER/nextflow_tutorial 
module load git 
git clone https://github.com/yetulaxman/nf_coverage_demo.git
cd nf_coverage_demo
git clone https://github.com/iarcbioinfo/data_test

```

Here the aim of bioinformatics analysis is to plot mean coverage over a series of BAM files

### Using containers with nextflow *via* commandline option
Here is a simple example syntax (for an alternative approach, see *profiles* section below) to use docker/singularity containers: 

```bash
## For Docker
nextflow run <nextflow_script>  -with-docker <image_path> # e.g.,image_path = biocontainers/fastqc:v0.11.9_cv7

## For Singularity
nextflow run <nextflow_script>  -with-singularity <image_path>    
```

Because of the way how nextflow works with containers, you **don't** need to have software (e.g., `fastqc`) installed on your machine. It will download container image and  uses *fastqc* from the image.

### Using containers with nextflow *via* profiles

We often need to add some other attributes besides a container flag as mentioned above. This is accomplished using *profiles*. A profile is a set of configuration attributes that can be activated/chosen when launching a pipeline execution.  When a workflow script is launched, Nextflow first looks for a file named `nextflow.config` in the current directory and in the workflow (or script base) directory (if different from current directory). Finally, it checks for the file $HOME/.nextflow/config. Configuration files can contain the definition of one or more profiles. 

Example profiles are shown below:

```nextflow
profiles {


 docker {
    docker.enabled = true 
    process.container = 'iarcbioinfo/nf_coverage_demo:v2.3'
    pullTimeout = "200 min"
  }
  
  singularity {
    singularity.enabled = true 
    singularity.autoMounts = true
    process.container = 'shub://IARCbioinfo/nf_coverage_demo:v2.3'
    pullTimeout = "200 min"
  }
}
```

copy above script and paste in `nextflow.config` file which is located in current directory.

You can then launch nf_coverage workflow (from `nf_coverage_demo` folder) with defined profiles as shown below:

```bash
module load nextflow

nextflow run plot_coverage.nf  \
          -profile singularity \
          --bam_folder data_test/BAM/BAM_multiple/ \
          --bed data_test/BED/TP53_exon2_11.bed
```

### Reporting and visualising nextflow pipeline
Nextflow provides options for reporting and visualisation your pipeline using the following nextflow flags:
```bash
-with-dag
-with-timeline
-with-report
```
You can either use the flags in commandline or add each feature to config file as discussed below:

####  `dag`
Either use the following flag (-with-dag) when launching script as below:

```
nextflow run  <nextflow_script>  -with-dag <file-name>.dot
```
or add the following script to `nextflow.config` file at the end.

```
dag {
  enabled = true
  file="dag.png"
}
```
####  `timeline`

Either use the following flag (-with-timeline) when launching script as below:
```
nextflow run <nextflow_script> -with-timeline <file-name>.html
````
or add the following script to `nextflow.config` file at the end.

```
timeline {
  enabled = true
}
```
####  `report`
Either use the following flag (-with-report) when launching script as below:

```
nextflow run <nextflow_script> -with-report <file-name>.html
```
or add the following script to `nextflow.config` file at the end.
```
report {
  enabled = true
}
```

### `trace`

Either use the following flag (-with-trace) when launching script as below:

```
nextflow run <nextflow_script> -with-trace <file-name>.txt
```
or add the following script to `nextflow.config` file at the end.

 ```
trace {
  enabled = true
}
```

For the convenience of this tutorial, configure all visualisation features (i.e., dag/timeline/reports/trace) into `nextflow.config` file.

### Launch nextflow application as a batch job

Once you have configured profiles for singularity and enabled reporting/visualisation features in *nextflow.config* file, you can use the following batch script to submit on Puhti:

```
#!/bin/bash
#SBATCH --time=00:10:00
#SBATCH --partition=test
#SBATCH --account=project_xxx


export TMPDIR=$PWD
export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD
unset XDG_RUNTIME_DIR


# Activate  Nextflow on Puhti
module load  nextflow

# Nextflow command here
nextflow run plot_coverage.nf  \
       -profile singularity  \
       --bam_folder data_test/BAM/BAM_multiple/  \
       --bed data_test/BED/TP53_exon2_11.bed 
```
copy and paste above script to a file (nf_coverage.sh), replace project number with correct one in slurm directives and finally submit sbatch script to Puhti cluster:

```
rm -fr work/    # remove previous analysis results
rm *.html *.png trace.txt # remove these visualisation files if any
sbatch nf_coverage.sh # start a fresh job
```

### Viewing analysis results on Puhti 

Copy all nextflow report and visualisation files from working directory (i.e., .html, .dot and .txt files) to home directory to view them from your local browser.

```
mkdir -p $HOME/nextflow_output
cp *.html *.png *.txt *.pdf  $HOME/nextflow_output
```
> **Updated note**:  if the following SSH tunneling looks complicated, feel free to use [Puhti web interface](https://www.puhti.csc.fi) to view files in $HOME/nextflow_output  by launching Desktop under Apps. You can find the files in you home folder.

One has to open a port on Puhti login node to access files on your Puhti home directory from your local computer via browser. In this course, every participant should have a *unique port number* opened on Puhti login node. **Open a new terminal** on your local machine and replace *$port* value with some random number (e.g., a number between 7000 and 9000) before executing the following command:

```
ssh -L $port:localhost:$port <your_csc_username>@puhti.csc.fi  # e.g., with port number: 7077 
                                                              # ssh -L 7077:localhost:7077 <username>@puhti.csc.fi 
```
and then run the following command (also use the same port value that you have slected before) on the login node:
```
python3 -m http.server $port # with port number: 7077 -> python3 -m http.server 7077
```

Point your browser to http://localhost:$port (remember to replace your port number with *$port*) on your local machine. You can now view all files available on your Puhti home directory. 

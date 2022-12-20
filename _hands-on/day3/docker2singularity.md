---
topic: singularity
title: Tutorial1 -  Conversion of Docker Images to singularity
---

# Converting Docker Image to Singularity

We have learned to convert a Docker image to singularity using *singularity build* subcommand in previous episodes. And also, note that we can use *singularity pull* subcommand to achieve the same outcome. As an example for docker image conversion, we will use a bioconda package named, [trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) software which is available as a docker image in Quay container registry. Trimmomatic is a flexible read trimming tool for Illumina NGS data. 

###  Expected outcome of this tutorial:
Upon completion of this tutorial, you will be able to:
- Launch a singularity container of a bioconda package which is available as a docker image (e.g., trimmomatic image)
- Manage singularity TMPDIR and CACHEDIR directories in HPC environment


### Launching trimmomatic software as a singularity container

1. Launch an interactive session on Puhti as below:

   ```bash
   # start interactive node as below and choose your project name on command prompt
   sinteractive -c 2 -m 4G -d 100
   ```
2. Visit [trimmomatic software webpage](https://bioconda.github.io/recipes/trimmomatic/README.html)  as available on bioconda channel. Look for the URI (Looks
   something like the following: quay.io/biocontainers/trimmomatic:tag) of trimmomatic image in *docker pull* command and incorporate [the latest tag](https://quay.io/repository/biocontainers/trimmomatic?tab=tags) information for trimmomatic software. Once you have URI path with tag information, you can build a singularity image as below:
  
   ```bash
    singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
   Upon successful completion of above command, you will be able to see trimmomatic singularity image (name: trimmomatic_0.32--hdfd78af_4.sif) in the current
   directory.
   
3. When you convert multiple smaller images or a bigger image, the singularity cache can take up lot of space. Please note that the singularity cache directory is 
   by default your HOME directory which is limted by disk space quota in CSC HPC environment. As a best practice tip, do not pull a Singularity image cache to your 
   Puhti Home Directory. In order to avoid such cache issues which would result in disk space errors, we recommend resetting Singularity TMPDIR and CACHEDIR 
   directories either to Lustre scratch or $LOCAL_SCRATCH  before performing image conversion as below:
  
   ```bash  
    export SINGULARITY_TMPDIR=/scratch/project_xxxx/$USER  (or $LOCAL_SCRATCH)
    export SINGULARITY_CACHEDIR=/scratch/project_xxxx/$USER (or $LOCAL_SCRATCH)
    unset XDG_RUNTIME_DIR
    singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
    It is better to use LOCAL_SCRATCH directories as Singularity TMPDIR and CACHEDIR directories especially if the image sizes are quite big (in the order of 
    GBs). Usually larger docker images are composed of several layers and importing and unpacking the layers into one single image file for singularity can be I/O
    intensive.

4. Launch trimmomatic container and check comamndline help 
    ```bash
    singularity exec trimmomatic_0.32--hdfd78af_4.sif trimmomatic --help   # or simply ./trimmomatic_0.32--hdfd78af_4.sif
   ```
  
   > Few useful tips for managing singularity cache:
  
    ```bash  
    singularity cache list  # You can check the disk space taken up by the image in cache folder
    singularity cache clean  # clean singularity cache
    ```



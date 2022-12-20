---
topic: snakemake
title: Tutorial6 - Sankemake toy example (WIP)
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Running Snakemake with a pre-installed module on Puhti

Snakemake is installed as a module on Puhti and can be used as below: 


```bash
module load snakemake/7.17.1
snakemake -s test.smk      -j 1     --latency-wait 60   --cluster "sbatch -t 10 \
--account=project_2001659 --job-name=fastqc-help  --tasks-per-node=1 --cpus-per-task=1 \
--mem-per-cpu=4000 -p test"
```

In the above script, contents of nextflow script, *test.smk* are as below:

```bash

rule all:
        input: "CAPITALISE.txt"

rule say_hello:
        output: "smaller_case.txt"
        shell:
                """
                echo "testing toy example for snakemake workflow" > smaller_case.txt
                """
rule capitalise:
        input: "smaller_case.txt"
        output: "CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```


### Installing other python packages needed for Snakemake workflow

The current Snakemake module on Puhti is based on pip-installation and has a limited set of other python packages. For any real-world examples, one needs to have other python packages which can be installed using *pip* in a user defined folder (e.g., a directory in scratch or Projappl)

Change to working directory to e.g., scratch area  and create a folder for installing python packages (Here, let's install *matplotlib* package that is not available in Snakemake module) 

```bash
cd /scratch/project_xxx/$USER/snakemake_workflow/ && mkdir venv
```

Install the needed packages:

```bash
export PYTHONUSERBASE="/scratch/project_xxx/$USER/snakemake_workflow/venv"
pip3.9 install --user matplotlib 
export PATH="/scratch/project_xxx/yetukuri/snakemake_workflow/venv/bin:$PATH"
export PYTHONPATH="/scratch/project_xxx/yetukuri/snakemake_workflow/venv/lib/python3.9/site-packages/"  
#(this needs to be the same version of python package)
```

Content of test2.smk file is as shown below:

```bash
import matplotlib   # You can use it if needed as part of preporcessing of data.

rule all:
        input: "CAPITALISE.txt"

rule say_hello:
        output: "smaller_case.txt"
        python -c "import matplotlib"
        shell:
                """
                echo "testing toy example for snakemake workflow where matplotlib package is installed" > smaller_case.txt
                """
rule capitalise:
        input: "smaller_case.txt"
        output: "CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```

Run Snakemake workflow:

```bash
module load snakemake/7.17.1
snakemake -s test2.smk      -j 1     --latency-wait 60   --cluster "sbatch -t 10 \
--account=project_2001659 --job-name=fastqc-help  --tasks-per-node=1 --cpus-per-task=1 \
--mem-per-cpu=4000 -p test"
```

## Snakemake workflow with singularity containers

Use * --use-singularity* flag to activate singularity environement and bind mount necessary disk space 

```bash
module load snakemake/7.17.1
snakemake -s test3.smk      -j 1     --latency-wait 60     --use-singularity --singularity-args "-B /scratch/project_2001659/yetukuri/snakemake_workflow:/scratch/project_2001659/yetukuri/snakemake_workflow"   \
--cluster "sbatch -t 10 --account=project_xxx --job-name=fastqc-help  --tasks-per-node=1 --cpu


Contents of test3.smk are as below:

```bash
# test availability of different packages 
import matplotlib

import pkg_resources;installed_packages = pkg_resources.working_set;installed_packages_list = sorted(["%s==%s" % (i.key, i.version) for i in installed_packages]);print(installed_packages_list)

rule all:
        input: "HELP-CAPITALISE.txt"

rule say_hello:
        output: "fastqc-help.txt"
        singularity: "docker://biocontainers/fastqc:v0.11.9_cv8"
        shell:
                """
                fastqc --help > "fastqc-help.txt"
                """

rule capitalise:
        input: "fastqc-help.txt"
        output: "HELP-CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```

## Running Snakemake with HyperQueue
If your workflow manager is using sbatch for each process execution and you have many short processes it's advisable to switch to HyperQueue to improve throughput and decrease load on the system batch scheduler.

Using Snakemake's --cluster flag we can use hq submit instead of sbatch:

```
snakemake --cluster "hq submit --cpus <threads> ..."

```

> Note: If you want to install a specific version of Snakemake along with other python packages, we recommend installing  using pip. This way, one can use
  singularity containers smoothly in Snakemake workflow.

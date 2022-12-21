---
topic: nextflow
title: Tutorial3 -  Running existing pipelines (nf-core pipeline)
---

nf-core is a community effort to collect a curated set of analysis pipelines built using Nextflow. Here we use [Sarek workflow](https://github.com/nf-core/sarek) as an example pipeline to detect variants on whole genome or targeted sequencing data. 

Here is an example batch script to run the pipeline on Puhti:
```bash
#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=project_xxxx
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=4000


export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD
unset XDG_RUNTIME_DIR

# Activate  Nextflow on Puhti
module load nextflow

# nf-core pipeline examples here
# Variant calling on genome data
nextflow run nf-core/sarek -r 2.7.1 -profile test,singularity -resume
# proteomics example
# nextflow run nf-core/proteomicslfq  -r 1.0.0  -profile test,singularity -resume
# metabolomics example
# nextflow run nf-core/metaboigniter -r 1.0.1 -profile test,singularity -resume
```
copy and paste the above script to a file named sarek_nfcore.sh and replace your project number with project_xxxx in slurm directives.

Finally, submit your job

```bash
sbatch sarek_nfcore.sh
```

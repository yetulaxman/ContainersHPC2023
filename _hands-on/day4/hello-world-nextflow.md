---
topic: nextflow
title: Tutorial1 - Hello-world example
---

# Nextflow on Puhti Supercomputer
Running and managing workflows for bioinformatics applications can be challenging as the workflows usually are fragile eco-systems of several software tools and their dependencies. We therefore need a workflow manager like Nextflow to manage our scientific workflow. [Nextflow](https://www.nextflow.io/docs/latest/index.html) is a [groovy-based](https://en.wikipedia.org/wiki/Apache_Groovy) language for expressing the entire workflow in a single script and also facilitates the ease of working with workflows by rendering several useful features as mentioned below: 
 - Workflow management
 - Reproducibility
 - Portability
 - Scalability
 - Parallelization
 - Easy prototyping 
 - Partial resumption

## Learning Objectives
Upon completion of these tutorials in course, you will be able to learn: 
- The basic understanding of how Nextflow works
- The deployment of Nextflow scripts on Puhti supercomputer
- How to use Singularity containers with Nextflow for your analysis


## Set up your work environment for tutorials on Puhti
This hands-on tutorial uses Puhti supercomputer for executing Nextflow scripts for interactive and batch jobs. One therefore needs to have either a training or user account at CSC to access Puhti.

Login to Puhti using `ssh` command followed by making a work directory named `nextflow_tutorial` on *scratch* drive as shown below: 

```nextflow
ssh <your_csc_username>@puhti.csc.fi
mkdir -p  /scratch/project_xxxx/$USER/nextflow_tutorial && cd /scratch/project_xxxx/$USER/nextflow_tutorial 
```

### Start an interactive session on Puhti

Lanuch an [interactive session](https://docs.csc.fi/computing/running/interactive-usage/) on Puhti as below:
```
sinteractive -c 2 -m 4G -d 250 -A project_xxxx  # replace actual project number here
module load nextflow
```

## Tutorial 1: Hello-world example 
In this Hello-world tutorial, you will learn how to run a Nextflow script as well as understand the default location of resulting output files.

Download course material from CSC's [`allas`](https://docs.csc.fi/data/Allas/) object storage as shown below:

```bash
wget https://a3s.fi/nextflow/tutorial_demo.tar.gz
tar -xavf tutorial_demo.tar.gz && rm tutorial_demo.tar.gz
```

After unpacking the `tutorial_demo.tar.gz` file, you can see `hello_demo` folder which has hello-world script (ending with `.nf`) for running this demo. Execute the script by entering the following command on your interactive Puhti terminal: 

```nextflow
cd hello_demo
nextflow run hello-world.nf
```
This script defines one process named `sayHello`. This process takes a set of greetings from different languages and then writes each one to a separate file.

The resulting terminal output would look similar to the text shown below:

```nextflow
N E X T F L O W  ~  version 20.07.1
Launching `hello-world.nf` [cheeky_shaw] - revision: 3ffdbdd5c7
executor >  local (5)
[e2/9aa8c8] process > sayHello (5) [100%] 5 of 5 ✔
```
#### Where are output files created from the above hello-world example?

The hexadecimal number, like e2/9aa8c8, identifies a unique process execution and the number is also the prefix of directory where `sayHello` process is executed. You can inspect the files produced by above script by changing to directory $PWD/work.

Execute the following command on your terminal:
```
ls -l work/*/*
```
You can see that there is a separate output file created under each directory. 

#### What kind of hidden files exist inside $PWD/work directory?

Hidden files are present in each process directory, and the files are very handy when you want to debug a failed process.

you can find the hidden files as shown below:

```
ls -la work/*/*
```

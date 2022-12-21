---
topic: bioapplications
title: Tutorial1 - BLAST example
---

There are many bioinformatics applications available as docker images in  different registries such as [DockerHub](https://hub.docker.com), [Quay Container Registry](https://quay.io) and [Biocontainers](https://biocontainers.pro). Our aim here is to acquire some basic skills to use any containerised application from the registries for our bioinformatics analysis in HPC environment.  These skills greatly empower bioinformaticians with several thousands of containerised applications which can be easily deployed in a reproducible manner. This tutorial provides hands-on experience with one of the popular bioinformatics tools called, BLAST which compares nucleotide or protein sequences to corresponidng sequences in a database and calculates the statistical significance of the matches.

### Expected learning outcome from tutorial:
After this tutorial, you will be able to: 
- Search a bioinformatics application (in this case, BLAST) as a docker image from a registry (from Quay.io)
- Deploy the application as a singularity container in Puhti environment interactively


### Run BLAST analysis using a singularity container

1. Navigate to your *scratch* folder and launch an interactive terminal on Puhti supercomputer:

   ```bash 
   cd /scratch/project_xxxx/$USER    # choose your course (or your own) project 
   mkdir -p BLAST && cd BLAST   #  create a separate folder for this analysis
   sinteractive -c 2 -m 4G -d 100   # choose project number upon terminal prompt
   ``` 
2. You'll use a BLAST (Basic Local Alignment Search Tool) container image as available from [Quay Registry](https://quay.io). Visit the webpage of Quay registry 
   and search for the BLAST image (using keyword: BLAST) on the top right hand corner. You can find the BLAST images from different repositories/accounts. Pick the 
   one under biocontainer repository (i.e., biocontainers/blast). And also search for different tags available for the image (hint: on the left side menu, click on
   *tags* icon). Once you managed to find a fully qualified URI (= docker://hostname/repository/imagename:tag) for docker image, you can convert it to singularity 
   image using **singularity build** subcommand as below:
    
   ```bash
   singularity build blast_quay.sif docker://quay.io/biocontainers/blast:2.12.0--pl5262h3289130_0
   ```
   Once the image is built successfully, you should be able to see singularity image file (file name: blast_quay.sif) in the current directory.

3. Run a simple command inside of the singularity container to get commanline help for blastp  
   ```bash
   singularity exec blast_quay.sif blastp -help
   ```

4. Download and prepare the data needed to start analysis with BLAST tool

   ```bash
    wget https://www.uniprot.org/uniprot/Q61074.fasta # mouse Protein phosphatase 1G
    ```
   This is a FASTA sequence for a mouse protein.  You'll also need a reference database to BLAST against and the database can be downloaded from the link as shown
   below:

   ```bash
    curl -O ftp://ftp.ncbi.nih.gov/refseq/M_musculus/mRNA_Prot/mouse.1.protein.faa.gz
    gunzip mouse.1.protein.faa.gz
    ```
    You need to prepare the mouse database with `makeblastdb` for the search using the following command:

    ```bash
    # mkdir makeblastdb
    singularity exec -B $PWD:$PWD blast_quay.sif makeblastdb -in mouse.1.protein.faa -dbtype prot
    ```  
    After the container has finished the job, you should see several new files in the current directory.
    
5. Finally, as you have all the input data ready for analysis, you can now do the final alignment step using `blastp` as below:

   ```bash
   singularity exec -B $PWD:$PWD blast_quay.sif blastp -query Q61074.fasta -db mouse.1.protein.faa -out results.txt
   ```
   The final results are stored in `results.txt`;

   ```bash
   less results.txt   # you should be able to see the sequenc matches the isoforms of phosphatases in mouse database as the best hits in this blast search
   ```

   ```bash
   
   Sequences producing significant alignments:                          (Bits)  Value

   XP_036016308.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_030105454.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_030105453.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_006523925.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_036016307.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_030105452.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_036016306.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_006523924.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_011244623.1 protein phosphatase 1B isoform X1 [Mus musculus]       130     4e-32
   XP_011244624.1 protein phosphatase 1B isoform X1 [Mus musculus]       130     4e-32
   XP_006523923.1 protein phosphatase 1B isoform X1 [Mus musculus]       130     4e-32
   XP_036013149.1 protein phosphatase 1A isoform X2 [Mus musculus]       128     6e-32
   XP_036013148.1 protein phosphatase 1A isoform X1 [Mus musculus]       127     3e-31
  
    ```
  
  You can see that several protein phosphate isoforms from mouse proteins database match with protein phosphatase 1G (as expected)



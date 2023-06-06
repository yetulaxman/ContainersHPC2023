---
topic: docker
title: Tutorial2 -  Using local Docker images as HPC applications 
---

This tutorial explains how to build a singularity/Apptainer image on HPC systems from a local docker image. In reality, existing docker images may not be suitable for our purpose. In that case, we have to either modify an existing docker image or build a new one. Unfortunately, the docker-related operations can only be done on our local machines or any host machine where we have privileged root access. 

###  Expected outcome of this tutorial:
After this tutorial, you will learn to:
- Prepare a docker image and save it locally 
- Launch an Apptainer container from a local docker image 

### Converting a local docker image to Apptainer

1. Let's use [trimmomatic software](http://www.usadellab.org/cms/?page=trimmomatic), which is a flexible read trimming tool for Illumina NGS data, as available in [Quay Registry](https://quay.io). Visit the webpage of Quay registry and search for the trimmomatic image (using keyword: trimmomatic) on the top right hand corner of the page. You can find the trimmomatic images from different repositories/accounts. Pick the one under biocontainer repository (i.e., biocontainers/trimmomatic). And also search for different tags available for the image (hint: on the left side menu, click on *tags* icon) and pick one tag from the list. Once you managed to find a fully qualified URI (i.e., hostname/repository/imagename:tag) for docker image, you are ready for pulling the image. Go to <a href="http://labs.play-with-docker.com/" target="_blank"> PWD terminal</a> and run the following command (with your own tag) to pull trimmomatic image (here example is with tag: 0.32--hdfd78af_4):

   ```bash
    docker pull quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
   In a real-world use case, you may have to modify the image pulled from registries or even build new one on your local machine to meet your needs. 
  
2. Once an image is pulled or built on your local machine, the image is stored in local registry (usually at /var/lib/docker) on host machine. In order to find
   the stored docker images, use **docker images** command. 
  
   ```bash  
   docker images
   ```
   From the above command, you need to find an image ID (this is unique to you) of trimmomatic image to save it locally. 
  
3. Create a tarball of the Docker image (with image id as cc8b303fee58 which would be different for you)  using the **docker save** command as below:
  
   ```bash
   docker save cc8b303fee58 -o trimmomatic_image.tar  # in this case image_id is : cc8b303fee58
   ls -l  trimmomatic_image.tar  # to see the local copy of image tarball
   ```

4. Copy the image tarball from PWD to your **scratch* folder on Puhti 

   ```  
   scp trimmomatic_image.tar YOURCSCUSERNAME@puhti.csc.fi:/scratch/project_xxx/YOURCSCUSERNAME  # replace `project_xxxx` with a valid project number 
   ```

5. Build Apptainer image from the tarball. 
 
    ```bash
    cd /scratch/project_xxxx/YOURCSCUSERNAME  # replace `project_xxxx` with a valid project number 
    apptainer build local_trimmomatic_image.sif docker-archive://trimmomatic_image.tar
    ```
  
6. Launch Apptainer container and check if you can get a command-line help for trimmomatic software

    ```bash
   apptainer exec local_trimmomatic_image.sif trimmomatic --help
   ```

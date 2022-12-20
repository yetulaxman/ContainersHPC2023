---
topic: singularity
title: Tutorial4(bonus) -  Sharing images using registries
---

We have to sometimes prepare custom images to meet our needs either by building a new image from Dockerfile or adding missing software tools on pre-built images. And also, we usually want to re-use the customised image later by ourselves or share it with other collaborators. One way to share images with others is to use image registries such as DockerHub! In this session, you will learn to modify existing Docker image and share it with others via DockerHub. For this purpose, we will make use of [FastQC software](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) image from DockerHub as a starting image for modification. 

This exercise requires a workstation with docker client installed and thus can't be done in CSC HPC environment.  <a href="http://labs.play-with-docker.com/" target="_blank"> Use PWD terminal</a> instead.

## Sharing docker image via DcokerHub

1. Deploy FastQC Container

   Let's launch FastQC container interactively as it is easy for testing or development. For that, use *docker run* command with special flag **-it** so that the
   command attaches you with an interactive terminal inside the container as shown below:
   
    ```bash
    docker container run -it biocontainers/fastqc:v0.11.9_cv7 /bin/sh
    ```
    > Note: the flags -it are short for -i -t which are the short forms of --interactive (Keep STDIN open) and --tty (allocate a terminal).

2. Modify FastQC Container

   Now that you are inside the container, you can modify the image as you wish. Just for illustration, as FastQC container lacks vim editor, you can install **vim**
   inside the container as shown below:
  
   ```bash
   apt-get update && apt-get install -y vim
   ```
   use Ctrl + p then Ctrl + q on terminal to detach from container. 

3. Commit the modified changes to FastQC image

   Let's find out the **container id** corresponding to the *FastQC* container into which you have installed **vim** editor using  `docker ps -a` command on the
   terminal. Once we have container id, you are ready to commit the changes as shown below:

   ```bash
   docker ps -a
   docker commit <container id> fastqc-vim:test   # docker commit 6527b0394bdf  fastqc-vim:test
   ```
   
4. Push your image to your DockerHub repository
 
   Sharing an image *via* docker registry such as  DockerHub (the most popular image registry, hosting hundreds of thousands of images) is an efficient way of 
   sharing and managing your images. Once an image is in a docker (public) registry, anyone can pull it from there. However, this involves setting up an account in 
   DockerHub. If you don't have DockerHub account already, here are few steps you can do to set-up your account:
     - One can create an account on the DockerHub using instructions [here](https://hub.docker.com/account/signup/). After verifying your email you are ready to 
   go and upload your first docker image.
     - Click on Create Repository.
     - Choose a name  and a description for your repository and click Create.

   Once you have docker credentials in place, you are ready to push the image. But before pushing your docker image to DockerHub,  you just need to rename docker image
   to your namespace/account first using `docker tag` command as below:

    ```bash
     docker tag <image id> your-dockerhub-user-name/repo-name[:tag]   # find <image id> corresponding to repository, fastqc-vim  by typing `docker images` command
     on host machine. e.g., command: docker tag f689b999263b your-dockerhub-user-name/fastqc-vim:test 
    ```
    All images should be tagged with an appropriate prefix to repository name before pushing an image.

    Push your image finally as below:
 
     ```bash
      docker login # authenticate yourself using DockerHub credentials
      docker push your-dockerhub-user-name/repo-name[:tag]  # docker push your-dockerhub-user-name/fastqc-vim:test
      ```
    Once the push  to repository is successful, your image is now available for everyone to use. Go to your profile page on the DockerHub  to view  your new docker 
    image.

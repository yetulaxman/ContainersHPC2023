---
topic: containers
title: Tutorial3 - Hello-world Docker example
---

This short tutorial gives you a very brief idea on how docker containers are launched. Although we don't work with docker containers on HPC systems, we will be referring to docker images occasionally as we proceed. Docker is one of the most popular and matured container platforms and it is worth gaining a basic idea on Dockers which will be useful when working with singularity containers later.

## Learning Objectives
In this tutorial, you will learn to: 
- Launch an instance for Docker environment in [Play with Docker](https://labs.play-with-docker.com/)
- Spawn a hello-world docker container 
- Understand the mechanism behind running a docker container

## Launch an instance to run Docker commands using play-with-docker(PWD)

You need [DockerHub](https://hub.docker.com/) credentials to work with PWD. <a href="http://labs.play-with-docker.com/" target="_blank">In PWD webpage,</a> create a session for this tutorial using your DockerHub credentails. After successful login, click on **+ add new instance** on left side panel and then a terminal should appear on the right. 

## Spin up a hello-world Container 

Let’s start with a simple *hello-world* container from [Dockerhub](https://hub.docker.com) which is a docker registry for sharing images. Try running the following command on linux terminal:

```bash
docker run hello-world
```
When a `docker run` command is issued *via* commandline interface (CLI), docker client contacts Docker daemon to check whether an image named, "hello-world" exists locally. If the image is not found locally, docker daemon pulls "hello-world" official image from DockerHub. Once docker image is available locally, `docker run` command creates a new container from that image.

#### What is the terminal output from the above `docker run` command? #####

  ```bash
  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  78445dd45222: Pull complete
  Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
  Status: Downloaded newer image for hello-world:latest

 Hello from Docker!
  This message shows that your installation appears to be working correctly.
 ```
 
Note that `docker run` creates a container, executes the command in it and stops the container when it is done.

#### What will happen if you run the above command again? Did you observe any change in terminal output now?

Docker image has already been downloaded locally and therefore docker can run the container straight away.

#### What is the default image tag used in `hello-world` example?

By default, an image is pulled with `latest` tag if available. It is possible to pull an image with a specified tag and is actually a good practice to use specific tag name for reproducible research. Tag provides a version control-like mechanism for docker images.

Congratulations, you have run a “Hello-World” Docker successfully !!!


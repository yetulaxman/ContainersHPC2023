---
topic: containers
title: Tutorial1 - Custom Rstudio on Puhti
---
In this tutorial, you will learn how to:
   - Pull a Rstudio image from DokcerHub
   - Launch a Rstudio image on Puhti
   - Make custom installations of R packages



Start an interactive node as below and choose your project name upon the command prompt:

```bash
sinteractive -c 2 -m 4G -d 250

```
Build an apptainer image (singularity) for Rstudio as retrieved from  a docker registry (e.g., in this case, [DockerHub](https://hub.docker.com/)) as below:

```bash
# Navigate to the scratch area of your project before pulling an image from dockerhub
cd /scratch/project_xxxx/$USER 
apptainer pull --name rstudio_v430.sif docker://rocker/rstudio:4.3.0


# Please note that usually 'singularity exec -B ... image.sif ...' command is sufficient for most applications. 
# But rstudio being a complex GUI application in shared  environment like Puhti, we need to set 
# several settings, most of which are CSC-specific before launching Rstudio in start-srtudio-server.
# But for now, just change the name of image (tip: look for "rstudio.sif" and you should see it in two places) in the script,
# start-rstudio-server  and launch apptainer container from the terminal. 

# Download start script for rstudio
wget https://a3s.fi/biocontainers2023/start-rstudio-server
# update image name (i.e., use 'rstudio_v430.sif' in the place of 'rstudio.sif') in the start script and launch rstudio
chmod +x start-rstudio-server 
./start-rstudio-server 
```
Follow the instructions that appear on screen upon successfull launching of Rstudio if you have already set-up SSH keys on Puhti. If you don't have SSH keys already in place, follow the instructions below for  SSH port tunneling for login node first and then compute node:

```bash
ssh -L 8787:localhost:8888 <username>@puhti.csc.fi    # Issue this command while being on local machine                                                        
ssh -L 8888:localhost:container_port <username>@$HOSTNAME      # Issue this command on login node; $HOSTNAME is compute node attached to 
                                                               # interactive session change "container_port" number where rstudio is exposed on 
                                                               # compute node); 
 ```
Point your browser to http://localhost:8787. If successful, rstudio should appear now on the browser. You need credentials to login to rstudio. Copy and paste rstudio username and passpassword from the terminal where you have launched rstudio on Puhti.

#### How to install an R package on Puhti

You can now stop running rstudio on interactive node (control + c) and follow the instructions for installing an R package. Please note that your own package installations have to be R-version specific and should be installed on `/projappl` directory of your project.

Create a folder for installing R packages in `/projappl` (open a login node shell in the [Puhti web interface](https://www.puhti.csc.fi/) or log in to Puhti with SSH):

``` bash
# replace your actual project number and include specific version of R (in this case, <rversion> is: R430) in the directory name
# where you are going to install your custom R packages

mkdir -p /projappl/<project>/$USER
mkdir /projappl/<project>/$USER/project_rpackages_R430   
```

Launch R console from your apptainer container in an interactive shell session: 

``` r
# Navigate to the folder where you have apptainer image and launch R session inside of Rstudio container
singularity exec -B /projappl/  rstudio_v430.sif bash
R
# add installation path to .libpaths 
.libPaths(c("/projappl/<project>/$USER/project_rpackages_<rversion>", .libPaths())) 
# Assign `libpath` to point to this directory 
 libpath <- .libPaths()[1]
# For example, you can try installing a package called `ROCR` now
 install.packages("ROCR", lib = libpath)
# Exit out of R terminal and then the container

control +d  # exit out of R terminal
control +d  # exit out of container
```

Finished! The R package is now ready to be loaded and used. Try loading with `library(ROCR)`.

```r
# exit the container now and relaunch rstudio as below
start-rstudio-server # Follow the instructions as before to access rstudio from your web browser

```

💡 The package location is defined only for the current R session! R has to be reminded of the location at the start of every R session or script where you want to use the project-specific package by running this command again:

``` r
.libPaths(c("/projappl/<project>/project_rpackages_<rversion>", .libPaths()))   # replace <rversion> with exact version tag you have used
library(ROCR)
```

#### More information

💡 Read more about using R on Puhti in [r-env documentation](https://docs.csc.fi/apps/r-env/) in Docs CSC.

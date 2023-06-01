---
topic: containers
title: Tutorial1 - Custom Rstudio on Puhti
---
In this tutorial, you will learn how to:
   - Pull a Rstudio image from DokcerHub
   - Launch a Rstudio image on Puhti
   - Make custom installation of R packages



Start interactive node as below and choose your project name on prompt:

```bash
sinteractive -c 2 -m 4G -d 250

```
Build a singularity image (Apptainer) from  a docker registry (e.g., DockerHub) as below:

```bash
# Navigate to the scratch area of your project before pulling an image from dockerhub
cd /scratch/project_xxxx/$USER 
apptainer pull --name rstudio_v430.sif docker://rocker/rstudio:4.3.0


# Please note usually 'singularity exec -B ... image.sif ...'  is sufficient for most applications. 
# But rstudio being complex application with GUI in # shared  environment like Puhti, we need to set 
# several settings, most of which are CSC-specific before launching Rstudio in start-srtudio-server.
# But for now, just #change the name of image (tip: look for "/Full/path/rstudio.sif") in the script:
# start-rstudio-server  and launch singularity container from the terminal. Download start script for
# rstudio
wget https://a3s.fi/biocontainers2023/start-rstudio-server
# update image name (i.e., rstudio_v430.sif) in the following script and run
chmod +x start-rstudio-server 
./start-rstudio-server 
```
Follow the instructions that appear on screen upon successfull launching of Rstudio. If you don't have SSH keys already in place, follow the instructions below for  SSH port tunneling for login node first and then for compute node:

```bash
ssh -l <username> -L 8787:localhost:8888 puhti.csc.fi    # Issue this command while being on local machine                                                        
ssh -l <username>  -L 8888:localhost:container_port <username>@$HOSTNAME      # Issue this command on login node; $HOSTNAME is compute node attached to 
                                                                              # interactive session change "container_port" number where rstudio is exposed on 
                                                                              # compute node); 
                                                                
```

Point your browser to http://localhost:8787. If successful, rstudio should appear now on the browser. Copy and paste rstudio username and passpassword from the terminal where you have launched rstudio to open rstudio on your local browser

#### How to install an R package on Puhti

You can now stop the running rstudio on the interactive node (control + c) and follow the instructions for installa an R package. Please note that your own package installations have to be project specific,  R version specific and should installed on `/projappl` directory of your project

1.  Create a folder for your R packages in `/projappl` (open a login node shell in the [Puhti web interface](https://www.puhti.csc.fi/) or log in to Puhti with SSH):

``` bash
# replace your actual project number and include specific version of R (in this case, <rversion> is: R430) 
# with which you are going to install R packages

mkdir -p /projappl/<project>/$USER
mkdir /projappl/<project>/$USER/project_rpackages_R430   
```

2. Launch the R console in an interactive shell session from your singularity container 

``` r
singularity exec -B /projappl/  rstudio_v430.sif bash
R
.libPaths(c("/projappl/<project>/$USER/project_rpackages_<rversion>", .libPaths())) 
# Assign `libpath` to point to this directory 
 libpath <- .libPaths()[1]
# For example, you can try installing a package called `ROCR` now
 install.packages("ROCR", lib = libpath)
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

💡 Instead of installing a missing package for your own project, you can ask for a module-wide installation for all users by contacting [servicedesk\@csc.fi](servicedesk@csc.fi)

#### More information

💡 Read more about using R on Puhti in [r-env documentation](https://docs.csc.fi/apps/r-env/) in Docs CSC.

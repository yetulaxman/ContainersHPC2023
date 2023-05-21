---
topic: rstudio
title: Tutorial - Custom R notebooks on Puhti
---

```bash
# start interactive node as below and choose your project name on prompt
sinteractive -c 2 -m 4G -d 250

# Build a singularity image (Apptainer) from  a docker registry (e.g., DockerHub)

singularity pull --name rstudio_v430.sif docker://rocker/rstudio:4.3.0

# Launch singularity container from the terminal
# modify the name of the image in the script: start-rstudio-server 
# usually 'singularity exec -B ... image.sif ...'  is sufficient for most applications. But rstudio being complex, we need to set several seetings most of which #are CSC-specific before launching Rstudio in start-srtudio-server. But for now, just change the 

start-rstudio-server 
```

#follow the instructions that appear on screen upon successfull launching of rstudio. If you don't have SSH keys in place follow the isntructions here 
SSH port tunneling for login node first and then for compute node

```bash
ssh -l <username> -L 8787:localhost:8888 puhti-login1.csc.fi    # Issue this command while being on local machine                                                        
ssh -l <username>  -L 8888:localhost:container_port <username>@$HOSTNAME      # Issue this command on login node; $HOSTNAME is compute node attached to 
                                                                              # interactive session change "container_port" number where rstudio is exposed on 
                                                                              # compute node); 
                                                                
```

Point your browser to http://localhost:8787. If successful, rstudio should appear now on the browser. Copy and paste rstudio username and passpassword from the terminal where you have launched rstudio to open rstudio on your local browser

#### How to install an R package on Puhti

You can now stop the running rstudio on the interactive node (control + c) and follow the instructions for installa an R package. Please note that your own package installations have to be project specific,  R version specific and should installed on `/projappl` directory of your project

1.  Create a folder for your R packages in `/projappl` (open a login node shell in the [Puhti web interface](https://www.puhti.csc.fi/) or log in to Puhti with SSH):

``` bash
# First navigate to /projappl/<project>, then
mkdir project_rpackages_<rversion>
```

2.  Start an R session (launch RStudio in the [Puhti web interface](https://www.puhti.csc.fi/) or launch the R console in an interactive shell session: start the job with `sinteractive` -\> `module load r-env` -\> `start-r`). In R, add the folder you created above to the list of directories where R will look for packages:

``` r
.libPaths(c("/projappl/<project>/project_rpackages_<rversion>", .libPaths())) 
```

Assign `libpath` to point to this directory (not strictly necessary but can make life easier):

``` r
libpath <- .libPaths()[1]
```

3.  Install the package (again, defining `lib = libpath` to specify the installation location is not strictly necessary but recommended)

``` r
install.packages("packagename", lib = libpath)
```

For example, you can try installing a package called `ROCR` with

``` r
singularity exec -B /projappl/  rstudio_v430.sif bash
R
install.packages("ROCR", lib = libpath)
```

Finished! The R package is now ready to be loaded and used. Try loading `beepr` with `library(ROCR)`.

```r
# exit the container now and relaunch rstudio as below
start-rstudio-server # Follow the instructions as before to ass rstudio

```

💡 The package location is defined only for the current R session! R has to be reminded of the location at the start of every R session or script where you want to use the project-specific package by running this command again:

``` r
.libPaths(c("/projappl/<project>/project_rpackages_<rversion>", .libPaths())) 
library(ROCR)
```

💡 Instead of installing a missing package for your own project, you can ask for a module-wide installation for all users by contacting [servicedesk\@csc.fi](servicedesk@csc.fi)

#### More information

💡 Read more about using R on Puhti in [r-env documentation](https://docs.csc.fi/apps/r-env/) in Docs CSC.

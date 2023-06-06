---
topic: containers
title: Tutorial2 - Custom Jupyter notebooks on Puhti
---
In this tutorial, you will learn how to:
  - Install your custom Jupyter notebook in Puhti with Tykky wrapper tool
  - Launch the Jupyter notebook from Puhti Web Interface


## Accessing custom Jupyter notebooks *via* Puhti web interface 

A custom Jupyter notebook to meet the needs of your computing environment can
be provisioned at CSC through [Puhti web interface](https://www.puhti.csc.fi). Here are minimal instructions
for setting up your Jupyter notebook on Puhti supercomputer:

### Installing necessary computing environment to *projappl* directory using *tykky* wrapper tool

[Tykky](https://docs.csc.fi/computing/containers/tykky/) container wrapper installs your software tools/packages  as a container
image for improved performance metrics such as faster startup times, reduced I/O load, and fewer number of file creations on 
Lustre parallel filesystems. For the purpose of 
this tutorial, we use [NMRLipids course](https://www.helsinki.fi/en/researchgroups/biophysical-chemistry/nmrlipids-summer-school-2022) 
set up which needs installation of several [python packages](https://raw.githubusercontent.com/CSCfi/Puhti_gui_tutorial/master/env_nmr.yml) 
for Jupyter notebook. Please install all needed computing environment in the *projappl* directory.  

Install python packages for course using tykky wrapper as below:
```bash
# Navigate to the scratch area of your course project 
cd /scratch/project_xxxx/      # Make sure to replace the correct project number here

# As a starting point, you can download all necessary template files for the 
# customising course environment from allas object storage storage (as shown below). 
# The downloaded template files include a module environment file namely,  NMRLipids-course.lua 
#  where you can change the settings such as the absolute path for bin directory (where you would  
# have installed all the python packages using tykky), notebook path and  
# source path for downloading extra material

wget https://a3s.fi/CSC_training/Puhti_web_helper.tar.gz && tar -xavf Puhti_web_helper
cd Puhti_web_helper
module load tykky
mkdir -p /projappl/project_xxxx/$USER and mkdir -p /projappl/project_xxxx/$USER/NMRLipids  
# You can write all needed python packages in yaml as in env_nmr.yml for NMRLpids course and install with tykky
conda-containerize new --prefix /projappl/project_xxxx/$USER/NMRLipids  env_nmr.yml  #  Installation can take for a while
```
Tykky would install all needed packages (as listed in the file, *env_nmr.yml*) to the directory `/projappl/project_xxxx/$USER/NMRLipids`. 
Please note that you have to provide absolute path of **bin** directory  (which exists inside the folder of `/projappl/project_xxxx/$USER/NMRLipids`) in the ".lua" file

### Creating a custom environment in Puhti web interface

The template files for custom notebook environment have to be added under the directory `/projappl/project_xxxx/www_puhti_modules/` in order te be visible in Puhti web interface. The `www_puhti_modules` directory can be created if it does not exist.

The two files needed for setting up the course modules are:
   - a `<<course_name>>.lua` file that defines the module that sets up the Python environment. Only files containing the text Jupyter will be visible in the app.
   - a `<<course_name>>-resources.yml` that defines the default resources used for Jupyter.
  
In this NMRLipids course example, the above-mentioned two template files (`NMRLipids-course-resources.yml` and `NMRLipids-course.lua`) are available in the folder 'Puhti_web_helper' that you have downloaded from allas. so just modify cautiously for your own installation and place them in `www_puhti_modules` folder as below:

```bash

# Make sure to use correct project number (in two places in NMRLipids-course.lua file) and your CSC username in the the copied files in
# /projappl/project_xxxx/www_puhti_modules.

mkdir -p /projappl/project_xxxx/www_puhti_modules && cp NMRLipids-course-resources.yml NMRLipids-course.lua /projappl/project_xxxx/www_puhti_modules

```
As the name of the ".lua" file will appear as custom notebook environment on Puhti web inaterface, please use some unique name  for .lua and .yaml file 
(e.g., add your CSC user name in the file name e.g., NMRLipids-course-yourcscusername.lua) in this course environment.

```
cd /projappl/project_xxxx/www_puhti_modules
# add your real CSC username in .lua and .yaml file names
mv  NMRLipids-course-resources.yml  NMRLipids-course-yourcscusername-resources.yml
mv  NMRLipids-course.lua  NMRLipids-course-yourcscusername.lua 
```

### Access custom notebook via Puhti web interface

1. Log in to [Puhti web interface](https://www.puhti.csc.fi)
2. Login with CSC/HAKA/VIRTU credentials (Users should have accepted Puhti service in [myCSC](https://my.csc.fi/welcome) 
   page under a course (or own) project before using this service). 
3. Once login is successful, select "Interactive Sessions" on the top menu bar and then click "Jupyter for courses". 
 On the right-hand side you can see the different fields for filling in before launching as a batch job. For this course, select 
 the "Project" and "Working directory" corresponding to course project. Then you will be able to see "NMRLipids-course-yourcscusername" 
 module under the "Course module" field. You can then launch Jupyter notebook which will be launched in the 
 interactive partition by default. You can also change the default settings by checking "Show custom resource settings".
4. Upon successful launching a job, you can click on "Connect to Jupyter" to see the course notebook corresponding to 
  your course environment.

###  Useful CSC documentation

- [Jupyter for course](https://docs.csc.fi/computing/webinterface/jupyter-for-courses/)
- [Tykky containerisation](https://docs.csc.fi/computing/containers/tykky/)




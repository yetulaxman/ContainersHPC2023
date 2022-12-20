---
title: Containers and Workflows in Bioinformatics
author: CSC Training
---

# Course material for _Containers and Workflows in Bioinformatics_ 

{% assign items = site.hands-on |  sort: "title" %}


## 1.  Introduction to CSC HPC environment and containers
### 1.1 [Slides:Introduction to CSC HPC environment](https://a3s.fi/containers-workflows/CSC_HPC_Environment.html)
### 1.2 [Slides:Fundamentals of containers](https://a3s.fi/containers-workflows/Introduction_to_Containers.html)
### 1.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2. Using Pre-existing Images for Bioapplications
### 2.1 [Slides: Using container images in HPC environment](https://a3s.fi/containers-workflows/Containers_in_HPC_environment.html)
### 2.2 [Slides:Containerised bio applications](https://a3s.fi/containers-workflows/bioapplications.html)
### 2.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'bioapplications' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 3. Running Singularity on HPC Environment 
### 3.1 [Slides: Converting docker images to singularity images](https://a3s.fi/containers-workflows/docker2singularity.html)
### 3.2 [Slides: Building singularity container images](https://a3s.fi/containers-workflows/Building_Singularity_Containers.html)
### 3.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'singularity' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 4. Nextflow on HPC 
### 4.1 [Slides: Introduction to workflows with nextflow](https://a3s.fi/containers-workflows/Intro_workflows.pdf)
### 4.2 [Slides: Workflows with singularity containers](https://a3s.fi/containers-workflows/workflow_singularity_containers.pdf)
### 4.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'nextflow' %}
[{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 5. Snakemake on HPC 
### 5.1 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'snakemake' %}
[{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## Information
<p></p>

<p>
  <div style="float: left; width: 50%;">
   <img src="./slides/img/EuroCC_Logo_invert.png" width=110 align=middle/>
   <p><small>
     This project has received funding from the European High-Performance Computing Joint Undertaking (JU) under grant agreement No 951732.
      </small>
    </p>
  </div>
  <div style="float: right; width: 50%;">
    <img src="https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-sa.png" width=180>
    <p><small>
  All material (C) 2020-2021 by CSC -IT Center for Science Ltd.  <br />
  This work is licensed under a <strong>Creative Commons Attribution-ShareAlike</strong> 3.0 <br />
  Unported License, <a href="http://creativecommons.org/licenses/by-sa/4.0/">http://creativecommons.org/licenses/by-sa/4.0/</a>
      </small>
    </p>
  </div>
</p>
<p>&nbsp;</p>
   

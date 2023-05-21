---
title: Containers in Supercomputing Environment
author: CSC Training
---

{% assign items = site.hands-on |  sort: "title" %}


## 2.1 Introduction to docker containers 
### Slides: Introduction to docker container
###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2.2 Using pre-existing docker images in HPC environement
### [Slides: Converting docker images to singularity images](https://a3s.fi/containers-workflows/docker2singularity.html)
### Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'conversion' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2.3 Containerized bioapplications
###  [Slides:Containerised bio applications](https://a3s.fi/containers-workflows/bioapplications.html)
### Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'bioapplications' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2.4  Containerised Applications
### [Slides: R and Jupyter notebooks as containers](https://a3s.fi/CSC_training/Notebooks.html)
### [Slides: R and Jupyter notebooks as containers](https://a3s.fi/CSC_training/workflows_throughput.html)
###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

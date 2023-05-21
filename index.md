---
title: Containers in Supercomputing Environment
author: CSC Training
---

{% assign items = site.hands-on |  sort: "title" %}


## 3. Working with Docker Containers 
### Slides: [Basic introduction to Docker containers](https://a3s.fi/biocontainers2023/intro_docker.html)
### Slides: [Conversion of Docker images to Apptainer images](https://a3s.fi/biocontainers2023/docker2singularity.html)

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'docker' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 4. Containerised Applications
### [Slides: R and Jupyter notebooks as containers](https://a3s.fi/CSC_training/Notebooks.html)
### [Slides:  High-throughput computing and workflows](https://a3s.fi/CSC_training/workflows_throughput.html)
###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

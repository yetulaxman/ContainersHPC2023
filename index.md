---
title: Bioinformatic Analysis with Containers
author: CSC Training
---

# Course material for _Bioinformatics analysis with containers_ 

{% assign items = site.hands-on |  sort: "title" %}


## 2.1 Introduction to docker containers 

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2.2 Using pre-existing docker containers in HPC environement

### Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'conversion' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2.3 Containerized bioapplications

### Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'bioapplications' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2.4 Workflows at CSC

###  Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'bioapplications' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}


## Information
<p></p>

<p>
  <div style="float: left; width: 50%;">
   <img src="./EuroCC_Logo_invert.png" width=110 align=middle/>
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
   

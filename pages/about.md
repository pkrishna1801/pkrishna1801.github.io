---
layout: page
title: About
permalink: /about/
weight: 3
---

# **About Me**

Hi I am **{{ site.author.name }}** :wave:,<br>
I build machine learning systems that go the full distance, from research-grade modeling to production deployment.
Right now I'm applying ML to computational drug discovery at Alkermes, where I've built end-to-end pipelines for compound prioritization and shipped them as fast, containerized APIs running on HPC infrastructure.
Before that, I trained a text diffusion model on 8× H100 GPUs for my master's research at Northeastern, where I also taught a graduate cloud computing course as a TA.
Earlier, I spent two years as a data engineer at Societe Generale processing 1M+ daily financial transactions, built RESTful microservices as an intern at Capgemini, and got my start optimizing embedded signal-processing code at PathPartner Technology.
What I bring: <br>
🔹 ML modeling: diffusion models, GNNs, deep learning (PyTorch) <br>
🔹 MLOps & deployment: Docker, Kubernetes, AWS, Terraform, FastAPI <br>
🔹 Data engineering: Spark, Airflow, Kafka, Elasticsearch <br>
I'm most useful in the gap between a model that works in a notebook and one that works in the real world. <br>


<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Machine Learning skills" source=site.data.ML-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>
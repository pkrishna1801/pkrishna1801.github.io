---
layout: page
title: Bio
permalink: /about/
weight: 1
---

<div class="bio-text" markdown="1">

# **Bio**

Hi, I am **{{ site.author.name }}** :wave:

I'm a Machine Learning Engineer with a background spanning drug discovery, life sciences, fintech, generative AI, medical imaging, and production ML deployment. Most recently, at Alkermes, I built Quantitative Structure–Activity Relationship (QSAR) models on in-house assay data, then turned them into a FastAPI service, an MCP server that AI agents can call as tools, and a dashboard that three teams use every day. Getting a model into someone's daily workflow is a different problem than getting it to converge, and it's the part I enjoy most.

Before that, I spent a year and a half training diffusion and transformer-based generative models for my master's thesis at Northeastern, running experiments across a cluster of H100 GPUs. The most useful thing I took away was learning how to debug and scale generative models when the usual tricks stop working. I started my career at Société Générale, building ETL pipelines and Airflow workflows that financial systems depend on. That grounding in ML production infrastructure is why my models hold up once they leave a Jupyter notebook.

On the modeling side I work with PyTorch, diffusion models, and GNNs. On the deployment side I use Docker, Kubernetes, AWS, Azure, and Terraform, with Spark and Airflow handling the data itself. I'm drawn to problems where the system either works in the real world or it doesn't, and where getting it there takes genuine engineering depth.

Outside of work: several hundred hours in Factorio and counting. (Yes, it's also a production pipeline problem.)

</div>

{% include about/skills.html %}

<div class="row">
{% include about/timeline.html %}
</div>

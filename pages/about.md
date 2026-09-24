---
layout: page
title: Bio
permalink: /about/
weight: 1
---

<div class="bio-grid">
<div class="bio-text" markdown="1">

# **Bio**

Hi, I am **{{ site.author.name }}** :wave:

I'm most useful in the gap between a model that works in a notebook and one that works in the real world.

Most recently I built the end-to-end ML lifecycle at Alkermes, from data pipelines and model training to fast, containerized APIs on HPC infrastructure. Before that I trained a text diffusion model on 8× H100s for my master's research at Northeastern, and taught a graduate cloud computing course as a TA.

Earlier: two years as a software engineer at Société Générale building data and ML systems that processed 1M+ daily financial transactions, microservices at Capgemini, and my start optimizing embedded signal-processing code at PathPartner.

Apart from the technical stuff, you can talk to me about photography, Factorio, and Haikyuu.

</div>

<aside class="bio-facts">
  <dl>
    <dt>Most recent</dt>
    <dd>Machine Learning Intern, Alkermes · 2026</dd>
    <dt>Education</dt>
    <dd>M.S., Northeastern University · GPA 3.84</dd>
    <dt>Based in</dt>
    <dd>Boston, MA</dd>
    <dt>Focus</dt>
    <dd>Generative AI · LLMs &amp; agents · Production ML</dd>
    <dt>Open to</dt>
    <dd>ML Engineer, Applied Scientist, and Forward Deployed Engineer roles</dd>
  </dl>
  <div class="bio-facts-actions">
    <a href="{{ '/assets/Resume.pdf' | relative_url }}" class="btn btn-primary no-underline" download="Prarthana_Krishnamurthy_Resume.pdf"><i class="fas fa-download mr-2"></i>Resume</a>
    <a href="mailto:{{ site.author.email }}" class="no-underline" aria-label="Email"><i class="fas fa-envelope"></i></a>
    <a href="https://linkedin.com/in/{{ site.author.linkedin }}" class="no-underline" target="_blank" rel="noopener" aria-label="LinkedIn"><i class="fab fa-linkedin-in"></i></a>
    <a href="https://github.com/{{ site.author.github }}" class="no-underline" target="_blank" rel="noopener" aria-label="GitHub"><i class="fab fa-github"></i></a>
  </div>
</aside>
</div>

{% include about/skills.html %}

<div class="row">
{% include about/timeline.html %}
</div>

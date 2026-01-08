---
layout: default
permalink: /
---

<!-- {% include landing.html %} -->


<div class="row justify-content-center align-items-center p-4">
  <div class="col-lg-8 text-center">
    <h1 class="display-3 font-weight-bold mb-3">Hi, I'm Prarthana Krishnamurthy</h1>
    <h2 class="lead mb-4">ML Engineer & AI Researcher</h2>
    <p class="lead text-muted mb-5">
      Specializing in transforming complex research concepts into scalable, production-ready AI solutions. 
      I build intelligent systems at the intersection of deep learning, healthcare, and molecular modeling.
    </p>
    
    <!-- <div class="mb-5">
      <a href="{{ site.baseurl }}/projects" class="btn btn-primary btn-lg mx-2 mb-2">View Projects</a>
      <a href="{{ site.baseurl }}/about" class="btn btn-outline-primary btn-lg mx-2 mb-2">About Me</a>
    </div> -->
  </div>
</div>

<!-- What I Do Section -->
<div class="row mt-5 mb-5">
  <div class="col-12 text-center mb-4">
    <h2 class="font-weight-bold">What I Do</h2>
  </div>
</div>

<div class="row mb-5">
  <div class="col-lg-4 col-md-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body text-center">
        <div class="mb-3" style="font-size: 3rem;">🏥</div>
        <h5 class="card-title font-weight-bold">Medical Imaging</h5>
        <p class="card-text">Creating diffusion models and deep learning solutions for improved diagnostic visualization and analysis</p>
      </div>
    </div>
  </div>
  
  <div class="col-lg-4 col-md-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body text-center">
        <div class="mb-3" style="font-size: 3rem;">🧬</div>
        <h5 class="card-title font-weight-bold">Molecular Modeling</h5>
        <p class="card-text">Implementing graph neural networks for molecular property prediction and drug discovery applications</p>
      </div>
    </div>
  </div>
  
  <div class="col-lg-4 col-md-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body text-center">
        <div class="mb-3" style="font-size: 3rem;">🤖</div>
        <h5 class="card-title font-weight-bold">RAG Systems</h5>
        <p class="card-text">Building retrieval-augmented generation systems for more reliable and context-aware AI applications</p>
      </div>
    </div>
  </div>
</div>

<!-- Featured Projects -->
<div class="row mt-5 mb-4">
  <div class="col-12 text-center mb-4">
    <h2 class="font-weight-bold">Featured Projects</h2>
    <p class="text-muted">Explore some of my recent work in AI and machine learning</p>
  </div>
</div>

<div class="row mb-5">
  <div class="col-lg-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body">
        <h5 class="card-title font-weight-bold">🤖 Agentic HR Assistant</h5>
        <p class="card-text">Intelligent HR assistant leveraging RAG and LLM agents for automated query resolution and document processing</p>
        <a href="{{ site.baseurl }}/projects" class="btn btn-sm btn-outline-primary">Learn More →</a>
      </div>
    </div>
  </div>
  
  <div class="col-lg-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body">
        <h5 class="card-title font-weight-bold">🧠 MRI Synthesis</h5>
        <p class="card-text">Advanced diffusion models for medical image synthesis and enhancement</p>
        <a href="{{ site.baseurl }}/projects" class="btn btn-sm btn-outline-primary">Learn More →</a>
      </div>
    </div>
  </div>
  
  <div class="col-lg-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body">
        <h5 class="card-title font-weight-bold">🔬 Molecular Property Prediction</h5>
        <p class="card-text">Graph neural networks for predicting molecular properties in drug discovery</p>
        <a href="{{ site.baseurl }}/projects" class="btn btn-sm btn-outline-primary">Learn More →</a>
      </div>
    </div>
  </div>
  
  <div class="col-lg-6 mb-4">
    <div class="card h-100 shadow-sm">
      <div class="card-body">
        <h5 class="card-title font-weight-bold">🛒 Product Recommendation Engine</h5>
        <p class="card-text">Production-ready recommendation system using collaborative filtering and deep learning</p>
        <a href="{{ site.baseurl }}/projects" class="btn btn-sm btn-outline-primary">Learn More →</a>
      </div>
    </div>
  </div>
</div>

<!-- Background -->
<div class="row mt-5 mb-5 bg-light p-5">
  <div class="col-lg-8 mx-auto text-center">
    <h2 class="font-weight-bold mb-4">My Journey</h2>
    <p class="lead">
      Currently pursuing M.S. in Computer Software Engineering at <strong>Northeastern University</strong>, 
      I blend academic research with industry experience from <strong>Societe Generale</strong>, 
      where I built cloud-native systems processing over 1M daily transactions.
    </p>
    <p class="lead">
      My passion lies in making AI systems that are not just intelligent, but also interpretable, 
      ethical, and aligned with human needs.
    </p>
    <a href="{{ site.baseurl }}/about" class="btn btn-primary mt-3">Read My Full Story →</a>
  </div>
</div>

<!-- Call to Action -->
<div class="row mt-5 mb-5">
  <div class="col-12 text-center">
    <h3 class="font-weight-bold mb-3">Let's Connect</h3>
    <p class="lead text-muted mb-4">Interested in collaborating or discussing AI projects?</p>
    <div>
      {% if site.author.email %}
      <a href="mailto:{{ site.author.email }}" class="btn btn-outline-dark mx-2 mb-2">
        <i class="fas fa-envelope"></i> Email Me
      </a>
      {% endif %}
      {% if site.author.linkedin %}
      <a href="https://www.linkedin.com/in/{{ site.author.linkedin }}" target="_blank" class="btn btn-outline-dark mx-2 mb-2">
        <i class="fab fa-linkedin"></i> LinkedIn
      </a>
      {% endif %}
      {% if site.author.github %}
      <a href="https://github.com/{{ site.author.github }}" target="_blank" class="btn btn-outline-dark mx-2 mb-2">
        <i class="fab fa-github"></i> GitHub
      </a>
      {% endif %}
    </div>
  </div>
</div>
---
title: What is Diffusion?
tags: [Diffusion Models, Deep Learning, Research]
style: fill
color: info
description: Notes on diffusion models, from the core intuition to a real bidirectional MRI synthesis pipeline.
---

Diffusion models learn to generate data by learning to undo noise. That single idea — train a network to reverse a gradual corruption process — turned out to be general enough to power state-of-the-art image generators, and, as I found out firsthand, medical image synthesis and even text generation.

## The core idea

Take a clean image (or embedding, or anything continuous) and add a little Gaussian noise to it. Then add a little more. Repeat a few hundred or a few thousand times, and you end up with pure noise. That's the *forward process*, and it's fixed — no learning involved.

The interesting part is the *reverse process*: training a neural network to predict, at each noise level, what was just removed. Chain enough of these small denoising steps together starting from pure noise, and you get a sample from the data distribution. The network never has to generate the whole thing in one shot — it just has to be good at the much easier local problem of "slightly less noisy version of this."

## Why this matters beyond images

Most people meet diffusion models through image generation, but the forward/reverse noising framework doesn't care what the underlying data looks like, as long as it's continuous. That's what made it useful in two very different projects I've worked on:

**Bidirectional MRI synthesis.** In clinical practice, a patient scan often only includes one MRI modality (say, T1-weighted) when a diagnosis would benefit from another (T2-weighted) — and re-scanning is expensive and slow. I built a timestep-aware DDPM that learns the reverse process conditioned on the *other* modality, using cross-attention in the U-Net so the model can pull in relevant structure from the source scan at every denoising step instead of just concatenating inputs. That cross-attention piece mattered a lot — it's what let the model fuse modalities instead of just learning a blurry average. The model reached a PSNR of 23.8 and SSIM of 0.81 translating T1 to T2 and back.

**Non-autoregressive text generation.** For my master's thesis, I trained diffusion models directly in BERT embedding space to generate text non-autoregressively — instead of predicting one token at a time left-to-right, the whole sequence gets denoised in parallel. This is a much harder setting than images: embeddings are high-dimensional and the "correct" output has to decode back to coherent language, not just look plausible. Getting this to converge well cut validation loss by 92.3% and got semantic similarity (BERT score) to 0.93.

## The unglamorous part: sampling

Training a diffusion model is only half the problem — sampling from it naively takes hundreds or thousands of network evaluations, which is far too slow for anything real. I used DPM-Solver++, a faster ODE-based sampler that treats the reverse process as solving a differential equation rather than a long Markov chain, which let me cut the number of sampling steps dramatically while keeping discretization error down to 0.65. Getting sampler choice right ended up mattering as much as the architecture itself — a great model with a bad sampler still produces garbage or takes forever.

Scaling the text model also meant getting the infrastructure right: I trained across 8x NVIDIA H100 GPUs using PyTorch FSDP and NCCL optimization, sustaining 95%+ GPU utilization. None of that shows up in a diagram of the model, but it's the difference between a model that trains in a reasonable time and one that doesn't train at all.

## The takeaway

Diffusion models are a genuinely general tool: the same forward-noise/reverse-denoise scaffolding works whether you're translating between MRI modalities or generating text in embedding space. The interesting engineering problems aren't really about the noise schedule — they're about how you condition the model on the information you actually have (cross-attention for modality fusion), and how you sample from it fast enough to be useful (DPM-Solver++).

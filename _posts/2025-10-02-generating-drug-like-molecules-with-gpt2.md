---
title: Generating Drug-Like Molecules with GPT-2
tags: [Drug Discovery, GPT-2, Molecular Modeling]
style: fill
color: success
description: Treating molecules as strings, and conditioning a language model on the properties you actually want.
---

The first time you learn that molecules can be represented as strings, it feels like a trick. SMILES notation (Simplified Molecular Input Line Entry System) encodes a molecule's atoms, bonds, and ring structure as a compact sequence of characters. The same benzene ring is just `c1ccccc1`. Once a molecule *is* a string, a whole toolbox built for sequences becomes available, including language models. That's the premise behind fine-tuning GPT-2 to generate novel drug-like molecules.

## Why generate molecules instead of just predicting properties

Property prediction (given a molecule, estimate its toxicity, solubility, permeability, and so on) is the more common use of ML in drug discovery, and it's valuable, but it only screens candidates someone already proposed. Generation flips the problem: instead of scoring an existing list of candidates, propose new ones directly, conditioned on the properties you want. That's a meaningfully different design space to search, especially when the "obvious" candidates in a class have already been tried.

## Conditioning on properties, not just structure

An unconditional generative model for SMILES will happily produce valid-looking molecules, but "valid" and "useful" are very different bars. The interesting part of this project was conditioning generation on *target biological properties*, so the model isn't just producing plausible chemistry, it's producing chemistry that's more likely to have the characteristics a drug candidate actually needs. That means the training setup has to pair each molecule with property labels and bake that conditioning into the generation process, rather than treating property prediction and generation as two disconnected models.

## Why GPT-2, and why it's harder than it sounds

GPT-2 is a decoder-only autoregressive transformer built for natural language, and SMILES strings are not natural language: the vocabulary is small (atoms, bonds, ring-closure digits, branch parentheses) but the syntax is unforgiving. A single wrong character can produce an invalid molecule or a chemically nonsensical one, in a way that a slightly-off sentence in English usually still parses fine. That makes token-level accuracy matter more here than in typical text generation, and it's why validity rate (the fraction of generated strings that correspond to real, parseable molecules) is as important a metric as the property scores themselves.

## The unglamorous bottleneck: descriptor calculation

Generating candidate molecules is the visible part of the pipeline, but scoring them (computing the molecular descriptors used to evaluate whether a generated candidate actually has the target properties) is where the real compute goes at scale, since it has to run on every candidate the model proposes, not just the ones that make it to a final shortlist. I parallelized descriptor calculation across 16 cores, which cut that step's runtime by 20x and turned generate-then-filter from a batch job you check on the next day into something you can iterate on the same afternoon.

## The takeaway

Representing molecules as strings is what makes this tractable with off-the-shelf language modeling tools, but the actual value is in the conditioning (generating toward a target property profile instead of generating anything valid) and in making the evaluation loop (descriptor calculation) fast enough that you can actually iterate on the generative model instead of waiting on it.

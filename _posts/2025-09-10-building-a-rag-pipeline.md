---
title: "Building a RAG Pipeline: Lexical Search Meets Dense Retrieval"
tags: [RAG, LLMs, Retrieval]
style: fill
color: primary
description: Why I combined BM25 and dense retrieval instead of picking one, in an end-to-end RAG web app.
---

Retrieval-augmented generation exists to solve a specific failure mode: LLMs answer confidently even when they don't actually know something, because they're generating the statistically likely next token, not looking anything up. RAG fixes this by giving the model retrieved passages to ground its answer in, instead of relying purely on what it memorized during training. I built an end-to-end RAG web app on Mistral-7B to explore how far that idea goes when the retrieval step is taken seriously instead of treated as an afterthought.

## Why not just use embeddings?

The default RAG recipe is: embed your documents, embed the query, do a nearest-neighbor search, stuff the results into the prompt. It works, but dense embeddings have a specific blind spot — they're good at semantic similarity and bad at exact terms. Ask about a specific product SKU, an error code, or an uncommon proper noun, and a dense retriever will often return something *topically* related instead of the passage that actually contains that exact string, because the embedding space compresses rare tokens toward their semantic neighborhood.

Lexical search (BM25) has the opposite profile: it's built on term frequency, so it's excellent at exact and rare-term matches but blind to paraphrasing — ask the same question with different words and it can miss a passage that answers it perfectly.

Neither is strictly better, so I used both: BM25 for lexical matching and FAISS-based dense retrieval (with MiniLM-L6-v2 embeddings) for semantic matching, merging results from each before handing candidates to the generator.

## The pipeline

1. **Ingestion:** documents get chunked and indexed twice — once into a BM25 index for lexical search, once into a FAISS vector index via MiniLM-L6-v2 embeddings for dense search.
2. **Retrieval:** an incoming query hits both indexes in parallel. Each returns its own top-k candidates with its own scoring scale, which is the annoying part — BM25 scores and cosine similarity scores aren't comparable, so merging them properly means normalizing or reranking rather than just concatenating.
3. **Generation:** Mistral-7B gets the merged, deduplicated set of passages plus the original question, and generates an answer grounded in what was retrieved.

## Why Mistral-7B specifically

For a RAG system, the generator's job is narrower than in open-ended chat — it mostly needs to synthesize and summarize retrieved text faithfully, not generate novel knowledge from parameters. That's a task a 7B open-weight model handles well, and self-hosting it means the whole pipeline — retrieval and generation — can run without a per-query bill to a hosted API, which matters once you're iterating on retrieval quality and re-running the same queries repeatedly during development.

## What actually moved the needle

The generator was rarely the bottleneck. When answers were wrong or unhelpful, it was almost always because retrieval handed it the wrong passages — either BM25 missing a paraphrased question, or dense retrieval missing an exact term. Hybrid retrieval directly targets that failure mode, and chunking strategy (how documents get split before indexing) turned out to matter almost as much: chunks too large dilute relevance scores, chunks too small lose the surrounding context the generator needs to actually answer the question.

The lesson that generalized past this one project: in RAG, the generator gets the credit but the retriever does most of the actual work.

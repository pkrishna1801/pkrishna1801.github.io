---
title: How to Train Your Dragon (LLM) for Product Recommendation
tags: [LLMs, GPT-4, Recommendation Systems]
style: fill
color: warning
description: Building a product recommendation engine on GPT-4 instead of collaborative filtering.
---

Most recommendation engines are built on collaborative filtering: find users who behave like you, recommend what they bought. It works, but it has a well-known cold-start problem — a new user or a new product has no history to filter on, so the system has nothing to go on. I wanted to see how far an LLM-based approach could get instead.

## The pitch: reasoning instead of history

Collaborative filtering recommends by pattern-matching against past behavior. An LLM can instead *reason* about a user's stated preferences and browsing behavior in natural language, and generalize to products it has never seen a purchase history for. There's no training data to collect, no retraining pipeline to run every time the catalog changes — the model's general world knowledge about products and categories does a lot of the work that a traditional system would need thousands of interactions to learn.

I built a product recommendation engine around GPT-4 that takes a user's preferences and recent browsing behavior and generates personalized suggestions directly, rather than scoring a candidate set produced by a separate retrieval step.

## Architecture

The system is a Flask backend that owns the recommendation logic, with a React frontend for the actual browsing experience:

- **Backend (Flask):** turns a user's preference profile and browsing history into a structured prompt, calls GPT-4, and parses the response into a clean list of recommendations with reasoning the frontend can display.
- **Frontend (React):** the interactive product browsing UI — this is what actually captures the signals (what a user looks at, what they say they want) that feed the backend.
- **Deployment:** backend on Heroku, frontend on Vercel, so the two scale and deploy independently.

Splitting it this way meant the prompt design and recommendation logic could iterate quickly on the backend without touching the frontend at all, which mattered because most of the actual work in a project like this is in the prompt, not the API glue.

## What's different from a traditional pipeline

The biggest shift is where the "intelligence" lives. A classic system has an offline training step (fit a model on interaction data) and a lightweight online scoring step. Here, all of the reasoning happens online, at request time, inside the prompt — which means:

- **No retraining when the catalog changes.** Add a new product category tomorrow and the system can reason about it immediately, using GPT-4's existing knowledge of what that category means to people.
- **Explanations come for free.** Because the model reasons in natural language, it's straightforward to have it justify *why* it's recommending something, which a collaborative filtering score can't do on its own.
- **Latency and cost move from training-time to request-time.** There's no offline compute bill, but every recommendation now costs an API call — a real tradeoff against a traditional model that's cheap to serve once trained.

## What I'd watch for

An LLM-based recommender is not a drop-in replacement for collaborative filtering at scale — it trades an offline training cost for a per-request one, and it's only as good as the preference and behavior signals you feed into the prompt. For a personalization surface where explanations matter and the catalog changes faster than you can retrain a model, though, it's a genuinely different and useful tool, not just a novelty.

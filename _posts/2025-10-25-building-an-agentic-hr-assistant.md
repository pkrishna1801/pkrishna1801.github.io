---
title: Building an Agentic HR Assistant with LangGraph
tags: [Agents, LangGraph, LLMs]
style: fill
color: secondary
description: What changes when an LLM app needs to reason across steps and remember context, not just answer one prompt.
---

A single well-crafted prompt gets you a long way with LLM applications, right up until the task actually requires more than one step. Hiring workflows are a good example: "should this candidate move to the next round" isn't a single question, it's a sequence — pull their application, check it against role requirements, compare with other candidates in the pipeline, and only then produce a recommendation. That's the gap agentic frameworks like LangGraph are built for, and it's what I used to build an HR assistant that handles hiring tasks end-to-end instead of answering isolated questions.

## Why a graph instead of a chain

The simplest way to compose LLM steps is a fixed chain — step one's output feeds step two, always in that order. That works until the task needs a decision: if the candidate's application is incomplete, ask for more information instead of proceeding to evaluation; if a hiring manager's follow-up question needs information from three steps ago, retrieve it instead of starting over. LangGraph models the workflow as a graph rather than a straight line, so the assistant can branch, loop back, or reroute to a different node based on what it finds — the control flow is explicit and inspectable instead of buried in one long prompt trying to handle every case at once.

## Context memory is what makes it feel coherent

The other piece that matters as much as the graph structure is persistent context memory across the conversation. Hiring tasks aren't one-shot — a hiring manager asks a question, gets an answer, then asks a follow-up that only makes sense in light of the first exchange ("of the candidates you just mentioned, which have the strongest systems background?"). Without shared memory across steps, each turn would need to re-establish context from scratch, which is both wasteful and fragile. With it, the assistant can reason step-by-step over the whole conversation instead of treating every message as an isolated query.

## What this buys you over a single prompt

- **Decomposition:** the model handles "check requirements," "compare candidates," and "draft a recommendation" as separate, debuggable steps instead of one prompt trying to do all three at once and silently skipping parts.
- **Recoverability:** if one step's output looks wrong (an incomplete application, an ambiguous requirement match), the graph can route to a clarification step instead of confidently hallucinating a resolution.
- **Automation of the boring parts:** the actual value for a hiring workflow isn't a chatbot — it's automating the repetitive parts of the process (checking requirements, summarizing candidates) so a human only has to review a well-structured recommendation instead of doing that work from scratch.

## The tradeoff

None of this is free — a graph with branches and memory is meaningfully harder to reason about and debug than a single prompt, and it's easy to over-engineer a workflow into a graph that's more complex than the task actually requires. The rule of thumb I came away with: reach for a chain first, and only reach for a graph when the task genuinely needs a decision the model has to make about what to do next, not just what to say next.

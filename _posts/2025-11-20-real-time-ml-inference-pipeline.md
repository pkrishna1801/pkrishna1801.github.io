---
title: A Real-Time ML Inference Pipeline on AWS
tags: [MLOps, AWS, Streaming]
style: fill
color: dark
description: Kinesis, Flink, and Lambda, wired together with Terraform, for ML predictions that can't wait for a batch job.
---

Most ML pipelines are batch jobs: collect data, run inference on a schedule, write results somewhere downstream. That's fine until a prediction is only useful within seconds of the event that triggered it — fraud scoring, live personalization, anomaly detection. I built a [real-time ML inference pipeline](https://github.com/pkrishna1801/ML-Streaming-Pipeline) on AWS, provisioned entirely through Terraform, for exactly that case.

## The shape of the pipeline

Events flow from Elasticsearch into a Kinesis stream, get enriched by an Apache Flink job, land in a second Kinesis stream, and are picked up by a Lambda function that calls an ML REST API and writes the prediction to DynamoDB. Splitting enrichment (Flink) from inference (Lambda) into two stages rather than one monolithic consumer keeps them independently scalable — Flink handles the stateful, always-on feature computation, while Lambda handles the bursty, stateless "call an API and write a row" work.

## Why Flink does the enrichment

Raw events aren't useful to a model on their own — a timestamp needs to become hour-of-day and day-of-week, a zip code needs to become a region, a user ID needs to become a behavioral segment tag. Flink computes these features in-stream, backed by an ElastiCache Redis layer for sub-millisecond lookups (zip-to-region, segment data) that would be far too slow to hit a primary datastore for on every single event. By the time a record reaches Lambda, it's already feature-complete — Lambda's only job is schema validation, the model call, and the write.

## Designing for failure, not just the happy path

A streaming pipeline that only works when nothing goes wrong isn't production-ready. A few things carry the load here:

- **An SQS dead-letter queue** catches records that fail processing after retries, so a bad record gets quarantined for inspection instead of silently dropped or stuck retrying forever.
- **CloudWatch alarms on stream lag**, not just errors — a producer running faster than Flink can consume, or Flink running faster than Lambda can drain, is a real-time pipeline's most common failure mode, and it's invisible if you're only watching for exceptions.
- **Private subnets and least-privilege IAM** throughout, with ML API credentials in Secrets Manager rather than plaintext environment variables — the pipeline touches production data end-to-end, so it's built to the same security bar as anything else that does.

## The takeaway

The interesting engineering here isn't the ML model — it's everything around it. Feature computation has to happen fast enough to not become the bottleneck, failures have to be caught and quarantined rather than silently swallowed, and the whole thing has to be reproducible enough to tear down and rebuild with a single `terraform apply`. That's what actually makes "real-time" a property of the system, not just a description of the model.

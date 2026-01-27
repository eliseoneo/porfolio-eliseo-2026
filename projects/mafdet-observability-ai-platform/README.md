# Mafdet: Observability + AI Analytics Platform (Architecture)

## Summary
A platform architecture that unifies metrics, logs, and operational datasets with AI-driven analysis layers. Designed for incremental rollout (start with Kafka, expand to Postgres and microservices), with emphasis on reproducibility and operational usefulness.

## Problem
- Metrics and logs exist, but insights are fragmented and incident response is inconsistent.
- Teams need a single approach to: collect → structure → analyze → summarize → act.

## Solution Overview (Spec)
- **Signal Layer**
  - Metrics: Prometheus (+ Thanos for retention/federation where needed)
  - Logs: Loki with normalization + taxonomy to reduce noise
- **Dataset Layer**
  - Runtime extraction pipelines producing Parquet datasets (partitioned, versioned schema)
  - Join-ready datasets for multi-signal analysis (metrics + logs + events)
- **Analysis Layer**
  - Statistical + ML anomaly detection on structured features
  - Clustering for recurring incident families
  - Optional graph modeling for meaning-based relationships (service ↔ symptom ↔ cause)
- **Narrative Layer**
  - LLM summarization for incident timelines and weekly operational reporting
  - RAG assistant grounded in runbooks and component docs

## Deliverables
- Platform blueprint (components + data flow)
- Taxonomy and schema conventions (logs/events/datasets)
- Incremental rollout plan (Kafka first → Postgres → services)

## Tech
Prometheus, Grafana, Loki, Thanos, Kafka, PostgreSQL, Parquet, Python, vector DB (optional), graph DB (optional)
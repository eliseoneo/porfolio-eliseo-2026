# Kafka Observability + MCP-Driven Diagnostics (Docker Swarm)

## Summary
Kafka environment deployed on **Docker Swarm**, instrumented via **JMX → Prometheus** and correlated with **Loki logs**. Goal: reduce time-to-diagnosis by linking broker/cluster signals to actionable explanations.

## Problem
- Metrics show symptoms (CPU/GC/ISR/lag), while logs contain context but are high-noise.
- Manual correlation across brokers is slow and inconsistent.

## Constraints
- Multi-broker environment (Swarm)
- Must support incremental rollout (start with Kafka, extend to Postgres/services later)
- Emphasis on **repeatable diagnostics** (not one-off investigations)

## Solution Overview (Spec)
- **Metrics**: JMX exporter → Prometheus → Grafana dashboards (cluster + broker views)
- **Logs**: broker/container logs centralized in Loki; event normalization by severity + event type
- **Correlation Layer**:
  - map log event classes to metric “states” (e.g., ISR shrink/expand, controller churn, GC pressure)
  - produce structured incident notes (what changed, likely causes, where to verify)
- **MCP scripts (conceptual)**:
  - broker-aware log pulls and metric snapshots for targeted time windows
  - outputs prepared for downstream dataset generation (see Logs→Parquet project)

## Deliverables
- Dashboard set: broker health, replication/ISR, consumer lag, JVM/GC
- PromQL library (recording/alerts-ready patterns)
- Log event taxonomy (Kafka warning/error categories)
- Correlation rules (symptom → hypothesis → verification signals)

## Tech
Kafka (KRaft), Docker Swarm, Prometheus, Grafana, Loki, JMX exporter, Python utilities

## Risks & Mitigations
- **False correlations** → keep rules explainable, include verification steps
- **Log noise** → normalize + categorize; focus on high-value event families
- **Scale** → broker-scoped pulls, time-window filtering, dataset batching

## Next Steps
- Automate periodic snapshots (10–30 min) into Parquet datasets
- Add anomaly scoring (trend change, burst detection, multi-signal triggers)
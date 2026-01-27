# Runtime Log Adapter: Docker Logs → Parquet (Operational Datasets)

## Summary
A scheduled pipeline that converts container runtime logs into **structured Parquet** datasets suitable for analytics and ML.

## Problem
- Logs are semi-structured and high-volume.
- Analytics needs consistent fields, partitions, and stable schemas.

## Solution Overview (Spec)
- **Sources**: `docker logs` or equivalent driver output (per container/broker)
- **Parsing**:
  - normalize timestamp, severity, component, message
  - extract Kafka/Postgres-specific fields (request type, partition, broker id, error class)
- **Enrichment**:
  - service, cluster_id, broker_id, environment, event_type
  - correlation keys (topic/partition, consumer group, request_id when available)
- **Storage**:
  - Parquet partitioning: `service=.../date=YYYY-MM-DD/hour=HH/`
  - schema versioning for safe evolution

## Deliverables
- Canonical schema (v1) + evolution rules
- Partitioning strategy for fast queries
- Sample synthetic dataset generation for demos (no real logs published)

## Tech
Python, pyarrow/parquet, regex/structured parsers, Docker runtime

## Next Steps
- Feature store fields for anomaly detection
- Join with metrics snapshots (Prometheus instant queries) for multi-modal analysis
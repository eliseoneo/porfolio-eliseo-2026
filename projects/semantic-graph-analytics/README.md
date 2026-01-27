# Semantic Graph Analytics for Email/Calendar Workload

## Summary
A semantic modeling approach for analyzing communication/workload (emails/calendar events) using clustering + meaning-based graph relations to produce KPI rollups and deviation signals.

## Problem
- Entity-only extraction misses “why” and “type of work”.
- Need a structure that supports business questions (load drivers, deviations, bottlenecks).

## Solution Overview (Spec)
- Canonical event fields (source, time, title/description, status)
- Semantic labeling:
  - `reason_semantic` category (meaning)
  - cluster subtypes (within each reason)
- Rollups:
  - weekly KPIs per cluster (volume, duration, deviation)
- Optional graph layer:
  - nodes: reason/cluster/team/project
  - edges: contributes_to, blocks, depends_on (meaning-first)

## Deliverables
- Data schema spec (events + semantic labels + rollups)
- KPI definitions and aggregation rules
- Query examples (what drives meeting load, where deviations increase)

## Tech
Python, Parquet, clustering, graph concepts (Neo4j optional)

## Next Steps
- Train similarity models on synthetic datasets to validate methodology
- Integrate summarization model to generate weekly narrative reports
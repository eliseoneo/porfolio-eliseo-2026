# Eliseo — Project Portfolio (2026)

Backend / Platform engineer focused on distributed systems, observability, and applied ML/LLM pipelines.
This repository is a **public portfolio**: short, client-friendly specifications of selected projects.
Implementation details and client-sensitive material remain private.

## Focus Areas
- **Observability platforms**: Prometheus, Grafana, Loki, Thanos, exporters (JMX/node/windows)
- **Distributed systems**: Kafka (KRaft), PostgreSQL HA, container orchestration (Swarm/Kubernetes)
- **Data pipelines**: runtime logs → structured datasets (Parquet) → analytics / anomaly detection
- **AI/ML systems**: RAG (Qdrant), local inference (llama.cpp), fine-tuning workflows (LoRA/QLoRA)
- **Computer vision**: detection + segmentation pipelines and benchmarking (Colab/GPU)

---

## Selected Projects

1) **Kafka Observability + MCP-Driven Diagnostics (Docker Swarm)**  
   Spec: [`projects/kafka-observability-mcp/README.md`](projects/kafka-observability-mcp/README.md)

2) **Runtime Log Adapter: Docker Logs → Parquet (Operational Datasets)**  
   Spec: [`projects/logs-to-parquet/README.md`](projects/logs-to-parquet/README.md)

3) **RAG Ops Assistant (Qdrant + llama.cpp)**  
   Spec: [`projects/rag-ops-assistant/README.md`](projects/rag-ops-assistant/README.md)

4) **OCR → Section Extraction → MongoDB → Proposal PDF (FastAPI)**  
   Spec: [`projects/ocr-to-proposal-pdf/README.md`](projects/ocr-to-proposal-pdf/README.md)

5) **Food / Ingredient Detection (CV: Detection + Segmentation)**  
   Spec: [`projects/cv-ingredient-detection/README.md`](projects/cv-ingredient-detection/README.md)

6) **Semantic Graph Analytics for Email/Calendar Workload**  
   Spec: [`projects/semantic-graph-analytics/README.md`](projects/semantic-graph-analytics/README.md)

7) **Anomaly Detection: Time Series + Logs**  
   Spec: [`projects/anomaly-detection-time-series-logs/README.md`](projects/anomaly-detection-time-series-logs/README.md)

8) **Mafdet Platform Architecture (Observability + AI Analytics)**  
   Spec: [`projects/mafdet-observability-ai-platform/README.md`](projects/mafdet-observability-ai-platform/README.md)

9) **MLflow Tracking: SQLite → MySQL Migration**  
   Spec: [`projects/mlflow-tracking-mysql-migration/README.md`](projects/mlflow-tracking-mysql-migration/README.md)

10) **PDF Alteration Detection with Autoencoders (Research Spec)**  
   Spec: [`projects/pdf-alteration-detection-autoencoders/README.md`](projects/pdf-alteration-detection-autoencoders/README.md)      

---

## Engagement
If you’re a client reviewing this portfolio:
- This repo intentionally avoids client identifiers, real logs, credentials, and production endpoints.
- I can provide a **private technical walkthrough**, sanitized samples, or a **private reference implementation** under NDA.

## Contact
- GitHub: https://github.com/eliseoneo
- LinkedIn: (add link)
- Email: (add email)



## About / Services

I help teams design, build, and operate **reliable distributed systems** and **observability + AI analytics pipelines**. My work is focused on turning runtime signals (metrics/logs/traces/docs) into **actionable diagnostics**, **operational datasets**, and **automation** that reduces incident time and improves system performance.

### What I do
- **Observability Architecture & Delivery**
  - Prometheus/Grafana/Loki/Thanos design, dashboards, recording rules, alert strategy
  - Signal design: SLIs/SLOs, golden signals, capacity/perf indicators
- **Kafka & Streaming Platforms**
  - Broker/cluster health, replication/ISR stability, consumer lag investigation
  - Performance tuning and failure-mode analysis (GC pressure, disk I/O, controller churn)
- **Operational Data Pipelines**
  - Runtime logs → normalization → enrichment → **Parquet datasets**
  - Feature engineering for anomaly detection and trend analysis
- **Applied AI for Operations**
  - RAG assistants for infra docs/runbooks (Qdrant + local inference via llama.cpp)
  - LLM-assisted summarization for incidents and weekly operational reporting
- **Backend APIs for Automation**
  - FastAPI services for ingestion, extraction, reporting, and generation workflows

### Outcomes you can expect
- Faster root-cause workflows (less “log hunting”, more structured signals)
- Better visibility into Kafka and critical services (cluster state + early warnings)
- Portable datasets for analytics/ML (Parquet + clear schemas + retention strategy)
- Practical automation around recurring operational questions and diagnostics
- Clear documentation and reproducible delivery (runbooks, specs, templates)

### How I work (engagement model)
1) **Discovery (short)**: current architecture, pain points, success metrics  
2) **Design**: target signals + dashboards + data model (spec + diagram)  
3) **Build & Integrate**: pipelines, dashboards, alerts, APIs, automation  
4) **Hardening**: testing, tuning, security/sanitization, handoff documentation  
5) **Knowledge transfer**: runbooks + operational playbooks

### Typical deliverables
- Grafana dashboards (JSON exports) + PromQL libraries (recording rules)
- Loki parsing/normalization strategy + event taxonomy
- Log-to-Parquet pipeline spec + schema + partitioning plan
- RAG ingestion/retrieval spec + Qdrant payload schema + prompt templates
- FastAPI endpoints specs + sample payloads + deployment notes

### Confidentiality
This repository is intentionally **spec-level** and sanitized. I do not publish client identifiers, real logs, credentials, or production endpoints. Detailed implementation and private demos can be shared under NDA.

Test push without SSH key.
Another test.




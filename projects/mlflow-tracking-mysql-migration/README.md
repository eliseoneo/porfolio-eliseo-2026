# MLflow Tracking: SQLite → MySQL Migration (Reproducible ML Experiments)

## Summary
Migrated an MLflow tracking deployment from the default SQLite backend to MySQL to improve reliability, concurrency, and reproducibility of experiment tracking in multi-run environments.

## Problem
- SQLite is convenient for local testing but becomes a limitation for:
  - concurrent runs (locking/contention)
  - long-term durability and operational management
  - shared environments and team workflows
- Need a stable tracking backend to support iterative ML experimentation and artifact lineage.

## Solution Overview (Spec)
- Stand up a MySQL instance (containerized or managed)
- Configure MLflow Tracking Server to use:
  - **MySQL** as backend store (experiments/runs/metrics/params metadata)
  - a separate artifact store (file system or object storage, depending on environment)
- Validate migration by:
  - creating new experiments and runs
  - logging parameters/metrics/artifacts
  - verifying UI consistency and queryability

## Deliverables
- Deployment specification (tracking server + DB backend)
- Configuration documentation (environment variables and connection patterns)
- Validation checklist for migration correctness
- Operational notes (backup, retention, and basic hardening recommendations)

## Tech
MLflow, MySQL, Python, Docker (optional), environment-based configuration

## Impact
- Improved stability for repeated/parallel experiment runs
- Clearer experiment lineage and easier long-term tracking
- Better readiness for team/shared usage and production-like workflows

## Next Steps
- Add authentication / access control (depending on environment)
- Define retention and backup strategy for both backend store and artifacts
- Integrate with CI to validate experiment logging on pipeline changes
# Anomaly Detection Research: Time Series + Logs

## Summary
A set of applied experiments and implementation patterns for detecting anomalies in operational telemetry (logs and time-series metrics), with emphasis on practicality: clear rules, explainable signals, and measurable operational value.

## Problem
- Infrastructure signals are noisy and non-stationary (seasonality, drift, bursts).
- Pure thresholding misses subtle degradations; pure ML can be hard to explain/operate.
- Need methods that are robust, lightweight, and suitable for incremental deployment.

## Solution Overview (Spec)
- **Time-series preparation**
  - Stationarity checks and transformations (detrending / differencing where needed)
  - Feature engineering for change detection and stability monitoring
- **Hybrid anomaly approach**
  - Classical ML baseline: One-Class SVM (novelty detection) for “shape” anomalies
  - Robust statistical guardrails: Tukey fences / quantile-based rules for spikes/outliers
  - Temporal constraints: rolling-window rules (e.g., “spike > threshold within 1 hour”)
- **Operationalization direction**
  - Convert detections into labeled events
  - Store as Parquet for post-analysis and model iteration
  - Optional forecasting track: evaluate forecast residuals as anomaly signals

## Deliverables
- Methodology specification (what to use when, trade-offs)
- Feature set definitions (time window, aggregation, volatility, burst metrics)
- Detection outputs schema (event type, confidence, affected signal, time window)

## Tech
Python, classical ML, time-series methods, Parquet datasets for iteration

## Next Steps
- Add forecasting-based residual detection (e.g., Chronos-style approach) as an option
- Build evaluation harness: precision/recall on labeled incidents + drift tracking
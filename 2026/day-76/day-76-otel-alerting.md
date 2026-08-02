# Day 76 – OpenTelemetry and Alerting

## What I Learned

Today I completed the third pillar of observability by learning OpenTelemetry and implementing alerting with Prometheus and Grafana.

---

# OpenTelemetry

Learned:

- Vendor-neutral observability framework
- Collects Metrics, Logs and Traces
- Uses OTLP protocol
- Sends telemetry to different backends

---

# OTEL Collector

Configured:

- OTLP Receiver
- Batch Processor
- Prometheus Exporter
- Debug Exporter

Verified:

- Collector running successfully
- Prometheus scraping collector metrics
- Debug exporter displaying traces

---

# OTLP

Learned:

- OTLP gRPC (4317)
- OTLP HTTP (4318)

Sent:

- Sample Trace
- Sample Metrics

Verified:

- Trace received by OTEL Collector
- Metrics exported to Prometheus

---

# Prometheus Alert Rules

Created alerts for:

- High CPU Usage
- High Memory Usage
- High Disk Usage
- Container Down
- Target Down

Learned:

- Alert Expressions
- Labels
- Annotations
- Pending State
- Firing State

---

# Grafana Alerting

Configured:

- Contact Point
- Notification Policy
- Alert Rule

Verified:

- Alert created successfully
- Alert state visible inside Grafana

---

# Observability Architecture

Metrics Pipeline

Node Exporter

↓

Prometheus

↓

Grafana

↓

Alerts

---

Logs Pipeline

Docker Containers

↓

Promtail

↓

Loki

↓

Grafana

---

Traces Pipeline

Application / curl

↓

OTEL Collector

↓

Debug Exporter

(Future: Jaeger / Tempo)

---

# Services Running

- Prometheus
- Grafana
- Loki
- Promtail
- Node Exporter
- cAdvisor
- OTEL Collector
- Notes App

---

# Important Files

- docker-compose.yml
- prometheus.yml
- otel-collector-config.yml
- alert-rules.yml

---

# Important Commands

docker compose up -d

docker compose ps

docker logs otel-collector

curl -X POST http://localhost:4318/v1/traces

curl -X POST http://localhost:4318/v1/metrics

docker compose stop notes-app

docker compose start notes-app

---

# Key Learning

Today I learned how OpenTelemetry collects telemetry data using receivers, processors, and exporters. I also understood how Prometheus and Grafana use alerting to automatically detect system issues, completing all three pillars of observability—metrics, logs, and traces.

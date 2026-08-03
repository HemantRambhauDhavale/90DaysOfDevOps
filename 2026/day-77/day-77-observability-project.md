# Day 77 – Full Observability Project

## What I Learned

Today I completed a production-style observability project by combining metrics, logs, traces, dashboards, and alerting into one complete monitoring stack.

---

# Complete Observability Stack

Built and deployed:

- Prometheus
- Node Exporter
- cAdvisor
- Grafana
- Loki
- Promtail
- OpenTelemetry Collector
- Notes Application

All services were managed using Docker Compose.

---

# Metrics Pipeline

Validated metrics from:

- Prometheus
- Node Exporter
- cAdvisor
- OTEL Collector

Practiced PromQL queries for:

- CPU Usage
- Memory Usage
- Disk Usage
- Container CPU
- Container Memory

Verified:

- All scrape targets were UP
- Metrics collected successfully

---

# Logs Pipeline

Configured:

- Loki
- Promtail

Practiced LogQL queries for:

- Container Logs
- Error Logs
- Application Logs
- Log Volume

Verified:

- Docker logs collected successfully
- Logs visible inside Grafana Explore

---

# Traces Pipeline

Used:

- OpenTelemetry Collector

Learned:

- Receivers
- Processors
- Exporters
- OTLP Protocol

Verified:

- Sample traces received
- Trace data visible in collector logs

---

# Grafana Dashboard

Created a unified dashboard showing:

- CPU Usage
- Memory Usage
- Disk Usage
- Container CPU
- Container Memory
- Application Logs
- Target Status

Verified:

- Dashboard displayed metrics and logs together

---

# Project Comparison

Compared my project with the reference repository:

- docker-compose.yml
- prometheus.yml
- loki-config.yml
- promtail-config.yml
- otel-collector-config.yml
- Grafana Datasources

Learned how production projects organize monitoring configurations.

---

# Architecture

Docker Containers

↓

Promtail → Loki → Grafana

↓

Node Exporter → Prometheus → Grafana

↓

cAdvisor → Prometheus → Grafana

↓

OTEL Collector → Prometheus / Debug Output

↓

Unified Grafana Dashboard

---

# Verification

Verified:

- All services running successfully
- Prometheus Targets UP
- Metrics collected
- Logs collected
- Traces received
- Dashboard working correctly

---

# Observability Journey Summary

| Day | Topics |
|------|--------|
| Day 73 | Observability, Prometheus, PromQL |
| Day 74 | Node Exporter, cAdvisor, Grafana |
| Day 75 | Loki, Promtail, LogQL |
| Day 76 | OpenTelemetry, Alerting |
| Day 77 | Complete Observability Project |

---

# Key Learning

Building the complete observability stack helped me understand how metrics, logs, and traces work together to monitor applications. Combining Prometheus, Grafana, Loki, Promtail, OpenTelemetry Collector, Node Exporter, and cAdvisor provided a practical understanding of production-grade monitoring using open-source tools.

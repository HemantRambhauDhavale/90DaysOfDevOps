# Day 73 – Introduction to Observability and Prometheus

## What I Learned

Today I started learning Observability and Prometheus.

---

# Observability vs Monitoring

Monitoring:

- Detects issues
- Sends alerts
- Shows system health

Observability:

- Helps identify root cause
- Uses Metrics, Logs and Traces
- Provides better troubleshooting

---

# Three Pillars of Observability

## Metrics

Examples:

- CPU Usage
- Memory Usage
- Request Count

Tool:

- Prometheus

---

## Logs

Examples:

- Application Logs
- Error Logs
- System Logs

---

## Traces

Shows:

- Complete request flow
- Service communication
- Request latency

---

# Prometheus Setup

Created:

- prometheus.yml
- docker-compose.yml

Started Prometheus using:

```bash
docker compose up -d

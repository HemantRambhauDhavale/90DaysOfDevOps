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

# Verified

Verified:

- Prometheus Dashboard accessible
- Prometheus running successfully
- Prometheus configuration loaded correctly
- Docker containers running without errors

---

# Scrape Targets

Configured:

- Prometheus Self Monitoring
- Sample Application Target

Verified:

- Prometheus Target → UP
- Notes App Target → UP

---

# PromQL Queries

Practiced:

```promql
up
```

```promql
process_resident_memory_bytes
```

```promql
prometheus_http_requests_total
```

```promql
rate(prometheus_http_requests_total[5m])
```

```promql
count({__name__=~".+"})
```

Learned how PromQL helps query real-time metrics collected by Prometheus.

---

# Counter vs Gauge

### Counter

Only increases over time.

Examples:

- Total HTTP Requests
- Login Count
- API Requests

### Gauge

Can increase or decrease.

Examples:

- CPU Usage
- Memory Usage
- Active Users
- Disk Usage

---

# Prometheus Storage (TSDB)

Learned:

- Prometheus stores metrics inside its built-in Time Series Database (TSDB).
- Default data retention is **15 days**.
- Docker Volume Mounting keeps metrics safe even if the container restarts.
- Retention can be configured based on time or storage size.

---

# Verification

Verified:

- Prometheus Dashboard accessible
- Targets showing **UP**
- PromQL queries returning live data
- Metrics collected successfully
- Sample application monitored by Prometheus

---

# Key Learning

Today I learned:

- Observability
- Monitoring vs Observability
- Three Pillars of Observability
- Prometheus Architecture
- Docker Compose
- Scrape Targets
- PromQL Basics
- Counter vs Gauge
- Prometheus TSDB
- Metrics Collection

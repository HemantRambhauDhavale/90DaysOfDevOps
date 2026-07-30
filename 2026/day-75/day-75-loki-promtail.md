# Day 75 – Log Management with Loki and Promtail

## What I Learned

Today I extended my observability stack by adding centralized log management using Grafana Loki and Promtail.

---

# Logging Pipeline

Docker Containers

↓

Promtail

↓

Loki

↓

Grafana

---

# Loki

Configured:

- Loki Server
- Log Storage Backend
- Docker Compose Integration

Verified:

- Loki running successfully
- Ready endpoint working

---

# Promtail

Configured:

- Docker Log Collection
- Log Shipping
- Container Log Discovery
- Docker Log Parsing

Verified:

- Container logs collected
- Logs sent to Loki successfully

---

# Grafana Integration

Added:

- Loki Datasource
- Grafana Explore
- Log Visualization

Verified:

- Logs visible inside Grafana
- Metrics and Logs available together

---

# LogQL

Practiced:

- View all logs
- Filter logs by container
- Search logs using keywords
- Count log lines
- Rate of logs

---

# Metrics and Logs Correlation

Learned:

- Analyze metrics and logs together
- Faster troubleshooting
- Better incident investigation
- Single dashboard for monitoring

---

# Loki vs ELK

Loki:

- Indexes labels only
- Lightweight
- Lower storage usage
- Easy to maintain

ELK:

- Full-text indexing
- Powerful search
- Higher resource usage
- Better for advanced log analytics

---

# Important Commands

docker compose up -d

docker compose ps

curl http://localhost:3100/ready

docker compose restart grafana

docker compose logs promtail

docker compose logs loki

---

# Key Learning

Today I learned how Loki and Promtail complete the second pillar of observability by collecting Docker logs, storing them efficiently, and making them searchable in Grafana using LogQL. Combining metrics and logs in one platform makes troubleshooting much faster and closer to real-world DevOps practices.

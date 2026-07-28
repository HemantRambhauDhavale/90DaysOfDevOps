# Day 74 – Node Exporter, cAdvisor and Grafana Dashboards

## What I Learned

Today I expanded my observability stack by adding Node Exporter, cAdvisor, and Grafana.

---

# Node Exporter

Installed:

- Node Exporter

Collected:

- CPU Usage
- Memory Usage
- Disk Usage
- Network Statistics
- Filesystem Information

Verified:

- Node Exporter running successfully
- Metrics available on Port 9100
- Prometheus scraping Node Exporter
- Target status showing UP

---

# cAdvisor

Installed:

- cAdvisor

Collected:

- Container CPU Usage
- Container Memory Usage
- Container Network Usage
- Container Filesystem Usage

Verified:

- cAdvisor running successfully
- Docker containers detected
- Metrics available in Prometheus
- Target status showing UP

---

# Grafana

Installed:

- Grafana

Configured:

- Prometheus Datasource
- Dashboard
- Dashboard Panels

Created:

- CPU Usage Panel
- Memory Usage Panel
- Disk Usage Panel
- Container CPU Panel
- Container Memory Panel

Verified:

- Grafana Dashboard accessible
- Datasource connected successfully
- Live metrics displayed

---

# Datasource Provisioning

Created:

- datasources.yml

Benefits:

- Automatic datasource creation
- Repeatable configuration
- No manual setup required
- Production-friendly deployment

Verified:

- Prometheus datasource automatically configured

---

# Community Dashboards

Imported:

- Dashboard ID 1860 (Node Exporter Full)
- Dashboard ID 193 (Docker Monitoring)

Verified:

- CPU Monitoring
- Memory Monitoring
- Disk Monitoring
- Network Monitoring
- Container Monitoring

---

# PromQL Queries

Practiced:

node_cpu_seconds_total

node_memory_MemAvailable_bytes

container_memory_usage_bytes

rate(container_cpu_usage_seconds_total{name!=""}[5m])

topk(3, container_memory_usage_bytes{name!=""})

---

# Node Exporter vs cAdvisor

Node Exporter:

Monitors:

- Host Machine

Examples:

- CPU
- Memory
- Disk
- Network

cAdvisor:

Monitors:

- Docker Containers

Examples:

- Container CPU
- Container Memory
- Container Network
- Container Filesystem

---

# Verification

Verified:

- Prometheus running successfully
- Node Exporter running
- cAdvisor running
- Grafana Dashboard accessible
- Prometheus datasource connected
- All scrape targets showing UP
- Community dashboards imported successfully
- Host and container metrics collected successfully

---

# Key Learning

Today I learned:

- Node Exporter
- cAdvisor
- Grafana
- Prometheus Datasource
- Dashboard Creation
- Datasource Provisioning
- Community Dashboards
- Host Monitoring
- Container Monitoring
- PromQL

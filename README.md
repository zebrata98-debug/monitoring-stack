# 📊 Monitoring Stack

A production-style observability stack built with **Prometheus**, **Grafana**, and **Node Exporter**, deployed via Docker Compose. Monitors host-level system metrics (CPU, memory, disk, network) with real alerting rules and a full Grafana dashboard.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                    Docker Network                    │
│                                                      │
│   ┌───────────────┐     scrapes      ┌────────────┐ │
│   │  Prometheus   │ ───────────────► │   Node     │ │
│   │   :9090       │                  │  Exporter  │ │
│   └───────┬───────┘                  │   :9100    │ │
│           │                          └────────────┘ │
│           │ data source                             │
│           ▼                                         │
│   ┌───────────────┐                                 │
│   │    Grafana    │                                 │
│   │    :3000      │                                 │
│   └───────────────┘                                 │
└─────────────────────────────────────────────────────┘
```

- **Prometheus** — scrapes and stores time-series metrics every 15 seconds
- **Node Exporter** — exposes host-level hardware and OS metrics
- **Grafana** — visualises metrics with dashboards and supports alerting

---

## 📁 Project Structure

```
monitoring-stack/
├── docker-compose.yml        # Defines all three services and shared network
├── prometheus/
│   ├── prometheus.yml        # Scrape config and alertmanager settings
│   └── alert.rules.yml       # Alerting rules for CPU, memory, and disk
└── README.md
```

---

## ⚙️ Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed
- [Docker Compose](https://docs.docker.com/compose/install/) installed
- Ports `9090`, `9100`, and `3000` available on your host

---

## 🚀 Getting Started

**1. Clone the repository**

```bash
git clone https://github.com/zebrata98-debug/monitoring-stack.git
cd monitoring-stack
```

**2. Start the stack**

```bash
docker compose up -d
```

**3. Verify all containers are running**

```bash
docker compose ps
```

You should see `running` status for `prometheus`, `grafana`, and `node-exporter`.

**4. Access the services**

| Service | URL | Credentials |
|---------|-----|-------------|
| Prometheus | http://localhost:9090 | — |
| Node Exporter | http://localhost:9100/metrics | — |
| Grafana | http://localhost:3000 | admin / admin123 |

---

## 📈 Setting Up the Grafana Dashboard

1. Log into Grafana at `http://localhost:3000`
2. Go to **Connections → Data sources → Add data source → Prometheus**
3. Set the URL to `http://prometheus:9090`
4. Click **Save & Test**
5. Go to **Dashboards → Import**
6. Enter dashboard ID `1860` (Node Exporter Full)
7. Select your Prometheus data source and click **Import**

---

## 🚨 Alert Rules

Three alerting rules are configured in `prometheus/alert.rules.yml`:

| Alert | Condition | Severity | Duration |
|-------|-----------|----------|----------|
| HighCPUUsage | CPU usage > 80% | Warning | 2 minutes |
| HighMemoryUsage | Memory usage > 85% | Warning | 2 minutes |
| LowDiskSpace | Disk free < 15% | Critical | 5 minutes |

View active alerts at: `http://localhost:9090/alerts`

---

## 🛠️ Useful Commands

```bash
# Start the stack in the background
docker compose up -d

# Stop the stack
docker compose down

# View logs for a specific service
docker compose logs prometheus
docker compose logs grafana

# Reload Prometheus config without restarting
curl -X POST http://localhost:9090/-/reload
```

---

## 📌 Notes

- Metric data is persisted in Docker named volumes (`prometheus_data`, `grafana_data`) and survives container restarts
- Prometheus retains 15 days of metrics by default (configurable in `docker-compose.yml`)
- Change the default Grafana password before deploying in any non-local environment

---

## 🧰 Tech Stack

![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat&logo=grafana&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)

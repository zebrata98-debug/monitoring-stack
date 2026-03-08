# monitoring-stack

A local observability stack using Prometheus, Grafana and Node Exporter running in Docker. Built this to learn how monitoring works in practice.

---


---

## Project layout

```
monitoring-stack/
├── docker-compose.yml
├── prometheus/
│   ├── prometheus.yml       # scrape config
│   └── alert.rules.yml      # alert rules
└── README.md
```

---

## Requirements

- Docker
- Docker Compose
- Ports 9090, 9100 and 3000 free on your machine

---

## How to run it

Clone the repo and start everything:

```bash
git clone https://github.com/zebrata98-debug/monitoring-stack.git
cd monitoring-stack
docker compose up -d
```

Check that all three containers are up:

```bash
docker compose ps
```

Then open these in your browser:

| Service | URL |
|---------|-----|
| Prometheus | http://localhost:9090 |
| Node Exporter metrics | http://localhost:9100/metrics |
| Grafana | http://localhost:3000 |

Grafana login is `admin` / `admin123` — change this if you're running it anywhere other than locally.

---

## Setting up Grafana

1. Go to Connections → Data sources → Add data source → Prometheus
2. Set the URL to `http://prometheus:9090` (use the service name, not localhost)
3. Save & Test — should say data source is working
4. Go to Dashboards → Import → enter ID `1860` → select your data source → Import

That gives you the Node Exporter Full dashboard with CPU, memory, disk and network all in one place.

---

## Alert rules

I set up three basic alerts in `prometheus/alert.rules.yml`:

- CPU above 80% for 2 minutes → warning
- Memory above 85% for 2 minutes → warning
- Disk space below 15% for 5 minutes → critical

You can check them at `http://localhost:9090/alerts`

---

## Handy commands

```bash
# stop everything
docker compose down

# check logs
docker compose logs prometheus
docker compose logs grafana

# reload prometheus config without restarting
curl -X POST http://localhost:9090/-/reload
```

---

## Notes

Data is stored in Docker named volumes so it survives restarts. Prometheus keeps 15 days of metrics by default, that can be changed in the compose file.

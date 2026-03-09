monitoring-stack

Prometheus + Grafana + Node Exporter running

Run

cd monitoring-stack

docker compose up -d

docker compose ps


Set-up Grafana 


Alerts

- CPU > 80% for 2m → warning
- Memory > 85% for 2m → warning
- Disk < 15% for 5m → critical

Commands

docker compose down

docker compose logs prometheus

docker compose logs grafana

curl -X POST http://localhost:9090/-/reload

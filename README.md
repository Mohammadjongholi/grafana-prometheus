
# Linux Server Monitoring with Prometheus and Grafana

A Docker Compose monitoring stack for Linux servers using Prometheus,
Grafana, and Node Exporter.

## Features

- Linux CPU, memory, disk, and uptime metrics
- Prometheus metrics collection every 15 seconds
- Grafana data source provisioning
- Dashboard provisioning from JSON
- Prometheus alert rules
- Persistent metrics and dashboard data

## Architecture

```text
Linux Host
   |
   +-- Node Exporter :9100
   |        |
   |        v
   |    Prometheus :9090
   |        |
   |        v
   +---- Grafana :3000
```

## Requirements

- Linux host with Docker Engine
- Docker Compose plugin
- Git
- Network access to the required ports

## Project Structure

```text
.
├── compose.yml
├── prometheus/
│   ├── prometheus.yml
│   └── alert-rules.yml
└── grafana/
    ├── provisioning/
    │   ├── datasources/
    │   └── dashboards/
    └── dashboards/
```

## Deploy

Clone the repository:

```bash
git clone https://github.com/Mohammadjongholi/grafana-prometheus.git
cd grafana-prometheus
```

Validate the Compose configuration:

```bash
docker compose config -q
```

Start the stack:

```bash
docker compose up -d
docker compose ps
```

## Access

- Prometheus: `http://<MONITORING_SERVER>:9091`
- Grafana: `http://<MONITORING_SERVER>:3000`

Replace `<MONITORING_SERVER>` with your monitoring server's IP address
or DNS name.

Grafana loads the Prometheus data source and the Linux Server Monitoring
dashboard automatically.

## Validate

Check Prometheus readiness:

```bash
curl -fsS http://localhost:9091/-/ready
```

Check scrape targets:

```bash
curl -fsS http://localhost:9091/api/v1/targets
```

Validate Prometheus configuration:

```bash
docker exec prometheus \
  promtool check config /etc/prometheus/prometheus.yml
```

Validate alert rules:

```bash
docker exec prometheus \
  promtool check rules /etc/prometheus/alert-rules.yml
```

## Alerts

The project includes rules for:

- Unavailable Prometheus targets
- CPU usage above 80%
- Available memory below 10%
- Root filesystem free space below 15%

Alerts are evaluated by Prometheus. Notification delivery requires
Alertmanager or another notification integration.

## Data Persistence

Prometheus metrics and Grafana data are stored in Docker named volumes.
Do not remove these volumes when recreating containers unless you intend
to delete the stored data.

## Security Notes

- Do not commit passwords, tokens, or private keys.
- Change default Grafana credentials.
- Restrict access to the monitoring ports.
- Do not expose Node Exporter directly to untrusted networks.
- Pin container image versions for repeatable deployments.
- Review container privileges and host filesystem mounts.

## Author

Mohammad Jangholi

GitHub: https://github.com/Mohammadjongholi

Repository: https://github.com/Mohammadjongholi/grafana-prometheus

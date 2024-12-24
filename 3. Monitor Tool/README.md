# Monitor Tool Setup

This guide will help you set up Grafana and Prometheus for monitoring.

## Prerequisites

- Docker installed on your system
- Docker Compose installed

## Step 1: Create Docker Compose File

Create a `docker-compose.yml` file with the following content:

```yaml
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus_data:/prometheus
    command:
      - "--config.file=/etc/prometheus/prometheus.yml"
    restart: unless-stopped

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    restart: unless-stopped

volumes:
  prometheus_data:
  grafana_data:
```

## Step 2: Configure Prometheus

Create a `prometheus/prometheus.yml` file with the following content:

```yaml
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: "node_exporter"
    static_configs:
      - targets: ['<ip-server>:9100','<ip-server>:9100',...]
```

## Step 3: Start Services

Run the following command to start Grafana and Prometheus:

```sh
docker compose up -d
```

## Step 4: Access Grafana and Prometheus

- Grafana: [http://localhost:3000](http://localhost:3000)
- Prometheus: [http://localhost:9090](http://localhost:9090)

## Step 5: Configure Grafana

1. Open Grafana in your browser.
2. Log in with the default credentials (`admin`/`admin`).
3. Add Prometheus as a data source:
     - Go to **Configuration** > **Data Sources**.
     - Click **Add data source**.
     - Select **Prometheus**.
     - Set the URL to `http://prometheus:9090`.
     - Click **Save & Test**.

You are now ready to use Grafana and Prometheus for monitoring.

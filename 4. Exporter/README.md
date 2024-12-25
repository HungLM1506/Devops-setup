# Node Exporter and Cadvisor Setup

## Introduction

This document provides instructions on how to install and configure Node Exporter and cAdvisor to monitor your system and containers.

## Requirements

- Docker
- Docker Compose

## Installation

### Node Exporter

1. Create a `docker-compose-exporter.yml` file:

    ```yaml
    services:
      node-exporter:
        image: prom/node-exporter:latest
        ports:
          - "9100:9100"
        volumes:
          - /proc:/host/proc:ro
          - /sys:/host/sys:ro
          - /:/rootfs:ro
        command:
          - '--path.procfs=/host/proc'
          - '--path.sysfs=/host/sys'
          - '--collector.filesystem.ignored-mount-points="^/(sys|proc|dev|host|etc)($$|/)"'
    ```

2. Start Node Exporter:

    ```sh
    docker compose -f docker-compose-exporter up -d
    ```

### Cadvisor

1. Add Cadvisor configuration to the `docker-compose-cadvisor.yml` file:

    ```yaml
    services:
      cadvisor:
        image: google/cadvisor:latest
        ports:
          - "8080:8080"
        volumes:
          - /:/rootfs:ro
          - /var/run:/var/run:rw
          - /sys:/sys:ro
          - /var/lib/docker/:/var/lib/docker:ro
    ```

2. Start cAdvisor:

    ```sh
    docker compose -f docker-compose-cadvisorup -d 
    ```

## Verification

- Access Node Exporter at `http://localhost:9100/metrics`
- Access Cadvisor at `http://localhost:8080`

## Conclusion

You have successfully installed Node Exporter and cAdvisor to monitor your system and containers.
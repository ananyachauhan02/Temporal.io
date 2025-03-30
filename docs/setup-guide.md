# Temporal.io Setup Guide

This guide provides step-by-step instructions for setting up Temporal using Docker and Kubernetes (K3s), as well as configuring monitoring with Prometheus and Grafana.

## Prerequisites
- Docker installed ([Installation Guide](https://docs.docker.com/get-docker/))
- Kubernetes (K3s) installed ([Installation Guide](https://k3s.io/))
- `kubectl` installed and configured
- Basic understanding of Temporal.io architecture

## Setup Options
### 1. Using Docker
- Deploy Temporal with PostgreSQL and Elasticsearch
- Instructions available in [docker-setup.md](docker-setup.md)

### 2. Using K3s (Lightweight Kubernetes)
- Deploy Temporal in a K3s cluster
- Instructions available in [k3s-setup.md](k3s-setup.md)

### 3. Monitoring with Prometheus & Grafana
- Set up monitoring for Temporal
- Instructions available in [monitoring.md](monitoring.md)

### 4. Troubleshooting
- Common issues and solutions
- See [troubleshooting.md](troubleshooting.md)

## Testing Your Setup
- Verify Temporal is running
- Steps provided in [testing.md](testing.md)







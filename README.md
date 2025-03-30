# Temporal Setup by docker compose 🚀

![Docker](https://img.shields.io/badge/Docker-✔-blue)
![Temporal](https://img.shields.io/badge/Temporal-Workflow-orange)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-DB-336791?logo=postgresql)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-Search-005571?logo=elasticsearch)

## 📌 What is Temporal.io?

Temporal.io is a workflow orchestration platform designed to help developers build reliable, scalable, and fault-tolerant applications. It ensures that long-running workflows execute without losing progress, even in the face of failures.
Temporal provides stateful execution, meaning it maintains the execution state of workflows indefinitely. It automatically handles failures, retries, and task scheduling, making distributed applications more resilient.

Deploy Temporal with Docker Compose (including PostgreSQL & Elasticsearch) in minutes.

## 📌 Features:
✅ Temporal Server & UI 🖥️

✅ PostgreSQL as Persistence DB 🛢️

✅ Elasticsearch for Search 🔎

✅ Simple Setup with Docker Compose 🚀



## 📜 Table of Contents:
🚀 Quick Start

📊 Verify Deployment

🔧 Configuration

💡 Troubleshooting

📜 License

## 🚀 Quick Start

1. Install Docker ([Installation Guide](https://docs.docker.com/get-docker/))
   
2. Clone the repository and run the containers
```sh
git clone https://github.com/temporalio/docker-compose.git

cd docker-compose
docker compose up
```
3. View Running Containers
```sh
docker ps 
```
<img width="1487" alt="Screenshot 2025-03-30 at 10 07 20 PM" src="https://github.com/user-attachments/assets/0ac5d49e-2b52-4f34-b621-501f1c1e99a7" />



## 📊 Verify Deployment
🖥️ Access Temporal UI

🔗 Open http://[public-ip-of-server]:8080 in your browser.



## 🔧 Configuration
🔹 Modify docker-compose.yml
```sh
version: "3.5"
services:
  elasticsearch:
    container_name: temporal-elasticsearch
    environment:
      - cluster.routing.allocation.disk.threshold_enabled=true
      - cluster.routing.allocation.disk.watermark.low=512mb
      - cluster.routing.allocation.disk.watermark.high=256mb
      - cluster.routing.allocation.disk.watermark.flood_stage=128mb
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms256m -Xmx256m
      - xpack.security.enabled=false
    image: elasticsearch:${ELASTICSEARCH_VERSION}
    networks:
      - temporal-network
    expose:
      - 9200
    volumes:
      - /var/lib/elasticsearch/data
  postgresql:
    container_name: temporal-postgresql
    environment:
      POSTGRES_PASSWORD: temporal
      POSTGRES_USER: temporal
    image: postgres:${POSTGRESQL_VERSION}
    networks:
      - temporal-network
    expose:
      - 5432
    volumes:
      - /var/lib/postgresql/data
  temporal:
    container_name: temporal
    depends_on:
      - postgresql
      - elasticsearch
    environment:
      - DB=postgres12
      - DB_PORT=5432
      - POSTGRES_USER=temporal
      - POSTGRES_PWD=temporal
      - POSTGRES_SEEDS=postgresql
      - DYNAMIC_CONFIG_FILE_PATH=config/dynamicconfig/development-sql.yaml
      - ENABLE_ES=true
      - ES_SEEDS=elasticsearch
      - ES_VERSION=v7
      - PROMETHEUS_ENDPOINT=0.0.0.0:8000
    image: temporalio/auto-setup:${TEMPORAL_VERSION}
    networks:
      - temporal-network
    ports:
      - 7233:7233
      - 8000:8000
    volumes:
      - ./dynamicconfig:/etc/temporal/config/dynamicconfig
  temporal-admin-tools:
    container_name: temporal-admin-tools
    depends_on:
      - temporal
    environment:
      - TEMPORAL_ADDRESS=temporal:7233
      - TEMPORAL_CLI_ADDRESS=temporal:7233
    image: temporalio/admin-tools:${TEMPORAL_ADMINTOOLS_VERSION}
    networks:
      - temporal-network
    stdin_open: true
    tty: true
  temporal-ui:
    container_name: temporal-ui
    depends_on:
      - temporal
    environment:
      - TEMPORAL_ADDRESS=temporal:7233
      - TEMPORAL_CORS_ORIGINS=http://localhost:3000
    image: temporalio/ui:${TEMPORAL_UI_VERSION}
    networks:
      - temporal-network
    ports:
      - 8080:8080
networks:
  temporal-network:
    driver: bridge
    name: temporal-network
```


# 📊 Temporal.io Logging and Monitoring

1. Install Prometheus using following commands:
```sh
sudo useradd --no-create-home --shell /bin/false prometheus
mkdir /etc/prometheus
mkdir /var/lib/prometheus
chown prometheus:prometheus /var/lib/prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.46.0/prometheus-2.46.0.linux-amd64.tar.gz
tar -xvf prometheus-2.46.0.linux-amd64.tar.gz
cd prometheus-2.46.0.linux-amd64
sudo mv console* /etc/prometheus
sudo mv prometheus.yml /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus
mv prometheus /usr/local/bin/
chown prometheus:prometheus /usr/local/bin/prometheus
```

2. Create a systemd service of prometheus
```sh
sudo nano /etc/systemd/system/prometheus.service

# add the following configuration in the file
[Unit]
Description=Prometheus Monitoring System
After=network.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus

[Install]
WantedBy=multi-user.target
```

3. To start and enable the service and check status of service
```sh
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
systemctl status prometheus
```

4. 📊 Verify Deployment
🖥️ Access prometheus UI
🔗 Open http://[public-ip-of-server]:9090 in your browser.

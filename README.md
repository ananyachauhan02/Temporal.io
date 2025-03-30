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

📦 Prerequisites

🛠️ Setup Guide

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


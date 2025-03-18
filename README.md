# 🚀 Elastic & Kibana on Raspberry Pi 4

This repository contains a **lightweight** and **optimized** configuration of **Elasticsearch and Kibana** for Raspberry Pi 4 using Docker.

## ⚡ Features & Optimizations

- ✅ **Minimal RAM usage**: Elasticsearch limited to **1GB RAM**, Kibana to **512MB RAM**.
- ✅ **Fleet Server & Security disabled** to **reduce load**.
- ✅ **Only Discover & Logs enabled** (no unnecessary modules).
- ✅ **Swap file recommendation** for better performance.

## 📦 Services Overview

### **1️⃣ Elasticsearch**

- **Heap size limited** (`-Xms512m -Xmx1g`)
- **Single-node mode** (`discovery.type=single-node`)
- **Security enabled** (basic authentication required)
- **SSL disabled** for simplicity
- **Persistent data volume** (`esdata`)

### **2️⃣ Kibana**

- **Only essential features enabled**:
  - ✅ `Discover`
  - ✅ `Logs`
- **Disabled non-essential modules**:
  - ❌ Fleet
  - ❌ Machine Learning (ML)
  - ❌ Security Solution
  - ❌ Enterprise Search
  - ❌ APM, Uptime, Metrics, Graph, Canvas, Reporting
- **Heap size limited** (`-Xms512m -Xmx1g`)

### **3️⃣ Setup Container (One-time initialization)**

- **Waits for Elasticsearch to be ready**
- **Sets user passwords for `elastic` and `kibana_system`**
- **Stops after successful setup** (no unnecessary resource usage)

## 🔧 **System Configuration for Raspberry Pi 4**

Elasticsearch **requires swap memory** to prevent crashes on low RAM.

### **1️⃣ Enable Swap File (Recommended 2GB Swap)**

```sh
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

### **2️⃣ Make Swap Persistent (on reboot)**

```sh
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
## 🚀 How to Run

### **Create a `.env` file with credentials:**
```ini
# Password for the 'elastic' user (at least 6 characters)
ELASTIC_PASSWORD={YOUR_ELASTIC_PASSWORD}

# Password for the 'kibana_system' user (at least 6 characters)
KIBANA_PASSWORD={YOUR_KIBANA_PASSWORD}

# Version of Elastic products
STACK_VERSION=8.17.3

# Set the cluster name
CLUSTER_NAME=docker-cluster

# Set to 'basic' or 'trial' to automatically start the 30-day trial
LICENSE=basic
#LICENSE=trial

# Port to expose Elasticsearch HTTP API to the host
ES_PORT=9200
#ES_PORT=127.0.0.1:9200

# Port to expose Kibana to the host
KIBANA_PORT=5601
#KIBANA_PORT=80

# Increase or decrease based on the available host memory (in bytes)
MEM_LIMIT=1073741824

# Project namespace (defaults to the current folder name if not set)
#COMPOSE_PROJECT_NAME=myproject
```

#### License Overview
Elasticsearch offers different license types, including:

- **Basic License**: Free and includes many features such as security, monitoring, and machine learning APIs with some limitations.
- **Trial License**: A 30-day trial that enables all premium features of the Platinum license. After expiration, it reverts to the Basic license unless upgraded.

## Start the stack:
```
docker-compose up -d
```

## Access Kibana:
```
URL: http://localhost:5601
Login with elastic / <your-password>
```

## 🎯 Why We Disabled These Features?
```
| Feature                    | Reason for Disabling                        |
|----------------------------|---------------------------------------------|
| **Fleet & Ingest Manager** | Not needed unless managing multiple agents  |
| **APM & Observability**    | Reduces memory usage                        |
| **Machine Learning (ML)**  | Uses a lot of RAM                           |
| **Graph & Canvas**         | Not required for basic log search           |
| **Enterprise Search**      | Heavy and not needed for Raspberry Pi setup |
| **Reporting & Alerting**   | Can be enabled later if necessary           |
```
## 📌 Notes
```
- This setup is **optimized for Raspberry Pi 4 (2GB+ RAM)**.
- If using **only 2GB RAM**, **increase swap to 4GB**.
- **For better performance**, use an SSD instead of an SD card.

💡 **Now you have a lightweight Elasticsearch & Kibana setup running on your Raspberry Pi!** 🚀
```

#### Official Elasticsearch Docker Compose
[Official Elasticsearch Docker Compose](https://github.com/elastic/elasticsearch/blob/8.17/docs/reference/setup/install/docker/docker-compose.yml)
[Official Elasticsearch ENV](https://github.com/elastic/elasticsearch/blob/8.17/docs/reference/setup/install/docker/.env)



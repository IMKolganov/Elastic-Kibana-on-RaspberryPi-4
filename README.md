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
STACK_VERSION=8.10.2
ELASTIC_PASSWORD=SuperSecurePassword
KIBANA_PASSWORD=KibanaSecurePassword
MEM_LIMIT=1g
```

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


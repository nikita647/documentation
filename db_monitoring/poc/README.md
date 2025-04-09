# POC of Middleware Monitoring



| **Author** | **Created on** | **Version** | **Last updated by**|**Last Edited On**|**Level** |**Reviewer** |
|------------|---------------------------|-------------|----------------|-----|-------------|-------------| 
| Nikita Joshi|   29-03-2025          | v1          | Nikita Joshi   |29-03-2025    |  Internal Reviewer | Komal Jaiswal |


---



## Table of Contents
- [Prerequisites](#prerequisites)
- [Install Redis on Ubuntu](#install-redis-on-ubuntu)
- [Install Prometheus on Ubuntu](#install-prometheus-on-ubuntu)
- [Install Grafana on Ubuntu](#install-grafana-on-ubuntu)
- [Create User Group and User for Prometheus](#create-user-group-and-user-for-prometheus)
- [Download and Install Redis Exporter](#download-and-install-redis-exporter)
- [Configuring Redis Exporter Service](#configuring-redis-exporter-service)
- [Configuring Redis Exporter Endpoint in Prometheus Configuration](#configuring-redis-exporter-endpoint-in-prometheus-configuration)
- [Grafana Dashboards for Redis Metrics](#grafana-dashboards-for-redis-metrics)
- [Best practices](#best-practices)
- [Conclusion](#conclusion)
- [Reference Links](#reference-links)

## Introduction

This document explains how to monitor Redis using Prometheus and Grafana. It highlights the importance of monitoring Redis for performance, health, and resource usage. Prometheus collects data from Redis, while Grafana visualizes this data, allowing for the creation of custom dashboards. This setup provides real-time insights, helping you detect issues early, optimize performance, and maintain a healthy Redis environment.

## Prerequisites
|                                                 |
|-------------------------------------------------|
| AWS Account with Ubuntu 22.04 LTS EC2 Instance. |
| Basic knowledge of AWS services, Prometheus, and Grafana. |

# Getting Started


## Install Redis on Ubuntu
Update the system before starting the installation:
```
sudo apt update
```
Add the repository to the apt index and then update it:
```
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
sudo apt update
```
After updating the repository, install Redis:
```
sudo apt-get install redis
```

### **Create a system user for Prometheus**
  ```
  sudo useradd --no-create-home --shell /bin/false prometheus
  ```
___
### **Create configuration directories**

  ``` bash
  sudo mkdir /etc/prometheus
  sudo mkdir /var/lib/prometheus
  ```
___
### **Set directory ownership**
  ``` bash
  sudo chown prometheus:prometheus /var/lib/prometheus
  ```
___
### **Navigate to `/tmp`**
  ``` bash
  cd /tmp/
  ```
___
### **Download Prometheus from the [Official Page](https://prometheus.io/download/#prometheus)**
  ``` bash
  wget https://github.com/prometheus/prometheus/releases/download/v3.2.1/prometheus-3.2.1.linux-amd64.tar.gz
 ```

___
###  **Extract the Prometheus package**
  ``` bash
  sudo tar -xvf prometheus-3.2.1.linux-amd64.tar.gz
  ```
___
###  **Move and set ownership of configuration files**

  ``` bash
  cd prometheus-2.47.2.linux-amd64
```
``` bash
  sudo mv prometheus.yml /etc/prometheus
```
``` bash  
sudo chown -R prometheus:prometheus /etc/prometheus
```
___
### **Move binaries and set ownership**

``` bash
sudo mv prometheus /usr/local/bin/
```
``` bash
sudo chown prometheus:prometheus /usr/local/bin/prometheus
```
___
### **Create a Systemd Service File**
``` bash
sudo nano /etc/systemd/system/prometheus.service
```

``` bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
___
### **Reload systemd and start Prometheus**
``` bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus
```

___
### **Access Prometheus in the browser**
``` bash
<server-public-ip>:9090
```

## **Installation Steps**

### **Download the Grafana GPG key** 

``` bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

### **Add the Grafana repository**
``` bash
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

### **Update package lists**
``` bash
sudo apt update
```

### **Install Grafana**
``` bash
sudo apt install grafana
```

### **Start the Grafana server**
``` bash
sudo systemctl start grafana-server
```

### **Check service status**
``` bash
sudo systemctl status grafana-server
sudo systemctl enable grafana-server
```

### **Access in browser**
``` bash
<instance_ip>:3000
```


## Download and Install Redis Exporter
Install the Redis exporter:
```
curl -s https://api.github.com/repos/oliver006/redis_exporter/releases/latest | grep browser_download_url | grep linux-amd64 | cut -d '"' -f 4 | wget -qi -
```
Extract the downloaded archive file:
```
tar xvf redis_exporter-*.linux-amd64.tar.gz
```
Move the binary to the appropriate directory:
```
sudo mv redis_exporter-*.linux-amd64/redis_exporter /usr/local/bin/
```
Confirm the installation:
```
redis_exporter --version
```
## Configuring Redis Exporter Service
Create a service for the Redis exporter:
```
sudo vim /etc/systemd/system/redis_exporter.service
```
Add the following content toredis_exporter.service
```
[Unit]
Description=Redis Exporter for Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
ExecStart=/usr/local/bin/redis_exporter
Restart=always

[Install]
WantedBy=multi-user.target

```
Save the file, verify the configuration, and reload the daemon service:
```
sudo systemctl daemon-reload
sudo systemctl enable redis_exporter
sudo systemctl start redis_exporter
```
Confirm the service status:
***
<img width="953" alt="image-5" src="https://github.com/user-attachments/assets/dc7e75ae-7a60-415d-abf8-f707ea47530e" />

***



## Configuring Redis Exporter Endpoint in Prometheus Configuration
<img width="1028" alt="Screenshot 2025-01-14 at 10 11 40 PM" src="https://github.com/user-attachments/assets/7823fa6e-9150-4efb-bc17-99ccad5b5980" />



Save the file and restart the Prometheus service:
```
sudo systemctl restart prometheus.service
```



## Grafana Dashboards for Redis Metrics

<img width="1674" alt="Screenshot 2025-01-14 at 10 15 03 PM" src="https://github.com/user-attachments/assets/81b7c2fb-077d-4884-ae7a-565a74035fc5" />


## Cheack Prometheus Dashboards status traget
<img width="1679" alt="Screenshot 2025-01-14 at 10 15 52 PM" src="https://github.com/user-attachments/assets/9ab20176-3ee5-419e-92d9-fc337fb625b5" />


## Best practices

- **Select Relevant Metrics:** Choose key metrics to monitor (e.g., latency, memory usage).
- **Simulate Scenarios:** Test with various workloads to evaluate performance.
- **Optimize Dashboards:** Create clear and actionable dashboards.
- **Document Findings:** Record issues and resolutions for future reference.

## Conclusion
In conclusion, implementing a monitoring solution for Redis databases using Prometheus and Grafana is essential for maintaining the health, performance, and reliability of your Redis environment. By following the steps outlined in this guide, you can effectively set up and configure Prometheus to collect metrics from Redis instances, visualize these metrics in Grafana dashboards, and proactively manage your Redis infrastructure.

## Reference Links

[Middleware Monitoring Documentation]()

[Monitoring Dashboard Design]()



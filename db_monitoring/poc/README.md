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

## Getting Started


### Step 1. **Install Redis**
- To install Redis on your system, please follow the link below for the Redis Installation Guide. :- [Redis Installation Guide](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Mohit-SCRUM-12/Common/Software/Redis/Installation/README.md)


### Step 2. **Update Redis Configuration**
- Kindly follow the link below for this  :- [ Redis Configuration and Security](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Mohit-SCRUM-12/Common/Software/Redis/Configuration/README.md)

  
- Opens the Redis configuration file for editing.
  To bind Redis to specific IP addresses (e.g., `127.0.0.1,172.31.24.234` and a private 
 IP) for security and accessibility.
---

###  Step 3. **Setup Prometheus**
 - To install Prometheus on your system, please follow the link below for the Prometheus Setup Guide. :-[ Prometheus Setup  Guide](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Nikita-SCRUM-104/Common/Software/Prometheus/README.md)

   
- **Access Prometheus in the browser**
``` bash
<server-public-ip>:9090
```
![Screenshot 2025-04-08 161204](https://github.com/user-attachments/assets/6251666e-1d35-4cd9-9494-41170600ce6e)



###  Step 4. **Setup Grafana**
 - To Setup Grafana on your system, please follow the link below for the Grafana Setup Guide. :-[ Grafana Setup  Guide](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Nikita-SCRUM-104/Common/Software/Grafana/README.md)

   
- **Access in browser**
``` bash
<instance_ip>:3000
```

![Screenshot 2025-04-08 161153](https://github.com/user-attachments/assets/847b6c82-840e-4b4f-bece-59d73de8de64)


### Step 5. **Setup Redis Exporter**

- Install the Redis exporter:
```
curl -s https://api.github.com/repos/oliver006/redis_exporter/releases/latest | grep browser_download_url | grep linux-amd64 | cut -d '"' -f 4 | wget -qi -
```
- Extract the downloaded archive file:
```
tar xvf redis_exporter-*.linux-amd64.tar.gz
```
- Move the binary to the appropriate directory:
```
sudo mv redis_exporter-*.linux-amd64/redis_exporter /usr/local/bin/
```
- Confirm the installation:
```
redis_exporter --version
```


### Step 6.  **Configuring Redis Exporter Service**

- Create a service for the Redis exporter:
```
sudo vim /etc/systemd/system/redis_exporter.service
```
- Add the following content toredis_exporter.service
```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/local/bin/redis_exporter \
  --log-format=txt \
  --namespace=redis \
  --web.listen-address=:9121 \
  --web.telemetry-path=/metrics

SyslogIdentifier=redis_exporter
Restart=always

[Install]
WantedBy=multi-user.target

```
- Save the file, verify the configuration, and reload the daemon service:
```
sudo systemctl daemon-reload
sudo systemctl enable redis_exporter
sudo systemctl start redis_exporter
```
- Confirm the service status:

```
systemctl status redis_exporter
```

![Screenshot 2025-04-08 162256](https://github.com/user-attachments/assets/a049c3e8-1d3d-44be-8e09-43b6ae9a9e63)

### **Access in browser**
``` bash
<instance_ip>:9121
```
![image](https://github.com/user-attachments/assets/edc2a562-805c-42f9-8df1-29dc9eabbddf)


## Step 7. Configuring Redis Exporter Endpoint in Prometheus Configuration

- Lets update our configuration file using below command:

``` bash
sudo nano /etc/prometheus/prometheus.yml

```
```
#Redis Servers
  - job_name: "redis"
    static_configs:
      - targets: ["localhost:9121"]

```

- Save the file and restart the Prometheus service:
```
sudo systemctl restart prometheus.service
```

- Now go to Prometheus dashboard and click on status, select target, you can see our exporter are up and running.


![Screenshot 2025-04-09 141835](https://github.com/user-attachments/assets/707e40dc-63d3-4456-a463-4abf9c052b44)





## Step 8. Setting up Grafana Dashboards for Redis Metrics

- go to the Connections and select the Data sources option.

![image](https://github.com/user-attachments/assets/478be982-e4c8-4f94-a88d-54726baa63ae)

- Search for Prometheus in the search bar and select it.

![image](https://github.com/user-attachments/assets/00531fd9-f75e-4617-9db6-4dccefe18b7c)

- In connection, in Prometheus server URL, give the server url on which our prometheus is running.


![image](https://github.com/user-attachments/assets/d321d584-0774-4893-9d18-04556296f6b7)

- After this click on save and test button. You will see the message for prometheus being successfully queried.


![image](https://github.com/user-attachments/assets/a94f3bac-9563-4025-9f5b-2d92db789d8d)

- Here you can start your own new dashboard by adding a visualization.


  So click on + Add visualization option button.

  Here you can also import dashboards.

![image](https://github.com/user-attachments/assets/3fe72a54-0383-43a3-9341-993d6946ae6e)


![image](https://github.com/user-attachments/assets/21b35188-1331-4ede-a719-300741748d38)


- Now in the Query section add the A query

go_gc_duration_seconds_sum
instance: localhost:9090


![image](https://github.com/user-attachments/assets/69be7d63-92cb-49f1-8b10-f6c688285744)

![image](https://github.com/user-attachments/assets/24040994-043f-4400-ad6d-12594e68da07)

![image](https://github.com/user-attachments/assets/1f6a1679-00f5-4b15-a4f4-987939c40617)

then add the B query

Metric: redis_cpu_user_seconds_total
job: redis
![image](https://github.com/user-attachments/assets/936d7c6c-1973-4ac7-88b4-aa704db34a28)


![Screenshot 2025-04-09 142402](https://github.com/user-attachments/assets/e0f927b8-206e-4a03-b8ed-4634a76c0dc9)

## Cheack Prometheus Dashboards status traget
![image](https://github.com/user-attachments/assets/042fa96b-2a5e-4066-959d-4e75e69fb548)


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



# POC of Infra Monitoring


<img src="https://github.com/user-attachments/assets/e0495a0e-f0f7-4179-8a7f-c6e5031b1be3" width="700" /> <br>


| **Author** | **Created on** | **Version** | **Last updated by**|**Last Edited On**|**Level** |**Reviewer** |
|------------|----------------------|-------------|----------------|-----|-------------|-------------|
| Aayush Verma|   14-04-2025        | Version 1   | Aayush Verma   |14-04-2025    |  Internal Reviewer | Komal Jaiswal |

---

## Table of Contents
- [Prerequisites](#prerequisites)
- [Setup Prometheus](#step-3-setup-prometheus)
- [Setup Grafana](#step-4-setup-grafana)
- [Best practices](#best-practices)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [Reference Links](#reference-links)

## Introduction

This document provides a step-by-step guide for setting up infrastructure monitoring using Prometheus, Grafana, and Node Exporter. while Grafana visualizes this data, allowing for the creation of custom dashboards. This setup provides real-time insights, helping you detect issues early, optimize performance

## Prerequisites
|                                                 |
|-------------------------------------------------|
| AWS Account with Ubuntu 22.04 LTS EC2 Instance. |
| Basic knowledge of AWS services, Prometheus, and Grafana. |

## Getting Started


###  Step 1. **Setup Prometheus**
 - To install Prometheus on your system, please follow the link below for the Prometheus Setup Guide. :-[ Prometheus Setup  Guide](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Nikita-SCRUM-104/Common/Software/Prometheus/README.md)

   
- **Access Prometheus in the browser**
``` bash
<server-public-ip>:9090
```
![image](https://github.com/user-attachments/assets/ff94863e-138b-4413-9035-a3c89ff9b4ef)


### **step 2. Setup NodeExporter**
 - To Setup NodeExporter on your system, please follow the link below for the NodeExporter Setup Guide. :-[ NodeExporter Setup  Guide](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Nikita-SCRUM-104/Common/Software/NodeExporter/README.md)

### **Access in browser**
``` bash
<instance_ip>:9100
```
![image](https://github.com/user-attachments/assets/47a668cc-e627-4985-a0e9-93ac875684e2)


## Step 3. Configuring Node Exporter Endpoint in Prometheus Configuration

- Lets update our configuration file using below command:

``` bash
sudo nano /etc/prometheus/prometheus.yml

```
```
- job_name: 'Node_Exporter'

    scrape_interval: 5s

    static_configs:

      - targets: ['<Server_IP_of_Node_Exporter_Machine>:9100']

```

- Save the file and restart the Prometheus service:
```
sudo systemctl restart prometheus.service
```

- Now go to Prometheus dashboard and click on status, select target, you can see our exporter are up and running.
- 


![image](https://github.com/user-attachments/assets/b55c3380-eece-49c8-bfe3-a1593c299dde)


###  Step 3. **Setup Grafana**
 - To Setup Grafana on your system, please follow the link below for the Grafana Setup Guide. :-[ Grafana Setup  Guide](https://github.com/snaatak-Zero-Downtime-Crew/Documentation/blob/Nikita-SCRUM-104/Common/Software/Grafana/README.md)

   
- **Access in browser**
``` bash
<instance_ip>:3000
```

![Screenshot 2025-04-08 161153](https://github.com/user-attachments/assets/847b6c82-840e-4b4f-bece-59d73de8de64)

Output:

default username: admin
default password: admin

![image](https://github.com/user-attachments/assets/f83c706a-01f4-4e91-a1d9-a9f5d630bb59)

___

- To visualize metrics, you need to add a data source first.

![image](https://github.com/user-attachments/assets/478be982-e4c8-4f94-a88d-54726baa63ae)

Click Add data source and select Prometheus

![image](https://github.com/user-attachments/assets/00531fd9-f75e-4617-9db6-4dccefe18b7c)

For the URL, enter http://localhost:9090 and click Save and test. You can see Data source is working.

![image](https://github.com/user-attachments/assets/d321d584-0774-4893-9d18-04556296f6b7)


Click on Save and Test.
![image](https://github.com/user-attachments/assets/a94f3bac-9563-4025-9f5b-2d92db789d8d)


- Here you can start your own new dashboard by adding a visualization.


  So click on + Add visualization option button.

  Here you can also import dashboards.

![image](https://github.com/user-attachments/assets/3fe72a54-0383-43a3-9341-993d6946ae6e)


Click on Import Dashboard paste this code 1860 and click on load

![image](https://github.com/user-attachments/assets/d6991b63-373d-4344-a2e1-b860042edf91)



![image](https://github.com/user-attachments/assets/d0d71fe8-7d7a-4a46-97f6-835547cd1c0b)
![image](https://github.com/user-attachments/assets/3b9c1d72-a560-4fc5-bd4f-2c57d64550e6)


---


___

####  **Prometheus Dashboards status target**

![image](https://github.com/user-attachments/assets/a4371698-25bb-4e54-a291-ca99e10e5f82)


## Best Practices

- Set up alert rules in Prometheus for CPU/memory thresholds.
- Regularly back up Grafana dashboards.
- Secure all endpoints using basic auth or reverse proxy with HTTPS.
- Monitor exporter logs for silent failures.

## Conclusion
By integrating Prometheus and Grafana with NodeExporter, we achieve real-time system-level monitoring. This setup ensures better visibility into infrastructure health, enabling proactive troubleshooting and performance optimization.


___

# Contact Information

| **Name**       | **Email Address**            |
|-----------------|------------------------------|
| Aayush Verma    | <aayush.verma@mygurukulam.co>     |


## Reference Links

| **Links**                                           | 
|-----------------------------------------------------|
|[Prometheus Official Documentation](https://prometheus.io/docs/introduction/overview/)|
|[Grafana Official Documentation](https://grafana.com/docs/)|
|[NodeExporter GitHub](https://github.com/prometheus/node_exporter)|






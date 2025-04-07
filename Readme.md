# Documentation on Rolling Deployment


| **Author** | **Created on** | **Version** | **Last updated by**|**Last Edited On**|**Level** |**Reviewer** |
|------------|---------------------------|-------------|----------------|-----|-------------|-------------| 
| Nikita Joshi|   8-04-2025          | v1          | Nikita Joshi   |8-04-2025    |  Internal Reviewer | Komal Jaiswal |





## Table of Contents

- [Introduction](#introduction)
- [Purpose](#purpose)
- [Features](#features)
- [Rolling Deployment Process](#rolling-deployment-process)
- [Rolling Deployment workflow](#rolling-deployment-workflow)
- [Tools and Technologies](#tools-and-technologies)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)
- [Contact information](#contact-information)
- [References](#references)


## Introduction

Rolling deployments are a software release strategy where new versions of an application are incrementally deployed to a subset of servers or instances. This process allows for a gradual rollout, reducing the risk of widespread issues caused by new releases. Instead of updating all instances at once, a rolling deployment updates a few at a time. This ensures that some instances continue running the old version while the new version is being deployed.

## Purpose
The purpose of this documentation is to provide a comprehensive guide on implementing rolling deployments. It covers the process, best practices, tools, and real-world examples to help you execute smooth and efficient rolling deployments.


## Features

| Feature               | Description                                                          |
|-----------------------|----------------------------------------------------------------------|
| **Zero Downtime**     | Gradually replace instances to avoid downtime.                        |
| **Incremental Updates** | Deploy updates in small batches to minimize risk.                     |
| **Automated Rollback**    | Automatically reverts to the previous stable version if issues are detected during deployment. |
| **Automated Deployment** | Use automation tools to streamline the deployment process.            |
| **Health-Driven Updates** | Updates only proceed if the updated instance passes all defined health and readiness checks. |


## Rolling Deployment Process

- ### Preparation

| Step                 | Description                                                          |
|----------------------|----------------------------------------------------------------------|
| **Environment Setup** | Ensure all servers and environments are configured properly.         |
| **Backup**           | Create backups of the current application state.                     |
| **Testing**          | Thoroughly test the new version in a staging environment.            |
| **Plan Rollout**     | Define the sequence and timing of the deployment across servers.     |


- ### Deployment
| Step                 | Description                                                          |
|----------------------|----------------------------------------------------------------------|
| **Batch Deployment** | Deploy the new version to a subset of servers at a time.             |
| **Health Checks**    | Perform health checks on updated servers to ensure stability.        |
| **Monitoring**       | Continuously monitor the deployment process for any issues.          |
| **Completion**       | Continue the process until all servers are running the new version.  |

## Rolling Deployment workflow
![rolling-deployment](https://github.com/user-attachments/assets/17e2c48f-fe65-4681-8e8d-94c42d73bf53)

##  Step By Step processs

#### 1. Prepare the New Version
- Build and verify the new application version.
- Ensure it is compatible with the existing environment to support smooth transitions.

#### 2. Configure the Load Balancer
- Use a load balancer to manage and route traffic.
- During deployment, it will temporarily remove servers being updated and restore them once verified.

#### 3. Begin Rolling Deployment
- Update servers one at a time (or in small batches).
- After each update, reroute traffic back to the updated server.

#### 4. Run Health Checks
- Validate that each updated server is healthy and functioning as expected.
- Continue only if the updated instance passes all necessary checks.

#### 5. Monitor Performance
- Monitor application performance metrics during the deployment (e.g., response time, error rate).
- Roll back the deployment if any issues are detected.

#### 6. Complete the Rollout
- Once all servers are updated, consider the deployment complete.
- Perform post-deployment testing and monitoring.




## **Tools and Technologies**

| Tool         | Description                                                           |
|--------------|-----------------------------------------------------------------------|
| **Kubernetes**| Container orchestration platform that supports rolling updates.      |
| **Ansible**  | Automation tool for configuration management and deployment.          |
| **Docker**   | Containerization platform that simplifies deployment.                 |
| **Jenkins**  | Continuous integration and continuous deployment server.              |
| **Prometheus** | Monitoring and alerting toolkit for system and application metrics.        |
| **Grafana**    | Visualization tool often used with Prometheus for monitoring dashboards.   |
| **Terraform**  | Infrastructure as Code (IaC) tool for provisioning cloud infrastructure.    |

___

##  Advantages vs  Disadvantages

| **Advantages**                              | **Disadvantages**                                                                 |
|---------------------------------------------|-----------------------------------------------------------------------------------|
|  Zero downtime                             |  Slower overall deployment process                                               |
|  Can rollback quickly if failure occurs    |  Compatibility issues between old and new versions during rollout               |
|  Only a portion of traffic is affected     |  Monitoring and debugging can be complex                                        |
|  Reduces risk by updating incrementally    |  Requires robust health checks (readiness/liveness probes)                      |
| Ensures continuous availability           |  Not ideal for major version changes with breaking dependencies                 |
| Good for large-scale systems              |  May need careful state management for session-based applications               |

___

## Best Practices

| **Best Practice**        | **Description**                                                                 |
|---------------------------|---------------------------------------------------------------------------------|
| **Test Thoroughly**       | Ensure comprehensive testing before deployment.                                |
| **Incremental Updates**   | Deploy updates in small batches to minimize downtime.                          |
| **Load Balancing**        | Use load balancers to redirect traffic away from servers being updated.         |
| **Monitor Continuously**  | Keep an eye on the deployment process and application performance.             |
| **Rollback Mechanism**    | Ensure you can quickly roll back to a stable version in case of failure.       |
| **Communication**         | Maintain clear communication with the team throughout the deployment process.  |
| **Auto-Scaling Support**  | Design rolling deployments to work with auto-scaling groups.                   |

___

## Conclusion
Rolling deployment is a highly effective strategy for updating applications while minimizing risk and downtime. By following the outlined process and best practices, you can achieve smooth and reliable deployments.


### **Contact Information**

| **Name** | **Email address**            | **Github ID**
|----------|-------------------------------|-------------------|
| Nikita joshi    |  jnikita647@gmail.com   | https://github.com/jnikita19  |

## References

| Links | Descriptions | 
|--------|------------|
| Rolling Blog | https://launchdarkly.com/blog/blue-green-deployments-versus-rolling-deployments/ | 
| Rolling deployments |https://octopus.com/devops/software-deployments/rolling-deployment/ | 
| Kubernetes Rolling Updates |https://kubernetes.io/docs/concepts/workloads/controllers/deployment/| 

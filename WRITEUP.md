# Write-up Template

## Analyze, choose, and justify the appropriate resource option for deploying the app.

_For **both** a VM or App Service solution for the CMS app:_

- _Analyze costs, scalability, availability, and workflow_
- _Choose the appropriate solution (VM or App Service) for deploying the app_
- _Justify your choice_

In order to decide which compute option to use, I followed the following flowchart:

![Use the following flowchart to select a candidate compute service.](images/compute-choices.png)

[Choose an Azure compute service for your application
](https://docs.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree) gives a good summary on the different compute options available on Azure.

Based on the information provided by the overview and the information below, I chose App Service for this project.

### Costs

- VM:
  - Running VMs is usually a bit less expensive than App Service but if we also consider the operational costs of updating the operating system, configuration changes, scale sets, load balancer, provisioning storage etc. it is more expensive for a simple application such as the article CMS
  - If an application is not running 24x7, VMs may be a good option as you only pay for compute when it's running
  - See [Linux Virtual Machines Pricing
    ](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux/) for details
- App Service
  - Cost depends on the chosen plan
  - As this is a test application, we can use the free tier
  - See [App Service pricing
    ](https://azure.microsoft.com/en-us/pricing/details/app-service/linux/) for details

### Scalability

- VM:
  - Supported by Virtual machine scale sets
  - Load balancer through Azure Load balancer
  - Limits:
    - Platform image: 1000 nodes per scale set
    - Custom image: 600 nodes per scale set
- App Service:
  - Built-in autoscaling service
  - Integrated load balancer
  - Limits: 30 instances, 100 with App Service Environment

### Availability

- VM:
  - Availability depends on the deployment type:
    - For all Virtual Machines that have two or more instances deployed across two or more Availability Zones in the same Azure region, we guarantee you will have Virtual Machine Connectivity to at least one instance at least 99.99% of the time.
    - For all Virtual Machines that have two or more instances deployed in the same Availability Set or in the same Dedicated Host Group, we guarantee you will have Virtual Machine Connectivity to at least one instance at least 99.95% of the time.
    - For any Single Instance Virtual Machine using Premium SSD or Ultra Disk for all Operating System Disks and Data Disks, we guarantee you will have Virtual Machine Connectivity of at least 99.9%.
    - For any Single Instance Virtual Machine using Standard SSD Managed Disks for Operating System Disk and Data Disks, we guarantee you will have Virtual Machine Connectivity of at least 99.5%.
    - For any Single Instance Virtual Machine using Standard HDD Managed Disks for Operating System Disks and Data Disks, we guarantee you will have Virtual Machine Connectivity of at least 95%.
  - Multi region failover through traffic manager
  - See [SLA for Virtual Machines](https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_9/ for details)
- App Service:
  - Apps running in a customer subscription will be available 99.95% of the time; No SLA is provided for Apps under either the Free or Shared tiers
  - Multi region failover through traffic manager
  - See [SLA for App Service](https://azure.microsoft.com/en-us/support/legal/sla/app-service/v1_4/) for details

### Workflow

- VM:
  - Needs custom DevOps workflow to deploy application
- App Service:
  - Easy deployment with the Azure CLI: `az webapp up`

## Assess app changes that would change your decision.

_Detail how the app and any other needs would have to change for you to change your decision in the last section._

App Service has some limitations:

- Scalability (14GB RAM and 4 vCPU cores per instance)
- Limited programming language support

In case the app needs better scalability or needs to support a different programming language, I would need to switch to VMs.

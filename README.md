# Microsoft Azure Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br>

## Table of Contents

## L1: Cloud Foundations (Entry-Level / Cloud Practitioner)
Focus: Core Azure concepts, cloud models, resource management, and fundamental services.

* [Azure Fundamentals](#-1-azure-fundamentals): Azure components, shared responsibility, cloud models, and resource management.
* [Core Services](#-2-core-services): Compute (VMs, App Service, AKS, Functions), Service Bus, Redis, API Management, and Batch.

## L2: Infrastructure (Associate Level)
Focus: Storage, networking, and security essentials for building and securing Azure infrastructure.

* [Security](#-3-security): Entra ID, RBAC, Key Vault, Managed Identities, Defender for Cloud, DDoS, JIT Access.
* [Azure Storage](#-4-azure-storage): Blob, access tiers, lifecycle management, replication options, SDK, and AzCopy.
* [Networking](#-5-networking): VNet, NSG, Load Balancer, Application Gateway, ExpressRoute, Bastion, VNet Peering.

## L3: Operations (Cloud Administrator / Developer)
Focus: Monitoring observability, databases, access governance, and operational management.

* [Monitoring](#-6-monitoring): Azure Monitor, KQL, Application Insights, alerts, distributed tracing, and sampling.
* [Databases](#-7-databases): Azure SQL, Cosmos DB, consistency models, Elastic Pools, PostgreSQL, and Redis.
* [Access Management](#-10-access-management): Entra ID Connect, Conditional Access, PIM, Managed Identities, B2B/B2C.

## L4: Architecture & Migration (Solutions Architect)
Focus: Cloud architecture patterns, disaster recovery, infrastructure as code, and migration strategies.

* [Architecture](#-8-architecture): Well-Architected Framework, multi-region HA, messaging services, ARM/Bicep, and DR.
* [Migration](#-9-migration): Azure Migrate, 6 R\'s, Database Migration Service, Data Box, Data Factory, and Integration Runtime.

## L5: Development & DevOps (Cloud Developer / DevOps Engineer)
Focus: Application development, CI/CD pipelines, performance optimization, and troubleshooting.

* [Application Development](#-13-application-development): Deployment slots, Azure Functions, Logic Apps, Event Grid, Service Bus, and AKS.
* [DevOps](#-14-devops): Azure DevOps YAML pipelines, GitHub Actions, Terraform, and container registries.
* [Performance Management](#-11-performance-management): Autoscaling, CDN, Front Door, Redis caching, SQL tuning, and VM right-sizing.
* [Troubleshooting](#-12-troubleshooting): Network Watcher, App Service diagnostics, AKS pods, VM connectivity, and failed deployments.

## L6: Expert (Cloud Architect / Lead)
Focus: Cost governance, AI integration, advanced patterns, and enterprise-scale decisions.

* [Cost Management](#-15-cost-management): Pricing models, Azure Advisor, Reserved Instances, Budgets, and Cost Analysis.
* [AI Services](#-16-ai-services): Azure OpenAI, Cognitive Services, AI Search, and Machine Learning.
* [Miscellaneous](#-17-miscellaneous): Advanced topics, best practices, and specialized Azure scenarios.
* [General Questions](#-18-general-questions): Cross-cutting questions covering real-world Azure architecture and operations.

<br>

## # 1. AZURE FUNDAMENTALS

<br>

## Q. What is Microsoft Azure and what are its core components?

**Microsoft Azure** is Microsoft\'s enterprise-grade cloud computing platform offering 200+ products and services across compute, networking, storage, databases, AI/ML, DevOps, security, and hybrid cloud. It operates across **60+ regions** worldwide (more than any other cloud provider as of 2025) organized into **Availability Zones** (physically separate datacenters within a region) for high availability.

**Core architectural components:**

| Component | Description |
|-----------|-------------|
| **Subscriptions** | Billing and access boundary for Azure resources |
| **Management Groups** | Hierarchical grouping of subscriptions for governance |
| **Resource Groups** | Logical containers for related resources sharing the same lifecycle |
| **Azure Resource Manager (ARM)** | Unified control plane for deploying and managing all resources |
| **Azure Regions & Availability Zones** | Geographic resilience and low-latency deployment |
| **Azure Active Directory (Microsoft Entra ID)** | Identity and access management for all Azure services |

**Create a resource group using Azure CLI:**

```bash
az group create \
  --name rg-myapp-prod \
  --location eastus \
  --tags Environment=Production Owner=TeamA
```

**ARM template (Bicep) for a resource group and storage account:**

```bicep
// main.bicep
param location string = 'eastus'
param storageName string = 'stmyapp${uniqueString(resourceGroup().id)}'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageName
  location: location
  sku: { name: 'Standard_LRS' }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
    supportsHttpsTrafficOnly: true
    minimumTlsVersion: 'TLS1_2'
  }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Describe the shared responsibility model in Azure.

The shared responsibility model defines which security tasks are handled by Microsoft (cloud provider) and which are the customer\'s responsibility. The boundary shifts depending on the service model.

```
                    Customer Responsibility →
|----------------------------------------------------------------------|
|                | On-Prem | IaaS    | PaaS    | SaaS    |
|----------------|---------|---------|---------|---------|
| Data           | Yes  | Yes  | Yes  | Yes  |
| Identity/Access | Yes  | Yes  | Yes  | Yes  |
| Application    | Yes  | Yes  | Yes  | MS   |
| OS & Runtime   | Yes  | Yes  | MS   | MS   |
| Networking     | Yes  | Shared  | MS   | MS   |
| Physical/DC    | Yes  | MS   | MS   | MS   |
```

**Key principle:** Microsoft always owns physical security, hardware, and the hypervisor. The customer always owns their data and identity configuration regardless of service type. For IaaS (VMs), the customer patches the OS. For PaaS (App Service), Microsoft patches the OS and runtime.

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/security/fundamentals/media/shared-responsibility/shared-responsibility.svg" alt="Azure Shared Responsibility Model" width="800px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the various cloud deployment models and service models in Azure?

**Deployment Models:**

| Model | Description | Example |
|-------|-------------|---------|
| **Public Cloud** | Resources on Microsoft\'s shared infrastructure | Azure VMs, App Service |
| **Private Cloud** | Dedicated infrastructure — Azure Stack Hub on-premises | Regulated industries |
| **Hybrid Cloud** | Mix of public + private, connected by Azure Arc or VPN/ExpressRoute | Enterprise migrations |
| **Multi-Cloud** | Azure + AWS/GCP, managed via Azure Arc | Vendor diversity |

**Service Models (IaaS / PaaS / SaaS):**

```
IaaS — Azure Virtual Machines, Azure VNet, Azure Disks
         "You manage: OS, middleware, runtime, data, applications"

PaaS — Azure App Service, Azure Functions, Azure SQL Database
         "You manage: data and applications only"

SaaS — Microsoft 365, Dynamics 365, Azure DevOps
         "You manage: data and user access only"
```

**When to choose:**

- **IaaS** → Lift-and-shift migrations, full OS control needed, custom software stacks.
- **PaaS** → Web apps, APIs, functions — focus on code not infrastructure.
- **SaaS** → Productivity tools; no infrastructure management at all.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the benefits of cloud computing with Azure?

| Benefit | Azure Feature |
|---------|--------------|
| **Scalability** | Azure Autoscale, VM Scale Sets — scale out in minutes |
| **High Availability** | 99.99% SLA with Availability Zones; Azure Site Recovery |
| **Global reach** | 60+ regions, 200+ edge nodes (Azure CDN/Front Door) |
| **OpEx over CapEx** | Pay-as-you-go billing; Reserved Instances for predictable workloads |
| **Security & Compliance** | 100+ compliance certifications (ISO 27001, SOC 2, HIPAA, FedRAMP) |
| **Elasticity** | Scale up in peak hours, scale down at night — no idle hardware cost |
| **Managed services** | PaaS eliminates OS patching, hardware replacement |
| **DevOps integration** | Azure DevOps, GitHub Actions, Azure Pipelines native integration |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you manage resources in Azure?

Azure provides multiple management planes:

**1. Azure Portal** — graphical browser-based management at `portal.azure.com`

**2. Azure CLI:**

```bash
# List all VMs in a resource group
az vm list --resource-group rg-myapp-prod --output table

# Start a VM
az vm start --resource-group rg-myapp-prod --name vm-webserver

# Scale App Service plan
az appservice plan update \
  --name asp-myapp \
  --resource-group rg-myapp-prod \
  --sku P2V3
```

**3. Azure PowerShell:**

```powershell
# Connect to Azure
Connect-AzAccount

# Get all resources in a group
Get-AzResource -ResourceGroupName "rg-myapp-prod" | Format-Table Name, ResourceType

# Deploy an ARM/Bicep template
New-AzResourceGroupDeployment `
  -ResourceGroupName "rg-myapp-prod" `
  -TemplateFile "./main.bicep" `
  -Verbose
```

**4. Azure Resource Manager (ARM) / Bicep / Terraform** — infrastructure as code (recommended for production).

**5. Azure Cloud Shell** — browser-based bash/PowerShell with pre-installed tools at `shell.azure.com`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Microsoft Azure and why do companies use it?

**Microsoft Azure** is a public cloud computing platform by Microsoft offering on-demand resources (compute, storage, networking, AI, databases) over the internet on a pay-as-you-go model.

**Reasons companies adopt Azure:**

| Reason | Explanation |
|--------|-------------|
| **Cost savings** | No upfront hardware capital; pay only for what you use |
| **Scalability** | Scale up or out in minutes to handle traffic spikes |
| **Global reach** | 60+ regions worldwide; deploy close to end users |
| **Security & compliance** | ISO, SOC, HIPAA, FedRAMP certifications built-in |
| **Integrated ecosystem** | Deep integration with Microsoft 365, Teams, Windows Server |
| **Hybrid support** | Azure Arc and Azure Stack extend cloud to on-premises |

```bash
# Check available Azure regions
az account list-locations --output table
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a Region and an Availability Zone in Azure?

| Concept | Definition | Example |
|---------|-----------|---------|
| **Region** | A geographic cluster of one or more datacenters (e.g., East US, West Europe) | `eastus`, `westeurope` |
| **Availability Zone (AZ)** | Physically separate datacenters inside a region, each with independent power/cooling/networking | Zone 1, Zone 2, Zone 3 in East US |
| **Region Pair** | Two paired regions for disaster recovery (automatically fail over certain services) | East US ↔ West US |

**Key points:**
- Resources deployed across AZs survive a single datacenter failure.
- Not all regions have AZs; check with `az account list-locations`.
- Services like Azure SQL, Storage, and VMs offer zone-redundant SKUs.

```bash
# Deploy a zone-redundant public IP across AZs
az network public-ip create \
  --name pip-webapp \
  --resource-group rg-demo \
  --sku Standard \
  --zone 1 2 3
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a Resource Group and why is it important?

A **Resource Group** is a logical container that holds related Azure resources sharing the same lifecycle, permissions, and billing context.

**Rules:**
- Every resource must belong to exactly one resource group.
- Deleting a resource group deletes all resources inside it.
- Resources in different regions can belong to the same resource group.
- RBAC and policies can be applied at the resource group scope.

```bash
# Create a resource group
az group create \
  --name rg-webapp-prod \
  --location eastus \
  --tags Environment=Production Owner=DevTeam

# List all resource groups
az group list --output table

# Delete a resource group (and all resources inside it)
az group delete --name rg-webapp-prod --yes --no-wait
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a Subscription and a Management Group?

| Concept | Purpose | Scope |
|---------|---------|-------|
| **Subscription** | Billing boundary; groups resources under a single bill | All resources inside a subscription |
| **Management Group** | Governance boundary; applies policies/RBAC to multiple subscriptions | One or many subscriptions |
| **Tenant (Root)** | The top-level Azure Active Directory (Entra ID) organization | All subscriptions and management groups |

```
Tenant (Entra ID)
└── Root Management Group
    ├── Management Group: Production
    │   ├── Subscription: App-Team-A
    │   └── Subscription: App-Team-B
    └── Management Group: Development
        └── Subscription: Dev-Sandbox
```

```bash
# Create a management group
az account management-group create --name "mg-production" --display-name "Production"

# Move a subscription into a management group
az account management-group subscription add \
  --name "mg-production" \
  --subscription "<subscription-id>"
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-setup-guide/media/organize-resources/scope-levels.png" alt="Azure Management Group and Subscription Hierarchy" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Virtual Machine (VM) and when should you use it?

An **Azure VM** is an IaaS (Infrastructure as a Service) offering — a virtual computer in the cloud running Windows or Linux. You manage the OS, runtime, and application; Azure manages the physical host.

**When to use a VM:**
- Lift-and-shift of on-premises applications.
- Custom OS configuration requirements.
- Running software that cannot run on PaaS services.
- Legacy applications not compatible with containers.

```bash
# Create a Linux VM (Ubuntu)
az vm create \
  --name vm-webserver \
  --resource-group rg-demo \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --public-ip-sku Standard

# Start / Stop / Deallocate a VM
az vm start  --name vm-webserver --resource-group rg-demo
az vm stop   --name vm-webserver --resource-group rg-demo
az vm deallocate --name vm-webserver --resource-group rg-demo  # stops billing for compute
```

> **Tip:** Deallocate (not just stop) a VM to stop compute charges. A stopped-but-allocated VM still incurs compute costs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure App Service and what are its main features?

**Azure App Service** is a fully managed PaaS for hosting web applications, REST APIs, and mobile backends. No OS or server management required.

| Feature | Detail |
|---------|--------|
| Supported runtimes | .NET, Node.js, Python, Java, PHP, Ruby |
| Deployment | Git, GitHub Actions, Azure DevOps, ZIP deploy |
| Built-in SSL | Automatic HTTPS with custom domains |
| Auto-scaling | Scale out/in based on CPU, memory, or schedules |
| Deployment slots | Blue-green deployments with zero downtime |
| Managed Identity | Built-in identity — no credentials in code |

```bash
# Create an App Service Plan (hosting infrastructure)
az appservice plan create \
  --name asp-webapp \
  --resource-group rg-demo \
  --sku B1 \
  --is-linux

# Create a web app on the plan
az webapp create \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --plan asp-webapp \
  --runtime "NODE:20-lts"

# Deploy from a ZIP file
az webapp deploy \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --src-path ./dist.zip \
  --type zip
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main Azure Storage types and when do you use each?

| Storage Type | Description | Use Case |
|-------------|-------------|----------|
| **Blob Storage** | Object storage for unstructured data | Images, videos, backups, logs |
| **File Storage** | Managed SMB/NFS file shares | Shared drives, lift-and-shift NAS |
| **Queue Storage** | Message queue (max 64 KB/message) | Decoupling app components |
| **Table Storage** | Key-value NoSQL store | Semi-structured data, telemetry |
| **Disk Storage** | Managed disks for VMs | OS and data disks for Azure VMs |

```bash
# Create a storage account
az storage account create \
  --name stmydemo2025 \
  --resource-group rg-demo \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2

# Create a blob container and upload a file
az storage container create \
  --name uploads \
  --account-name stmydemo2025 \
  --auth-mode login

az storage blob upload \
  --account-name stmydemo2025 \
  --container-name uploads \
  --name myfile.txt \
  --file ./myfile.txt \
  --auth-mode login
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a Virtual Network (VNet) and a Subnet in Azure?

A **Virtual Network (VNet)** is an isolated private network in Azure. Resources inside a VNet can communicate with each other by default but are isolated from other VNets and the internet unless explicitly configured otherwise.

A **Subnet** divides the VNet address space into smaller segments, allowing you to apply separate security rules (NSGs) and route tables to different resource tiers (web, app, database).

```bash
# Create a VNet with address space 10.0.0.0/16
az network vnet create \
  --name vnet-demo \
  --resource-group rg-demo \
  --address-prefix 10.0.0.0/16

# Add a web-tier subnet
az network vnet subnet create \
  --name snet-web \
  --vnet-name vnet-demo \
  --resource-group rg-demo \
  --address-prefix 10.0.1.0/24

# Add a database-tier subnet
az network vnet subnet create \
  --name snet-db \
  --vnet-name vnet-demo \
  --resource-group rg-demo \
  --address-prefix 10.0.2.0/24
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an SLA and what is the SLA for Azure VMs?

An **SLA (Service Level Agreement)** is Microsoft\'s commitment to uptime and connectivity for each Azure service. If the SLA is breached, customers receive service credits.

| Azure Service | SLA |
|--------------|-----|
| Single VM with Premium SSD | **99.9%** |
| VMs across 2+ Availability Zones | **99.99%** |
| Azure App Service (Standard+) | **99.95%** |
| Azure SQL Database (Business Critical) | **99.99%** |
| Azure Blob Storage (RA-GRS) | **99.99%** read, **99.9%** write |

**Downtime calculations:**
- 99.9% SLA = ~8.7 hours downtime per year
- 99.99% SLA = ~52 minutes downtime per year
- 99.999% SLA = ~5 minutes downtime per year

> **Tip:** Combining services in series multiplies the SLAs: `0.999 × 0.999 = 0.998` (composite SLA). Always use zone-redundant or geo-redundant deployments for production workloads.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between IaaS, PaaS, and SaaS in Azure?

| Model | You Manage | Azure Manages | Azure Example |
|-------|-----------|--------------|---------------|
| **IaaS** (Infrastructure as a Service) | OS, runtime, app, data | Hardware, network, hypervisor | Azure VMs, Azure Disk |
| **PaaS** (Platform as a Service) | App code, data | OS, runtime, middleware, infrastructure | App Service, Azure SQL, AKS |
| **SaaS** (Software as a Service) | User access and data | Everything | Microsoft 365, Dynamics 365 |

```
On-premises → IaaS → PaaS → SaaS
More control ←──────────────→ Less management
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What Azure pricing models are available and how do you estimate cost?

| Model | Description | Best For |
|-------|-------------|----------|
| **Pay-as-you-go** | Billed per second/hour of usage; no commitment | Dev/test, variable workloads |
| **Reserved Instances** | 1- or 3-year commitment; up to 72% savings | Predictable, steady-state production |
| **Spot VMs** | Use spare Azure capacity at up to 90% discount; can be evicted | Batch jobs, fault-tolerant workloads |
| **Azure Hybrid Benefit** | Use existing Windows/SQL Server licenses on Azure | Customers with on-prem Microsoft licenses |
| **Dev/Test pricing** | Discounted rates for non-production subscriptions | Developer sandboxes |

```bash
# Use Azure Pricing Calculator (web): https://azure.microsoft.com/pricing/calculator/

# View current spending in CLI
az consumption usage list \
  --start-date 2025-05-01 \
  --end-date 2025-05-31 \
  --output table

# Create a budget alert at $500/month
az consumption budget create \
  --budget-name monthly-budget \
  --amount 500 \
  --category Cost \
  --time-grain Monthly \
  --start-date 2025-05-01 \
  --end-date 2026-05-01 \
  --notification-threshold 80
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. CORE SERVICES

<br>

## Q. What are the main compute services in Azure and when do you choose each?

| Service | Best For | Billing |
|---------|----------|---------|
| **Azure Virtual Machines** | Lift-and-shift, full OS control, custom software | Per-second (CPU+RAM+disk) |
| **Azure App Service** | Web apps, REST APIs, mobile backends | Per plan tier |
| **Azure Functions** | Event-driven, short-lived tasks, microservices | Per execution / consumption |
| **Azure Container Instances (ACI)** | Single containers, batch jobs, dev/test | Per second (CPU+RAM) |
| **Azure Kubernetes Service (AKS)** | Containerized microservices at scale | VMs in node pool |
| **Azure Batch** | Large-scale parallel HPC/ML jobs | Pay for compute used |

**Azure Functions (serverless) example — HTTP trigger:**

```csharp
// Azure Functions v4 (.NET 8 Isolated Worker)
[Function("GetProduct")]
public async Task<HttpResponseData> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = "products/{id}")] HttpRequestData req,
    int id,
    FunctionContext context)
{
    var logger = context.GetLogger("GetProduct");
    logger.LogInformation("Fetching product {Id}", id);

    var product = await _productService.GetByIdAsync(id);
    if (product is null)
    {
        var notFound = req.CreateResponse(HttpStatusCode.NotFound);
        return notFound;
    }

    var response = req.CreateResponse(HttpStatusCode.OK);
    await response.WriteAsJsonAsync(product);
    return response;
}
```

**Azure App Service deployment (GitHub Actions):**

```yaml
- name: Deploy to Azure Web App
  uses: azure/webapps-deploy@v3
  with:
    app-name: 'myapp-api'
    slot-name: 'staging'
    publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
    package: ./publish
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Kubernetes Service (AKS) and how do you deploy to it?

**AKS** is a managed Kubernetes service where Azure handles the control plane (API server, etcd, scheduler) at no cost — you only pay for worker node VMs. AKS integrates with Microsoft Entra ID, Azure Monitor, Azure Container Registry, and Azure CNI networking.

**Create an AKS cluster:**

```bash
# Create ACR for images
az acr create --name myappacr --resource-group rg-myapp --sku Basic

# Create AKS cluster with workload identity and monitoring
az aks create \
  --resource-group rg-myapp \
  --name aks-myapp \
  --node-count 3 \
  --node-vm-size Standard_D4s_v5 \
  --enable-addons monitoring \
  --enable-oidc-issuer \
  --enable-workload-identity \
  --attach-acr myappacr \
  --generate-ssh-keys

# Get credentials
az aks get-credentials --resource-group rg-myapp --name aks-myapp
```

**Deploy an application:**

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp-api
  template:
    metadata:
      labels:
        app: myapp-api
    spec:
      containers:
      - name: myapp-api
        image: myappacr.azurecr.io/myapp-api:latest
        ports:
        - containerPort: 80
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:  { cpu: "500m", memory: "512Mi" }
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-api-svc
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: myapp-api
```

```bash
kubectl apply -f deployment.yaml
kubectl get pods
kubectl get service myapp-api-svc
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/aks/media/concepts-clusters-workloads/control-plane-and-nodes.png" alt="AKS Cluster Architecture" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Service Bus and when would you use it over Azure Storage Queues?

**Azure Service Bus** is an enterprise messaging broker supporting queues (point-to-point) and topics/subscriptions (publish-subscribe), with features like message sessions, dead-letter queues, duplicate detection, and transactions.

| Feature | Service Bus | Storage Queues |
|---------|-------------|----------------|
| Max message size | 100 MB (Premium) | 64 KB |
| Message ordering | Sessions (FIFO) | Best-effort |
| Dead-letter queue | Built-in | No |
| Publish-Subscribe | Topics | No |
| Transactions | Yes | No |
| Max TTL | Unlimited | 7 days |
| Throughput | Millions/sec (Premium) | Very high |

**Use Service Bus for** enterprise workflows, ordered processing, dead-letter handling. **Use Storage Queues for** simple decoupling, very large backlogs, cost-sensitive workloads.

**Sending and receiving messages (.NET 8):**

```csharp
// Send a message
await using var client = new ServiceBusClient(connectionString);
ServiceBusSender sender = client.CreateSender("orders-queue");

var order = new OrderMessage { OrderId = 42, CustomerId = "C001" };
var message = new ServiceBusMessage(JsonSerializer.SerializeToUtf8Bytes(order))
{
    MessageId = Guid.NewGuid().ToString(),
    Subject = "NewOrder",
    TimeToLive = TimeSpan.FromHours(24)
};
await sender.SendMessageAsync(message);

// Receive and process
ServiceBusProcessor processor = client.CreateProcessor("orders-queue",
    new ServiceBusProcessorOptions { MaxConcurrentCalls = 4 });

processor.ProcessMessageAsync += async args =>
{
    var body = args.Message.Body.ToObjectFromJson<OrderMessage>();
    await _orderService.ProcessAsync(body);
    await args.CompleteMessageAsync(args.Message);
};
processor.ProcessErrorAsync += args =>
{
    _logger.LogError(args.Exception, "Service Bus error");
    return Task.CompletedTask;
};
await processor.StartProcessingAsync();
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/service-bus-messaging/media/service-bus-messaging-overview/about-service-bus-queue.png" alt="Azure Service Bus Queue Architecture" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do Azure Service Bus Topics and Subscriptions work?

**Topics** enable publish-subscribe (fan-out) messaging — one message published to a topic is delivered to **all subscriptions**. Each subscription receives an independent copy and can apply **filter rules** to receive only relevant messages.

```
Publisher → Topic → Subscription A (all orders)     → Consumer A
                 → Subscription B (only high-value) → Consumer B
                 → Subscription C (only US region)  → Consumer C
```

**Create a topic and subscriptions via CLI:**

```bash
# Create Service Bus namespace (Standard tier supports topics)
az servicebus namespace create \
  --name sb-myapp \
  --resource-group rg-myapp \
  --location eastus \
  --sku Standard

# Create topic
az servicebus topic create \
  --namespace-name sb-myapp \
  --resource-group rg-myapp \
  --name orders

# Create subscription for all orders (no filter)
az servicebus topic subscription create \
  --namespace-name sb-myapp \
  --resource-group rg-myapp \
  --topic-name orders \
  --name inventory-service

# Create subscription with SQL filter — high-value orders only
az servicebus topic subscription create \
  --namespace-name sb-myapp \
  --resource-group rg-myapp \
  --topic-name orders \
  --name finance-service

az servicebus topic subscription rule create \
  --namespace-name sb-myapp \
  --resource-group rg-myapp \
  --topic-name orders \
  --subscription-name finance-service \
  --name high-value-filter \
  --filter-sql-expression "OrderTotal > 1000"
```

**Publish to a topic and receive from a subscription (.NET 8):**

```csharp
await using var client = new ServiceBusClient(
    "sb-myapp.servicebus.windows.net",
    new DefaultAzureCredential());

// Publisher — send to topic
ServiceBusSender sender = client.CreateSender("orders");

var order = new { OrderId = "ORD-001", CustomerId = "C001", OrderTotal = 1500.00, Region = "US" };
var message = new ServiceBusMessage(JsonSerializer.SerializeToUtf8Bytes(order))
{
    MessageId = Guid.NewGuid().ToString(),
    ApplicationProperties =
    {
        ["OrderTotal"] = 1500.00,   // used for SQL filter evaluation
        ["Region"]     = "US"
    }
};
await sender.SendMessageAsync(message);

// Subscriber — receive from a specific subscription
ServiceBusProcessor processor = client.CreateProcessor(
    topicName: "orders",
    subscriptionName: "inventory-service",
    new ServiceBusProcessorOptions { MaxConcurrentCalls = 2 });

processor.ProcessMessageAsync += async args =>
{
    var body = args.Message.Body.ToObjectFromJson<dynamic>();
    Console.WriteLine($"Inventory received order: {body}");
    await args.CompleteMessageAsync(args.Message);
};
processor.ProcessErrorAsync += args =>
{
    Console.Error.WriteLine($"Error: {args.Exception.Message}");
    return Task.CompletedTask;
};
await processor.StartProcessingAsync();
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/service-bus-messaging/media/service-bus-messaging-overview/about-service-bus-queue.png" alt="Azure Service Bus Topics and Subscriptions" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle message sessions in Azure Service Bus?

**Sessions** guarantee FIFO (first-in, first-out) delivery and exclusive processing for a group of related messages sharing the same `SessionId`. Only one consumer at a time holds the session lock — ensuring ordered, stateful processing.

**Use cases:** order line items that must be processed in sequence, multi-step workflows per customer, stateful saga orchestration.

```bash
# Create a session-enabled queue
az servicebus queue create \
  --namespace-name sb-myapp \
  --resource-group rg-myapp \
  --name orders-sequenced \
  --requires-session true
```

```csharp
await using var client = new ServiceBusClient(
    "sb-myapp.servicebus.windows.net",
    new DefaultAzureCredential());

// Send session messages (all steps for order ORD-001 share the same SessionId)
ServiceBusSender sender = client.CreateSender("orders-sequenced");

foreach (var step in new[] { "Created", "PaymentVerified", "Packed", "Shipped" })
{
    await sender.SendMessageAsync(new ServiceBusMessage($"Order ORD-001: {step}")
    {
        SessionId  = "ORD-001",   // groups messages — guarantees FIFO for this session
        MessageId  = Guid.NewGuid().ToString(),
        Subject    = step
    });
}

// Receive sessions — SDK locks one session at a time per consumer
ServiceBusSessionProcessor sessionProcessor = client.CreateSessionProcessor(
    "orders-sequenced",
    new ServiceBusSessionProcessorOptions { MaxConcurrentSessions = 4 });

sessionProcessor.ProcessMessageAsync += async args =>
{
    // args.SessionId identifies which order this message belongs to
    Console.WriteLine($"[{args.SessionId}] Processing: {args.Message.Body}");

    // Optionally store/retrieve session state (persistent across messages in the session)
    var state = await args.GetSessionStateAsync();
    await args.SetSessionStateAsync(
        BinaryData.FromObjectAsJson(new { LastStep = args.Message.Subject }));

    await args.CompleteMessageAsync(args.Message);
};
sessionProcessor.ProcessErrorAsync += args =>
{
    Console.Error.WriteLine(args.Exception.Message);
    return Task.CompletedTask;
};
await sessionProcessor.StartProcessingAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the Dead-Letter Queue (DLQ) work in Azure Service Bus?

The **Dead-Letter Queue (DLQ)** is a sub-queue automatically created for every queue and subscription. Messages are moved to the DLQ when:
- **Max delivery count exceeded** — processed and abandoned too many times (default: 10).
- **Message TTL expired** — if `EnableDeadLetteringOnMessageExpiration` is enabled.
- **Filter evaluation errors** — subscription filter throws an exception.
- **Explicitly dead-lettered** — application calls `DeadLetterMessageAsync()`.

```bash
# Enable dead-lettering on TTL expiry for a queue
az servicebus queue update \
  --namespace-name sb-myapp \
  --resource-group rg-myapp \
  --name orders-queue \
  --dead-lettering-on-message-expiration true \
  --max-delivery-count 5
```

```csharp
await using var client = new ServiceBusClient(
    "sb-myapp.servicebus.windows.net",
    new DefaultAzureCredential());

// Normal processor — explicitly dead-letter a poison message
ServiceBusProcessor processor = client.CreateProcessor("orders-queue");

processor.ProcessMessageAsync += async args =>
{
    try
    {
        var order = args.Message.Body.ToObjectFromJson<Order>();

        if (order.CustomerId is null)
        {
            // Explicitly move to DLQ with a reason
            await args.DeadLetterMessageAsync(
                args.Message,
                deadLetterReason: "MissingCustomerId",
                deadLetterErrorDescription: $"Order {order.OrderId} has no CustomerId");
            return;
        }

        await _orderService.ProcessAsync(order);
        await args.CompleteMessageAsync(args.Message);
    }
    catch (Exception ex)
    {
        // Abandon — delivery count increments; auto-DLQ after MaxDeliveryCount
        await args.AbandonMessageAsync(args.Message,
            new Dictionary<string, object> { ["LastError"] = ex.Message });
    }
};

// Read and reprocess DLQ messages
ServiceBusReceiver dlqReceiver = client.CreateReceiver(
    "orders-queue",
    new ServiceBusReceiverOptions
    {
        SubQueue = SubQueue.DeadLetter,
        ReceiveMode = ServiceBusReceiveMode.PeekLock
    });

var dlqMessages = await dlqReceiver.ReceiveMessagesAsync(maxMessages: 20);
foreach (var msg in dlqMessages)
{
    Console.WriteLine($"DLQ reason: {msg.DeadLetterReason} | {msg.DeadLetterErrorDescription}");
    // Fix and re-send, or log and complete
    await dlqReceiver.CompleteMessageAsync(msg);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement scheduled and deferred messages in Azure Service Bus?

**Scheduled messages** are enqueued now but only become available to consumers at a future `ScheduledEnqueueTime`. **Deferred messages** are received but intentionally set aside — they remain in the queue and must be retrieved later by `SequenceNumber`.

```csharp
await using var client = new ServiceBusClient(
    "sb-myapp.servicebus.windows.net",
    new DefaultAzureCredential());

ServiceBusSender sender = client.CreateSender("orders-queue");

// --- Scheduled message: send now, deliver in 2 hours ---
var reminder = new ServiceBusMessage("Send invoice reminder")
{
    MessageId              = Guid.NewGuid().ToString(),
    ScheduledEnqueueTime   = DateTimeOffset.UtcNow.AddHours(2)
};
long sequenceNumber = await sender.ScheduleMessageAsync(reminder, reminder.ScheduledEnqueueTime);
Console.WriteLine($"Scheduled with sequence: {sequenceNumber}");

// Cancel a scheduled message before it is delivered
await sender.CancelScheduledMessageAsync(sequenceNumber);

// --- Deferred message: receive, defer, retrieve later ---
ServiceBusReceiver receiver = client.CreateReceiver("orders-queue");

// Receive a message and defer it (keeps it in queue, invisible to normal receivers)
ServiceBusReceivedMessage msg = await receiver.ReceiveMessageAsync();
long deferredSeq = msg.SequenceNumber;
await receiver.DeferMessageAsync(msg,
    new Dictionary<string, object> { ["DeferredReason"] = "AwaitingExternalApproval" });

// Later: retrieve the deferred message directly by its sequence number
ServiceBusReceivedMessage deferred = await receiver.ReceiveDeferredMessageAsync(deferredSeq);
Console.WriteLine($"Processing deferred: {deferred.Body}");
await receiver.CompleteMessageAsync(deferred);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure Service Bus with Azure Functions?

**Azure Functions** has a built-in `ServiceBusTrigger` binding that invokes a function whenever a message arrives on a queue or topic subscription — no polling loop required.

```csharp
// Program.cs — isolated worker setup
var host = new HostBuilder()
    .ConfigureFunctionsWebApplication()
    .ConfigureServices(services =>
    {
        services.AddApplicationInsightsTelemetryWorkerService();
        services.ConfigureFunctionsApplicationInsights();
        services.AddScoped<IOrderService, OrderService>();
    })
    .Build();
await host.RunAsync();

// OrderProcessor.cs — queue trigger
public class OrderProcessor(IOrderService orderService, ILogger<OrderProcessor> logger)
{
    // Trigger on queue message; auto-completes on success, abandons on exception
    [Function("ProcessOrder")]
    public async Task Run(
        [ServiceBusTrigger(
            queueName: "orders-queue",
            Connection = "ServiceBusConnection")] // App Setting name
        ServiceBusReceivedMessage message,
        ServiceBusMessageActions messageActions)
    {
        logger.LogInformation("Processing order {MessageId}", message.MessageId);

        var order = message.Body.ToObjectFromJson<Order>();
        await orderService.ProcessAsync(order);

        // Explicit complete (use when autoCompleteMessages = false)
        await messageActions.CompleteMessageAsync(message);
    }
}

// TopicSubscriptionProcessor.cs — topic subscription trigger
public class InvoiceProcessor(ILogger<InvoiceProcessor> logger)
{
    [Function("GenerateInvoice")]
    [ServiceBusOutput("invoices-queue", Connection = "ServiceBusConnection")] // output binding
    public async Task<string> Run(
        [ServiceBusTrigger(
            topicName: "orders",
            subscriptionName: "finance-service",
            Connection = "ServiceBusConnection")]
        ServiceBusReceivedMessage message)
    {
        var order = message.Body.ToObjectFromJson<Order>();
        logger.LogInformation("Generating invoice for order {OrderId}", order.OrderId);

        // Return value is sent to invoices-queue via output binding
        return JsonSerializer.Serialize(new Invoice { OrderId = order.OrderId, Amount = order.Total });
    }
}
```

```json
// local.settings.json
{
  "Values": {
    "ServiceBusConnection": "Endpoint=sb://sb-myapp.servicebus.windows.net/;SharedAccessKeyName=..."
  }
}
```

```bash
# Grant the Function\'s Managed Identity the Service Bus Data Receiver role
az role assignment create \
  --assignee $(az functionapp identity show --name func-myapp --resource-group rg-myapp --query principalId -o tsv) \
  --role "Azure Service Bus Data Receiver" \
  --scope $(az servicebus namespace show --name sb-myapp --resource-group rg-myapp --query id -o tsv)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Redis Cache and what are its use cases?

**Azure Cache for Redis** is a fully managed, in-memory data store based on open-source Redis. It is used to accelerate data access for applications by caching frequently read data, storing session state, and enabling pub/sub messaging.

**Common tiers:**
- **Basic** — single node, no SLA, dev/test only
- **Standard** — two-node (primary + replica), 99.9% SLA
- **Premium** — clustering, persistence, VNet injection, geo-replication
- **Enterprise** — Redis Enterprise engine, active geo-replication, 99.999% SLA

**Usage in ASP.NET Core (.NET 8):**

```csharp
// Program.cs
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = builder.Configuration["Redis:ConnectionString"];
    options.InstanceName = "myapp:";
});

// ProductService.cs
public async Task<Product?> GetProductAsync(int id)
{
    string cacheKey = $"product:{id}";

    // Try cache first
    var cached = await _cache.GetStringAsync(cacheKey);
    if (cached is not null)
        return JsonSerializer.Deserialize<Product>(cached);

    // Cache miss — query database
    var product = await _db.Products.FindAsync(id);
    if (product is not null)
    {
        await _cache.SetStringAsync(
            cacheKey,
            JsonSerializer.Serialize(product),
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(15)
            });
    }
    return product;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure API Management and what are its benefits?

**Azure API Management (APIM)** is a fully managed gateway that sits in front of backend APIs, providing:

- **Security** — authentication (OAuth 2.0, subscriptions keys, JWT validation), rate limiting
- **Transformation** — request/response modification, protocol translation (REST ↔ SOAP)
- **Developer portal** — auto-generated API documentation and try-it-out console
- **Analytics** — traffic insights, error rates, latency via Azure Monitor
- **Versioning** — API versions and revisions without breaking clients

**Policy example — JWT validation and rate limiting:**

```xml
<policies>
  <inbound>
    <!-- Validate Bearer token from Microsoft Entra ID -->
    <validate-jwt header-name="Authorization" failed-validation-httpcode="401">
      <openid-config url="https://login.microsoftonline.com/{tenant-id}/v2.0/.well-known/openid-configuration"/>
      <audiences>
        <audience>api://my-api-app-id</audience>
      </audiences>
    </validate-jwt>

    <!-- Rate limit: 100 calls per minute per subscription key -->
    <rate-limit-by-key calls="100" renewal-period="60"
      counter-key="@(context.Subscription?.Key ?? 'anonymous')" />

    <!-- Add correlation ID for tracing -->
    <set-header name="X-Correlation-Id" exists-action="skip">
      <value>@(context.RequestId.ToString())</value>
    </set-header>

    <base />
  </inbound>
  <backend><base /></backend>
  <outbound>
    <!-- Remove internal headers before returning to client -->
    <set-header name="X-Powered-By" exists-action="delete"/>
    <base />
  </outbound>
</policies>
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/api-management/media/api-management-key-concepts/api-management-components.png" alt="Azure API Management Components" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Batch and how does it support large-scale parallel workloads?

**Azure Batch** is a managed HPC service that automatically provisions, scales, and manages pools of VMs to run large-scale parallel and high-performance computing jobs — no cluster management needed.

```bash
# Create a Batch account
az batch account create \
  --name mybatchaccount \
  --resource-group rg-batch \
  --location eastus \
  --storage-account mystorageaccount

# Create a pool of compute nodes
az batch pool create \
  --id mypool \
  --vm-size Standard_D4s_v5 \
  --target-dedicated-nodes 10 \
  --image canonical:0001-com-ubuntu-server-focal:20_04-lts \
  --node-agent-sku-id "batch.node.ubuntu 20.04"

# Create and submit a job
az batch job create --id myjob --pool-id mypool

# Add a task
az batch task create \
  --job-id myjob \
  --task-id task1 \
  --command-line "python process_data.py --input data1.csv"
```

**Use cases:** genomic sequencing, financial risk modelling, rendering, ML hyperparameter tuning, video transcoding.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When should you choose App Service vs Azure VM vs Azure Functions?

| Criteria | Azure VM | App Service | Azure Functions |
|----------|----------|-------------|-----------------|
| Control | Full OS control | Managed runtime | No infrastructure |
| Scaling | Manual or VMSS | Auto-scale (min 1) | Auto-scale to 0 |
| Billing | Per hour (even idle) | Per App Service Plan | Per execution (Consumption) |
| Best for | Legacy apps, custom OS | Web apps, REST APIs | Event-driven, short tasks |
| Cold start | No | No | Yes (Consumption plan) |
| Max execution | Unlimited | Unlimited | 10 min default (Consumption) |

**Example: Azure Function with HTTP trigger (.NET 8 isolated)**

```csharp
[Function("GetProduct")]
public IActionResult Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = "products/{id}")] HttpRequest req,
    int id)
{
    var product = _catalog.GetById(id);
    return product is null ? new NotFoundResult() : new OkObjectResult(product);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Azure Function triggers and bindings? Give examples.

**Triggers** define how a Function is invoked (one trigger per function). **Bindings** are declarative connections to data sources/sinks (input and output) — no manual SDK calls needed.

| Trigger | Invoked by |
|---------|-----------|
| `HttpTrigger` | HTTP request |
| `TimerTrigger` | CRON schedule |
| `BlobTrigger` | New blob in a container |
| `ServiceBusTrigger` | Message on Service Bus queue/topic |
| `EventGridTrigger` | Event from Event Grid |
| `CosmosDBTrigger` | Change feed in Cosmos DB |

```csharp
// Blob trigger → reads new file → writes to Cosmos DB (output binding)
[Function("ProcessUpload")]
[CosmosDBOutput("mydb", "processed", Connection = "CosmosConnection", CreateIfNotExists = true)]
public FileRecord Run(
    [BlobTrigger("uploads/{name}", Connection = "AzureWebJobsStorage")] Stream blobStream,
    string name,
    ILogger log)
{
    log.LogInformation($"Processing blob: {name}, Size: {blobStream.Length}");
    return new FileRecord { Id = name, ProcessedAt = DateTime.UtcNow, SizeBytes = blobStream.Length };
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Blob Storage tiers and when would you use each?

| Tier | Access Frequency | Latency | Cost | Min Duration |
|------|-----------------|---------|------|-------------|
| **Hot** | Frequent | Milliseconds | Highest storage | None |
| **Cool** | Infrequent (~once/month) | Milliseconds | ~50% less storage | 30 days |
| **Cold** | Rare (~once/quarter) | Milliseconds | ~60% less storage | 90 days |
| **Archive** | Almost never (compliance) | Hours (rehydrate first) | Lowest storage | 180 days |

```bash
# Set access tier when uploading
az storage blob upload \
  --account-name stmydemo2025 \
  --container-name backups \
  --name archive/backup-2024.zip \
  --file backup-2024.zip \
  --tier Cool

# Change tier of an existing blob
az storage blob set-tier \
  --account-name stmydemo2025 \
  --container-name backups \
  --name archive/backup-2024.zip \
  --tier Archive

# Rehydrate from Archive (takes hours — set priority)
az storage blob set-tier \
  --account-name stmydemo2025 \
  --container-name backups \
  --name archive/backup-2024.zip \
  --tier Hot \
  --rehydrate-priority High
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a SAS token and how do you generate one?

A **Shared Access Signature (SAS)** is a URI that grants restricted, time-limited access to Azure Storage resources without exposing the account key. You can scope it to specific resources, permissions, and time windows.

**Types of SAS:**
- **Account SAS** — access across services (Blob, File, Queue, Table)
- **Service SAS** — access to a specific service resource
- **User Delegation SAS** — signed with Microsoft Entra credentials (most secure)

```bash
# Generate a Blob SAS token (read-only, 1-hour expiry)
az storage blob generate-sas \
  --account-name stmydemo2025 \
  --container-name uploads \
  --name report.pdf \
  --permissions r \
  --expiry $(date -u -d '+1 hour' '+%Y-%m-%dT%H:%MZ') \
  --output tsv

# Generate a User Delegation SAS (recommended — no account key needed)
az storage blob generate-sas \
  --account-name stmydemo2025 \
  --container-name uploads \
  --name report.pdf \
  --permissions r \
  --expiry 2025-06-01T00:00Z \
  --as-user \
  --auth-mode login
```

```csharp
// .NET: Generate SAS URI using SDK (User Delegation — best practice)
var delegationKey = await blobServiceClient.GetUserDelegationKeyAsync(
    DateTimeOffset.UtcNow, DateTimeOffset.UtcNow.AddHours(1));

var sasBuilder = new BlobSasBuilder(BlobSasPermissions.Read, DateTimeOffset.UtcNow.AddHours(1))
{
    BlobContainerName = "uploads",
    BlobName = "report.pdf",
    Resource = "b"
};

var sasUri = blobClient.GenerateSasUri(sasBuilder);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Managed Identity and how does it remove the need to store credentials?

A **Managed Identity** is an Azure AD identity automatically created and managed by Azure for a resource (VM, App Service, Function, AKS pod). Other services can grant it access via RBAC — no passwords, connection strings, or secrets needed in code.

```csharp
// Access Key Vault using Managed Identity — zero credentials in code
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var client = new SecretClient(
    new Uri("https://kv-myapp.vault.azure.net/"),
    new DefaultAzureCredential());   // Uses MI when running in Azure

var secret = await client.GetSecretAsync("DbConnectionString");
Console.WriteLine(secret.Value.Value);
```

```bash
# Enable system-assigned MI on an App Service
az webapp identity assign \
  --name mywebapp-demo2025 \
  --resource-group rg-demo

# Grant the app\'s identity access to Key Vault secrets
az keyvault set-policy \
  --name kv-myapp \
  --object-id $(az webapp identity show \
      --name mywebapp-demo2025 \
      --resource-group rg-demo \
      --query principalId -o tsv) \
  --secret-permissions get list
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Application Insights and how do you add it to a .NET app?

**Application Insights** is the APM (Application Performance Monitoring) service inside Azure Monitor. It collects HTTP requests, dependency calls (SQL, HTTP, Redis), exceptions, custom events, and performance counters.

```csharp
// Program.cs (.NET 8)
builder.Services.AddApplicationInsightsTelemetry(options =>
{
    options.ConnectionString = builder.Configuration["APPLICATIONINSIGHTS_CONNECTION_STRING"];
    options.EnableAdaptiveSampling = true;
    options.EnableQuickPulseMetricStream = true;  // Live Metrics
});

// Custom event and metric tracking
public class OrderService(TelemetryClient telemetry)
{
    public async Task PlaceOrderAsync(Order order)
    {
        telemetry.TrackEvent("OrderPlaced", new Dictionary<string, string>
        {
            ["OrderId"] = order.Id,
            ["CustomerId"] = order.CustomerId
        });

        telemetry.GetMetric("order.total").TrackValue((double)order.Total);
    }
}
```

```bash
# Create Application Insights resource
az monitor app-insights component create \
  --app ai-myapp \
  --resource-group rg-demo \
  --location eastus \
  --application-type web

# Get connection string
az monitor app-insights component show \
  --app ai-myapp \
  --resource-group rg-demo \
  --query connectionString -o tsv
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an NSG (Network Security Group) and how does it work?

An **NSG** is a stateful L4 firewall applied to a subnet or NIC. It contains inbound and outbound rules evaluated by priority (100–4096; lower = higher priority). A default **DenyAll** rule at priority 65500 blocks all unmatched traffic.

```bash
# Create NSG and allow HTTPS inbound
az network nsg create \
  --name nsg-webapp \
  --resource-group rg-demo

az network nsg rule create \
  --nsg-name nsg-webapp \
  --resource-group rg-demo \
  --name Allow-HTTPS \
  --priority 100 \
  --direction Inbound \
  --protocol Tcp \
  --destination-port-range 443 \
  --source-address-prefixes Internet \
  --access Allow

az network nsg rule create \
  --nsg-name nsg-webapp \
  --resource-group rg-demo \
  --name Allow-HTTP \
  --priority 110 \
  --direction Inbound \
  --protocol Tcp \
  --destination-port-range 80 \
  --source-address-prefixes Internet \
  --access Allow

# Attach NSG to subnet
az network vnet subnet update \
  --name snet-web \
  --vnet-name vnet-demo \
  --resource-group rg-demo \
  --network-security-group nsg-webapp
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Key Vault and how do you store and retrieve a secret?

**Azure Key Vault** is a cloud service for securely storing and managing secrets (passwords, API keys), encryption keys, and TLS certificates. Access is controlled via RBAC or Key Vault access policies.

```bash
# Create a Key Vault
az keyvault create \
  --name kv-myapp-demo \
  --resource-group rg-demo \
  --location eastus \
  --sku standard \
  --enable-rbac-authorization true   # use RBAC instead of legacy access policies

# Store a secret
az keyvault secret set \
  --vault-name kv-myapp-demo \
  --name "DbPassword" \
  --value "SuperSecret@2025"

# Retrieve a secret
az keyvault secret show \
  --vault-name kv-myapp-demo \
  --name "DbPassword" \
  --query value -o tsv
```

```csharp
// .NET: Load all Key Vault secrets as configuration (no code changes needed per secret)
builder.Configuration.AddAzureKeyVault(
    new Uri("https://kv-myapp-demo.vault.azure.net/"),
    new DefaultAzureCredential());

// Access like any config value
var dbPassword = builder.Configuration["DbPassword"];
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure DevOps and what are its main components?

**Azure DevOps** is Microsoft\'s end-to-end DevOps platform providing source control, CI/CD pipelines, agile planning, and artifact management.

| Component | Purpose |
|-----------|---------|
| **Azure Repos** | Git or TFVC source control |
| **Azure Pipelines** | CI/CD automation for build, test, deploy |
| **Azure Boards** | Agile planning (Epics, Stories, Tasks, Bugs) |
| **Azure Artifacts** | Package management (NuGet, npm, Maven, PyPI) |
| **Azure Test Plans** | Manual and exploratory testing management |

**Basic CI pipeline (azure-pipelines.yml):**

```yaml
trigger:
  - main

pool:
  vmImage: ubuntu-latest

variables:
  buildConfiguration: Release

steps:
  - task: UseDotNet@2
    inputs:
      packageType: sdk
      version: '8.x'

  - script: dotnet restore
    displayName: Restore packages

  - script: dotnet build --configuration $(buildConfiguration)
    displayName: Build

  - script: dotnet test --configuration $(buildConfiguration) --no-build
    displayName: Test

  - task: PublishBuildArtifacts@1
    inputs:
      pathToPublish: '$(Build.ArtifactStagingDirectory)'
      artifactName: drop
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an ARM Template and what is Bicep?

**ARM (Azure Resource Manager) templates** are JSON files that declare Azure resources. **Bicep** is a DSL that compiles to ARM JSON — it has cleaner syntax, type safety, and IDE intellisense.

```bicep
// main.bicep — deploy a storage account
param storageAccountName string = 'stmydemo${uniqueString(resourceGroup().id)}'
param location string = resourceGroup().location
param skuName string = 'Standard_LRS'

resource storageAccount 'Microsoft.Storage/storageAccounts@2023-05-01' = {
  name: storageAccountName
  location: location
  sku: { name: skuName }
  kind: 'StorageV2'
  properties: {
    supportsHttpsTrafficOnly: true
    minimumTlsVersion: 'TLS1_2'
    allowBlobPublicAccess: false
  }
}

output storageAccountId string = storageAccount.id
output blobEndpoint string = storageAccount.properties.primaryEndpoints.blob
```

```bash
# Deploy a Bicep template
az deployment group create \
  --resource-group rg-demo \
  --template-file main.bicep \
  --parameters skuName=Standard_GRS

# Validate without deploying
az deployment group validate \
  --resource-group rg-demo \
  --template-file main.bicep
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Load Balancer and Application Gateway?

| Feature | Azure Load Balancer | Azure Application Gateway |
|---------|--------------------|-----------------------------|
| OSI Layer | Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| Routing | IP + Port hash | URL path, host header, query string |
| SSL Termination | No | Yes |
| WAF (Web App Firewall) | No | Yes (WAF v2 SKU) |
| Session Affinity | Source IP | Cookie-based |
| Use case | Non-HTTP, internal VM load balancing | Web apps, API gateway, microservices |

```bash
# Create an Application Gateway with WAF
az network application-gateway create \
  --name agw-webapp \
  --resource-group rg-demo \
  --location eastus \
  --sku WAF_v2 \
  --capacity 2 \
  --vnet-name vnet-demo \
  --subnet snet-agw \
  --public-ip-address pip-agw \
  --frontend-port 443 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --routing-rule-type Basic
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/load-balancer/media/load-balancer-overview/load-balancer.svg" alt="Azure Load Balancer vs Application Gateway" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. SECURITY

<br>

## Q. What is Microsoft Entra ID (formerly Azure Active Directory) and what are its key features?

**Microsoft Entra ID** (formerly Azure AD) is Microsoft\'s cloud-based identity and access management service. It provides single sign-on, MFA, conditional access, B2B/B2C collaboration, and identity protection for Azure, Microsoft 365, and thousands of SaaS applications.

**Key features:**

| Feature | Description |
|---------|-------------|
| **Single Sign-On (SSO)** | One login for thousands of pre-integrated SaaS apps |
| **Multi-Factor Authentication** | TOTP authenticator, phone call, FIDO2, passwordless |
| **Conditional Access** | Policy-based access based on user, device, location, risk |
| **Privileged Identity Management (PIM)** | Just-in-time elevation for admin roles |
| **Identity Protection** | ML-based risk detection for compromised credentials |
| **B2B Collaboration** | Invite external partners using their own identities |
| **B2C** | Customer identity for public-facing apps (millions of users) |
| **Managed Identities** | Azure resources authenticate without credentials |

**Assign a role via Azure CLI:**

```bash
# Get the object ID of a user
az ad user show --id "user@contoso.com" --query id -o tsv

# Assign Contributor role on a resource group
az role assignment create \
  --assignee "user@contoso.com" \
  --role "Contributor" \
  --scope "/subscriptions/{sub-id}/resourceGroups/rg-myapp"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement Role-Based Access Control (RBAC) in Azure?

RBAC controls **who** (security principal) can do **what** (role definition) on **which scope** (subscription, resource group, or resource).

**Built-in roles (most common):**

| Role | Permissions |
|------|-------------|
| **Owner** | Full access + assign roles |
| **Contributor** | Create/manage all resources, cannot assign roles |
| **Reader** | View resources only |
| **User Access Administrator** | Manage user access |
| **Virtual Machine Contributor** | Manage VMs (not VNet/storage) |

**Create a custom role (Bicep):**

```json
// custom-role.json
{
  "Name": "VM Operator",
  "IsCustom": true,
  "Description": "Can start, stop, and restart VMs",
  "Actions": [
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Compute/virtualMachines/read"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscription-id}"
  ]
}
```

```bash
az role definition create --role-definition custom-role.json

az role assignment create \
  --assignee "vm-team@contoso.com" \
  --role "VM Operator" \
  --scope "/subscriptions/{sub-id}/resourceGroups/rg-prod"
```

**Best practices:**
- Apply least-privilege — assign the minimum role needed.
- Assign roles to **groups**, not individual users.
- Prefer built-in roles; use custom roles only when built-ins are insufficient.
- Use **PIM** for privileged roles — require justification and approval.

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/role-based-access-control/media/overview/rbac-overview.png" alt="Azure Role-Based Access Control (RBAC) Overview" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Key Vault and how do you use it to manage secrets?

**Azure Key Vault** is a cloud HSM-backed service for storing and accessing secrets (passwords, connection strings), keys (encryption/signing), and certificates. It eliminates hardcoded credentials in code.

**Key features:**
- Centralized secret storage with full audit log in Azure Monitor
- Automatic certificate rotation and renewal
- HSM-backed keys (Premium tier) — FIPS 140-2 Level 3
- Integration with Managed Identities — no credentials needed

**Accessing a secret from ASP.NET Core using Managed Identity:**

```csharp
// Program.cs — add Key Vault as a configuration source
builder.Configuration.AddAzureKeyVault(
    new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/"),
    new DefaultAzureCredential());  // uses Managed Identity in Azure, developer credentials locally

// Access secret like any config value
var connectionString = builder.Configuration["SqlConnectionString"];
```

**Store a secret via CLI:**

```bash
# Create Key Vault
az keyvault create \
  --name kv-myapp-prod \
  --resource-group rg-myapp \
  --location eastus \
  --enable-soft-delete true \
  --retention-days 90

# Store a secret
az keyvault secret set \
  --vault-name kv-myapp-prod \
  --name "SqlConnectionString" \
  --value "Server=sql-myapp.database.windows.net;Database=mydb;..."

# Grant access to an App Service identity
az keyvault set-policy \
  --name kv-myapp-prod \
  --object-id $(az webapp identity show --name myapp --resource-group rg-myapp --query principalId -o tsv) \
  --secret-permissions get list
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Managed Identities in Azure and how do they improve security?

A **Managed Identity** is an automatically managed identity in Microsoft Entra ID that Azure assigns to a resource (VM, App Service, AKS pod, etc.). It allows the resource to authenticate to any Azure service that supports Entra ID auth — **without any credentials stored in code or config**.

**Types:**
- **System-assigned** — tied to the lifecycle of one resource; deleted when the resource is deleted.
- **User-assigned** — independent lifecycle; can be assigned to multiple resources.

**Enable and use System-Assigned Identity on App Service:**

```bash
# Enable managed identity on the app
az webapp identity assign \
  --name myapp \
  --resource-group rg-myapp

# Grant it access to Key Vault
az keyvault set-policy \
  --name kv-myapp-prod \
  --object-id $(az webapp identity show --name myapp --resource-group rg-myapp --query principalId -o tsv) \
  --secret-permissions get

# Grant it access to Storage
az role assignment create \
  --assignee $(az webapp identity show --name myapp --resource-group rg-myapp --query principalId -o tsv) \
  --role "Storage Blob Data Reader" \
  --scope /subscriptions/{sub}/resourceGroups/rg-myapp/providers/Microsoft.Storage/storageAccounts/mystg
```

**Authenticate to Blob Storage from code — no credentials:**

```csharp
// DefaultAzureCredential automatically uses Managed Identity in Azure
var credential = new DefaultAzureCredential();
var blobClient = new BlobServiceClient(
    new Uri("https://mystg.blob.core.windows.net"),
    credential);
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/media/managed-identity-best-practice-recommendations/user-assigned-identities.png" alt="Azure Managed Identities Overview" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Defender for Cloud and how does it monitor security?

**Microsoft Defender for Cloud** (formerly Security Center) is a Cloud Security Posture Management (CSPM) and Cloud Workload Protection Platform (CWPP) that provides:

- **Secure Score** — quantified view of your security posture (0–100)
- **Security recommendations** — prioritized actions with automated remediation
- **Threat protection** — alerts for VMs, containers, databases, App Service, storage
- **Regulatory compliance dashboard** — maps controls to CIS, NIST, PCI DSS, ISO 27001

**Enable Defender for Cloud via CLI:**

```bash
# Enable Defender for Servers Plan 2
az security pricing create \
  --name VirtualMachines \
  --tier standard

# Enable for Azure SQL
az security pricing create \
  --name SqlServers \
  --tier standard

# View security recommendations
az security assessment list \
  --resource-group rg-myapp \
  --output table
```

**Azure Policy — enforce secure configuration:**

```bash
# Apply the "Azure Security Benchmark" built-in initiative
az policy assignment create \
  --name "asb-assignment" \
  --policy-set-definition "1f3afdf9-d0c9-4c3d-847f-89da613e70a8" \
  --scope "/subscriptions/{sub-id}" \
  --enforcement-mode Default
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure DDoS Protection and how does it work?

**Azure DDoS Protection** provides automatic, always-on protection against volumetric, protocol, and resource-layer attacks for Azure public IP addresses.

| Tier | Cost | Coverage |
|------|------|----------|
| **DDoS Network Protection** (formerly Standard) | ~$2,944/month per protected VNet | Full L3/L4/L7 protection, SLA, cost credits |
| **DDoS IP Protection** | ~$199/month per public IP | Single IP protection, no SLA credit |
| **Basic (default)** | Free | Infrastructure-level protection only |

**Enable via Bicep:**

```bicep
resource ddosProtectionPlan 'Microsoft.Network/ddosProtectionPlans@2023-09-01' = {
  name: 'ddos-plan-prod'
  location: location
}

resource vnet 'Microsoft.Network/virtualNetworks@2023-09-01' = {
  name: 'vnet-prod'
  location: location
  properties: {
    addressSpace: { addressPrefixes: ['10.0.0.0/16'] }
    enableDdosProtection: true
    ddosProtectionPlan: { id: ddosProtectionPlan.id }
  }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Just-In-Time (JIT) VM Access and why is it important?

**JIT VM Access** in Microsoft Defender for Cloud temporarily opens inbound ports on a VM (RDP/SSH/custom) for a defined time window, on request. When not in use, the NSG rule blocks all inbound access — eliminating the constant attack surface of open management ports.

**How it works:**

1. Admin requests access — specifies duration (1–24 hours), source IP, and port.
2. Defender for Cloud modifies the NSG to allow the specific IP for that duration.
3. After the window expires, the NSG rule is automatically removed.

```bash
# Enable JIT on a VM
az security jit-policy create \
  --resource-group rg-prod \
  --vm-id /subscriptions/{sub}/resourceGroups/rg-prod/providers/Microsoft.Compute/virtualMachines/vm-prod \
  --ports '[{"number":22,"protocol":"TCP","allowedSourceAddressPrefix":"*","maxRequestAccessDuration":"PT3H"}]'

# Request access
az security jit-policy initiate \
  --resource-group rg-prod \
  --vm-id /subscriptions/{sub}/resourceGroups/rg-prod/providers/Microsoft.Compute/virtualMachines/vm-prod \
  --initiator-ip "203.0.113.10" \
  --ports '[{"number":22,"endTimeUtc":"2025-04-19T18:00:00Z","allowedSourceAddressPrefix":"203.0.113.10"}]'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you perform security and penetration testing on Azure resources?

Microsoft **permits penetration testing** of Azure-hosted resources without prior approval for most standard services (as of 2025), but requires customers to follow the [Azure Penetration Testing Rules of Engagement](https://www.microsoft.com/en-us/msrc/pentest-rules-of-engagement) and not test Microsoft\'s shared infrastructure, DDoS defenses, or other tenants.

**Azure-native security assessment tools:**

| Tool | Purpose |
|------|---------|
| **Microsoft Defender for Cloud** | Continuous CSPM — misconfigurations, vulnerabilities, secure score |
| **Azure Security Benchmark** | Microsoft\'s curated best-practice controls mapped to CIS/NIST |
| **Microsoft Defender Vulnerability Management** | OS and software CVE scanning for VMs |
| **Microsoft Sentinel** | SIEM/SOAR — log correlation, threat hunting |
| **Azure Policy** | Enforce and audit resource configurations at scale |

**Third-party tools commonly used with Azure:**
- **Nmap / Masscan** — network port scanning of exposed endpoints
- **OWASP ZAP / Burp Suite** — DAST for web applications hosted on App Service / AKS
- **Trivy / Grype** — container image vulnerability scanning (integrates with ACR)
- **Checkov / tfsec** — IaC static analysis for Bicep, Terraform, ARM templates

**Example: Run Trivy against an Azure Container Registry image:**

```bash
# Authenticate to ACR
az acr login --name myappacr

# Pull and scan the image locally
trivy image myappacr.azurecr.io/myapp-api:latest \
  --severity HIGH,CRITICAL \
  --exit-code 1

# Or use ACR\'s built-in vulnerability scanning (Microsoft Defender for Containers)
az acr update --name myappacr --resource-group rg-prod \
  --allow-trusted-services true

# View scan findings
az security assessment list \
  --resource-group rg-prod \
  --query "[?contains(displayName,'Container')].{Name:displayName, Status:status.code}" \
  --output table
```

**Azure Policy — enforce no public IP on VMs (prevent attack surface expansion):**

```bash
az policy assignment create \
  --name "deny-public-ip" \
  --policy "9daedab3-fb2d-461e-b861-71790eead4f6" \
  --scope "/subscriptions/{sub-id}/resourceGroups/rg-prod"
```

**Key penetration testing areas for Azure:**
1. **Identity & Access** — Entra ID misconfiguration, over-privileged service principals, exposed credentials in repos
2. **Network perimeter** — Open NSG rules, publicly accessible storage accounts, exposed management ports
3. **Application layer** — OWASP Top 10 against App Service/AKS workloads, APIM policy bypasses
4. **Container security** — Image vulnerabilities, privileged containers, secrets in environment variables
5. **Storage** — Publicly accessible Blob containers, lack of SAS token expiry controls
6. **IaC misconfigurations** — Insecure defaults in ARM/Bicep/Terraform before deployment

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. AZURE STORAGE

<br>

## Q. What storage services does Azure offer and when do you use each?

| Service | Type | Best For |
|---------|------|----------|
| **Blob Storage** | Unstructured object store | Images, videos, backups, logs, static websites |
| **Azure Files** | Managed SMB/NFS file share | Lift-and-shift legacy apps needing shared file system |
| **Azure Disks** | Block storage for VMs | OS disks, data disks attached to VMs |
| **Azure Queue Storage** | Message queue | Decoupling app components, simple task queues |
| **Azure Table Storage** | NoSQL key-value | Semi-structured data, IoT telemetry at low cost |
| **Azure Data Lake Storage Gen2** | Hierarchical namespace object store | Big data analytics, Spark/Hadoop workloads |

**Create a storage account with secure defaults:**

```bash
az storage account create \
  --name stmyapp001 \
  --resource-group rg-myapp \
  --location eastus \
  --sku Standard_ZRS \
  --kind StorageV2 \
  --min-tls-version TLS1_2 \
  --allow-blob-public-access false \
  --https-only true \
  --enable-hierarchical-namespace false
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the access tiers in Azure Blob Storage and how do you use lifecycle management?

**Access Tiers:**

| Tier | Storage Cost | Access Cost | Latency | Best For |
|------|-------------|-------------|---------|----------|
| **Hot** | Highest | Lowest | ms | Frequently accessed data |
| **Cool** | ~50% less | Higher | ms | Infrequently accessed, min 30 days |
| **Cold** | ~60% less | Higher | ms | Rarely accessed, min 90 days |
| **Archive** | Lowest | Highest | Hours (rehydrate) | Long-term retention, compliance, min 180 days |

**Lifecycle management policy (automatically tier blobs):**

```json
{
  "rules": [
    {
      "name": "tiering-rule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "actions": {
          "baseBlob": {
            "tierToCool":    { "daysAfterModificationGreaterThan": 30  },
            "tierToCold":    { "daysAfterModificationGreaterThan": 90  },
            "tierToArchive": { "daysAfterModificationGreaterThan": 180 },
            "delete":        { "daysAfterModificationGreaterThan": 365 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 30 }
          }
        },
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["logs/", "backups/"]
        }
      }
    }
  ]
}
```

```bash
az storage account management-policy create \
  --account-name stmyapp001 \
  --resource-group rg-myapp \
  --policy @lifecycle-policy.json
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the replication options in Azure Storage?

| Option | Acronym | Copies | Availability | Best For |
|--------|---------|--------|-------------|----------|
| Locally Redundant Storage | **LRS** | 3 in one datacenter | Lowest | Dev/test, lowest cost |
| Zone-Redundant Storage | **ZRS** | 3 across Availability Zones | High | Production in-region HA |
| Geo-Redundant Storage | **GRS** | 3 LRS + 3 in paired region | 99.99999999999999% | Disaster recovery |
| Geo-Zone-Redundant Storage | **GZRS** | 3 ZRS + 3 in paired region | Highest | Mission-critical |
| Read-Access GRS/GZRS | **RA-GRS / RA-GZRS** | Same as GRS/GZRS | Highest | Read from secondary during outage |

**Recommendation:**
- Use **ZRS** for most production workloads in regions with Availability Zones.
- Use **GZRS** for mission-critical data requiring regional failover.

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/storage/common/media/storage-redundancy/geo-redundant-storage.png" alt="Azure Storage Replication Options" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you upload and download blobs using the Azure SDK (.NET)?

```csharp
// Install: Azure.Storage.Blobs NuGet package

// Upload a blob using Managed Identity (no credentials)
var blobServiceClient = new BlobServiceClient(
    new Uri("https://stmyapp001.blob.core.windows.net"),
    new DefaultAzureCredential());

var containerClient = blobServiceClient.GetBlobContainerClient("uploads");
await containerClient.CreateIfNotExistsAsync();

// Upload from a stream
await using var stream = File.OpenRead("report.pdf");
var blobClient = containerClient.GetBlobClient("reports/2025/report.pdf");
await blobClient.UploadAsync(stream, new BlobUploadOptions
{
    HttpHeaders = new BlobHttpHeaders { ContentType = "application/pdf" },
    AccessTier = AccessTier.Cool,
    Metadata = new Dictionary<string, string> { ["uploaded-by"] = "batch-job" }
});

// Generate a time-limited SAS URL for download
var sasBuilder = new BlobSasBuilder
{
    BlobContainerName = "uploads",
    BlobName = "reports/2025/report.pdf",
    Resource = "b",
    ExpiresOn = DateTimeOffset.UtcNow.AddHours(1)
};
sasBuilder.SetPermissions(BlobSasPermissions.Read);

// Requires a user-delegation key (preferred) or storage account key
var sasUrl = blobClient.GenerateSasUri(sasBuilder);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the AzCopy tool and how is it used for data migration?

**AzCopy** is a command-line utility for high-performance data transfers to/from Azure Blob Storage, Azure Files, and Amazon S3. It supports parallel transfers, resume-on-failure, and SAS or Managed Identity authentication.

```bash
# Login with Managed Identity (no interactive prompt)
azcopy login --identity

# Copy a local folder to Blob Storage
azcopy copy 'C:\data\' 'https://stmyapp001.blob.core.windows.net/backups/' \
  --recursive \
  --overwrite=ifSourceNewer

# Sync (only changed/new files — like rsync)
azcopy sync 'C:\data\' 'https://stmyapp001.blob.core.windows.net/backups/' \
  --recursive \
  --delete-destination=true

# Copy between two storage accounts (server-side, no local bandwidth)
azcopy copy \
  'https://source.blob.core.windows.net/container/?<sas>' \
  'https://dest.blob.core.windows.net/container/?<sas>' \
  --recursive

# Copy from S3 to Azure Blob
azcopy copy \
  'https://s3.amazonaws.com/mybucket/myfile.csv' \
  'https://stmyapp001.blob.core.windows.net/imports/myfile.csv'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. NETWORKING

<br>

## Q. What is an Azure Virtual Network (VNet) and how does it support resource isolation?

An **Azure Virtual Network (VNet)** is the fundamental networking building block in Azure. It provides an isolated, private network namespace in the cloud where you define IP address space, subnets, route tables, and security policies.

**Key concepts:**

| Concept | Description |
|---------|-------------|
| **Address space** | CIDR block assigned to the VNet (e.g., `10.0.0.0/16`) |
| **Subnets** | Subdivisions of the VNet address space |
| **NSG** | Firewall rules applied to subnets or NICs |
| **Route table (UDR)** | Custom routes to override Azure\'s default routing |
| **Service Endpoints** | Direct route from VNet to Azure PaaS services |
| **Private Endpoints** | Azure PaaS services accessible only via private IP |

**Create a VNet with subnets:**

```bash
# Create VNet
az network vnet create \
  --name vnet-prod \
  --resource-group rg-myapp \
  --address-prefix 10.0.0.0/16

# Add web tier subnet
az network vnet subnet create \
  --vnet-name vnet-prod \
  --resource-group rg-myapp \
  --name snet-web \
  --address-prefix 10.0.1.0/24

# Add database tier subnet
az network vnet subnet create \
  --vnet-name vnet-prod \
  --resource-group rg-myapp \
  --name snet-db \
  --address-prefix 10.0.2.0/24
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Network Security Groups (NSGs) and how do you configure them?

An **NSG** is a stateful L4 packet filter containing inbound and outbound security rules. Each rule specifies source/destination IP, port, protocol, and allow/deny action. NSGs can be applied to subnets (affecting all NICs in the subnet) or individual NICs.

**Create an NSG allowing HTTPS inbound, blocking everything else:**

```bash
az network nsg create \
  --name nsg-web \
  --resource-group rg-myapp

# Allow HTTPS from internet
az network nsg rule create \
  --nsg-name nsg-web \
  --resource-group rg-myapp \
  --name Allow-HTTPS-Inbound \
  --priority 100 \
  --direction Inbound \
  --source-address-prefixes Internet \
  --destination-port-ranges 443 \
  --protocol Tcp \
  --access Allow

# Allow HTTP (redirect to HTTPS)
az network nsg rule create \
  --nsg-name nsg-web \
  --resource-group rg-myapp \
  --name Allow-HTTP-Inbound \
  --priority 110 \
  --direction Inbound \
  --source-address-prefixes Internet \
  --destination-port-ranges 80 \
  --protocol Tcp \
  --access Allow

# Deny all other inbound
az network nsg rule create \
  --nsg-name nsg-web \
  --resource-group rg-myapp \
  --name Deny-All-Inbound \
  --priority 4000 \
  --direction Inbound \
  --source-address-prefixes '*' \
  --destination-port-ranges '*' \
  --protocol '*' \
  --access Deny

# Attach to subnet
az network vnet subnet update \
  --vnet-name vnet-prod \
  --resource-group rg-myapp \
  --name snet-web \
  --network-security-group nsg-web
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Load Balancer and Azure Application Gateway?

| Aspect | Azure Load Balancer | Azure Application Gateway |
|--------|--------------------|-----------------------------|
| OSI Layer | Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| Routing | IP + Port hash | URL path, host header, cookies |
| SSL Termination | No | Yes |
| WAF | No | Yes (WAF SKU) |
| Health probes | TCP/HTTP | HTTP/HTTPS |
| Session affinity | IP-based | Cookie-based |
| Autoscaling | No (Standard) | Yes (v2) |
| Cost | Lower | Higher |
| Use case | Non-HTTP, VM scale sets | Web apps, APIs, microservices |

**Configure Application Gateway with WAF (Bicep):**

```bicep
resource appGateway 'Microsoft.Network/applicationGateways@2023-09-01' = {
  name: 'agw-myapp'
  location: location
  properties: {
    sku: {
      name: 'WAF_v2'
      tier: 'WAF_v2'
    }
    autoscaleConfiguration: {
      minCapacity: 2
      maxCapacity: 10
    }
    webApplicationFirewallConfiguration: {
      enabled: true
      firewallMode: 'Prevention'
      ruleSetType: 'OWASP'
      ruleSetVersion: '3.2'
    }
    // ... frontend, backend, listener, routing rule configs
  }
}
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/application-gateway/media/application-gateway-url-route-overview/figure1-720.png" alt="Azure Application Gateway Architecture" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure ExpressRoute and how does it differ from VPN Gateway?

| Aspect | Azure VPN Gateway | Azure ExpressRoute |
|--------|-------------------|-------------------|
| Connection | Over public internet (encrypted) | Private, dedicated fibre through a provider |
| Bandwidth | Up to 10 Gbps | Up to 100 Gbps |
| Latency | Variable (internet routing) | Predictable, low latency |
| SLA | 99.9% | 99.95% |
| Cost | Lower (Gateway + traffic) | Higher (circuit + provider cost) |
| Encryption | IPsec/TLS built-in | Optional MACsec (not encrypted by default) |
| Best for | Branch offices, dev/test | Finance, regulated industries, large data transfer |

**Both support coexistence** — ExpressRoute for primary high-bandwidth connection, VPN Gateway as failover.

```bash
# Create VPN Gateway (site-to-site)
az network vnet-gateway create \
  --name vpn-gw-prod \
  --resource-group rg-myapp \
  --vnet vnet-prod \
  --gateway-type Vpn \
  --vpn-type RouteBased \
  --sku VpnGw2 \
  --no-wait
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/expressroute/media/expressroute-introduction/expressroute-connection-overview.png" alt="Azure ExpressRoute Connection Overview" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Bastion and how does it enhance VM security?

**Azure Bastion** is a fully managed PaaS service providing secure RDP and SSH connectivity to VMs directly through the Azure Portal — over TLS 443 — without exposing VMs to a public IP address or open RDP/SSH ports.

```bash
# Create Azure Bastion (requires AzureBastionSubnet /26 minimum)
az network vnet subnet create \
  --vnet-name vnet-prod \
  --resource-group rg-myapp \
  --name AzureBastionSubnet \
  --address-prefix 10.0.255.0/26

az network public-ip create \
  --name pip-bastion \
  --resource-group rg-myapp \
  --sku Standard \
  --zone 1 2 3

az network bastion create \
  --name bastion-prod \
  --resource-group rg-myapp \
  --vnet-name vnet-prod \
  --public-ip-address pip-bastion \
  --sku Standard \
  --enable-tunneling true  # allows native SSH/RDP clients
```

**Benefits:**
- No public IP on VMs — attack surface eliminated.
- No management of NSG rules for RDP/SSH ports.
- Session recorded and auditable in Azure Monitor.
- Standard SKU supports native client (Windows Terminal, PuTTY).

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/bastion/media/bastion-overview/architecture.png" alt="Azure Bastion Architecture" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is VNet peering and when do you use it?

**VNet peering** connects two Azure VNets so resources in either VNet can communicate with low-latency private IP routing (no gateway, no encryption overhead, no bandwidth throttling).

- **Regional peering** — both VNets in the same region.
- **Global peering** — VNets in different Azure regions.

```bash
# Peer vnet-hub → vnet-spoke
az network vnet peering create \
  --name hub-to-spoke \
  --resource-group rg-hub \
  --vnet-name vnet-hub \
  --remote-vnet /subscriptions/{sub}/resourceGroups/rg-spoke/providers/Microsoft.Network/virtualNetworks/vnet-spoke \
  --allow-vnet-access \
  --allow-forwarded-traffic \
  --allow-gateway-transit  # hub shares its VPN gateway

# Peer vnet-spoke → vnet-hub (must create both directions)
az network vnet peering create \
  --name spoke-to-hub \
  --resource-group rg-spoke \
  --vnet-name vnet-spoke \
  --remote-vnet /subscriptions/{sub}/resourceGroups/rg-hub/providers/Microsoft.Network/virtualNetworks/vnet-hub \
  --allow-vnet-access \
  --use-remote-gateways  # spoke uses hub\'s VPN gateway
```

**Hub-and-spoke topology** — a central hub VNet (with shared services: firewall, VPN gateway, Bastion) peers to multiple spoke VNets (one per application/environment).

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/virtual-network/media/virtual-networks-peering-overview/local-or-remote-gateway-in-peered-virual-network.png" alt="Azure VNet Peering" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. MONITORING

<br>

## Q. What is Azure Monitor and what are its key components?

**Azure Monitor** is the unified observability platform for all Azure resources. It collects metrics, logs, and traces, and feeds them into analysis, alerting, dashboards, and automated actions.

**Architecture:**

```
Data Sources (VMs, App Service, AKS, custom)
          ↓
Azure Monitor
  ├── Metrics (time-series, 93-day retention, near real-time)
  ├── Logs (Log Analytics workspace — KQL queryable, configurable retention)
  ├── Traces (Application Insights — distributed tracing, dependency maps)
  └── Activity Log (resource-level audit — who did what, when)
          ↓
Analysis & Action
  ├── Alerts (metric / log / activity log)
  ├── Dashboards & Workbooks
  ├── Action Groups (email, SMS, webhook, ITSM, Logic App, runbook)
  └── Autoscale
```

**Enable diagnostics on an App Service:**

```bash
az monitor diagnostic-settings create \
  --name appservice-diag \
  --resource $(az webapp show --name myapp --resource-group rg-myapp --query id -o tsv) \
  --workspace $(az monitor log-analytics workspace show --workspace-name law-myapp --resource-group rg-myapp --query id -o tsv) \
  --logs '[{"category":"AppServiceHTTPLogs","enabled":true},{"category":"AppServiceConsoleLogs","enabled":true}]' \
  --metrics '[{"category":"AllMetrics","enabled":true}]'
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/azure-monitor/media/overview/overview.png" alt="Azure Monitor Overview" width="800px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you write Kusto Query Language (KQL) queries in Log Analytics?

**KQL** is the query language for Azure Monitor Logs, Application Insights, and Microsoft Sentinel. It uses a pipe-based syntax where each operator transforms the table.

```kusto
// Top 10 slowest HTTP requests in the last hour
AppRequests
| where TimeGenerated >= ago(1h)
| where Success == false or DurationMs > 2000
| project TimeGenerated, Name, DurationMs, ResultCode, Success
| top 10 by DurationMs desc

// Error rate by operation per 5-minute bucket
AppRequests
| where TimeGenerated >= ago(6h)
| summarize
    TotalRequests = count(),
    FailedRequests = countif(Success == false)
    by bin(TimeGenerated, 5m), Name
| extend ErrorRate = round(100.0 * FailedRequests / TotalRequests, 2)
| where ErrorRate > 5
| order by TimeGenerated desc

// VM CPU alert — average CPU > 80% for 15 minutes
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where InstanceName == "_Total"
| summarize AvgCPU = avg(CounterValue) by bin(TimeGenerated, 5m), Computer
| where AvgCPU > 80

// Exceptions with stack trace
AppExceptions
| where TimeGenerated >= ago(24h)
| project TimeGenerated, ExceptionType, Method, OuterMessage, InnermostMessage, AppRoleInstance
| order by TimeGenerated desc
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Application Insights and how do you instrument a .NET application?

**Application Insights** is the application performance monitoring (APM) component of Azure Monitor. It automatically collects: HTTP requests, dependencies (SQL, HTTP, Azure Storage), exceptions, custom events, and performance counters.

```csharp
// Program.cs (.NET 8 / ASP.NET Core)
builder.Services.AddApplicationInsightsTelemetry(options =>
{
    options.ConnectionString = builder.Configuration["APPLICATIONINSIGHTS_CONNECTION_STRING"];
    options.EnableAdaptiveSampling = true;
    options.EnableQuickPulseMetricStream = true; // Live Metrics
});

// Track custom events and metrics
public class OrderService(TelemetryClient telemetry)
{
    public async Task PlaceOrderAsync(Order order)
    {
        var sw = Stopwatch.StartNew();
        try
        {
            await _repo.SaveAsync(order);
            sw.Stop();

            // Track custom event with properties
            telemetry.TrackEvent("OrderPlaced", new Dictionary<string, string>
            {
                ["OrderId"]    = order.Id.ToString(),
                ["CustomerId"] = order.CustomerId,
                ["Channel"]    = order.Channel
            },
            new Dictionary<string, double>
            {
                ["OrderTotal"] = (double)order.Total,
                ["ItemCount"]  = order.Items.Count
            });

            // Track custom metric
            telemetry.TrackMetric("OrderProcessingMs", sw.ElapsedMilliseconds);
        }
        catch (Exception ex)
        {
            telemetry.TrackException(ex, new Dictionary<string, string>
            {
                ["OrderId"] = order.Id.ToString()
            });
            throw;
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you set up alerts and action groups in Azure Monitor?

**Alerts** fire when a metric crosses a threshold or a log query returns results. **Action Groups** define what happens when an alert fires.

```bash
# Create an action group (email + webhook)
az monitor action-group create \
  --name ag-oncall \
  --resource-group rg-myapp \
  --short-name oncall \
  --action email admin-email admin@contoso.com \
  --action webhook pagerduty "https://events.pagerduty.com/integration/{key}/enqueue"

# Create a metric alert — CPU > 85% for 5 minutes on a VM
az monitor metrics alert create \
  --name "High CPU Alert" \
  --resource-group rg-myapp \
  --scopes $(az vm show --name vm-webserver --resource-group rg-myapp --query id -o tsv) \
  --condition "avg Percentage CPU > 85" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 2 \
  --action ag-oncall

# Create a log alert — 5xx errors in App Service logs
az monitor scheduled-query create \
  --name "App Service 5xx Errors" \
  --resource-group rg-myapp \
  --scopes $(az webapp show --name myapp --resource-group rg-myapp --query id -o tsv) \
  --condition "count > 10" \
  --condition-query "AppRequests | where ResultCode >= 500 | summarize count()" \
  --window-duration 5 \
  --evaluation-frequency 5 \
  --severity 1 \
  --action-groups $(az monitor action-group show --name ag-oncall --resource-group rg-myapp --query id -o tsv)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Service Health and how does it differ from Azure Monitor?

| | Azure Service Health | Azure Monitor |
|--|---------------------|---------------|
| Focus | Azure platform health (outages, maintenance, advisories) | Your application and resource health |
| Data Source | Microsoft\'s incident feed | Your metrics, logs, and traces |
| Use case | Know if an Azure region/service is down | Know if your app is slow or throwing errors |
| Alerts | Service issues, planned maintenance, health advisories | Threshold breaches on your resources |

**Create a Service Health alert for regional outages:**

```bash
az monitor activity-log alert create \
  --name "Azure Outage Alert" \
  --resource-group rg-myapp \
  --condition category=ServiceHealth regions=eastus services="Virtual Machines" \
  --action-group $(az monitor action-group show --name ag-oncall --resource-group rg-myapp --query id -o tsv)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Application Insights for distributed tracing in a microservices architecture?

**Distributed tracing** correlates telemetry across multiple services using a shared `operation_Id`. Application Insights propagates the `traceparent` header (W3C Trace Context) automatically when using the SDK, so a single end-to-end transaction can be visualized in the **Transaction Diagnostics** blade even across Azure Functions, App Service, and external HTTP calls.

```csharp
// ASP.NET Core — correlation headers are forwarded automatically by HttpClient
// registered via AddHttpClient()
builder.Services.AddApplicationInsightsTelemetry();
builder.Services.AddHttpClient<OrderServiceClient>(); // correlation headers auto-propagated

// Manually enrich a span with extra properties (useful in background workers)
public class InventoryWorker(TelemetryClient telemetry)
{
    public async Task ProcessAsync(string orderId)
    {
        using var op = telemetry.StartOperation<RequestTelemetry>("ProcessInventory");
        op.Telemetry.Properties["OrderId"] = orderId;

        try
        {
            await ReserveStockAsync(orderId);
            op.Telemetry.Success = true;
        }
        catch (Exception ex)
        {
            telemetry.TrackException(ex);
            op.Telemetry.Success = false;
            throw;
        }
    }
}

// Azure Functions — enable distributed tracing with isolated worker
// In Program.cs:
services.AddApplicationInsightsTelemetryWorkerService();
services.ConfigureFunctionsApplicationInsights();
```

```bash
# Enable distributed tracing in Application Insights (connection string approach)
az monitor app-insights component create \
  --app ai-myapp \
  --resource-group rg-prod \
  --location eastus \
  --application-type web

# Retrieve connection string (use this — not the deprecated instrumentation key)
az monitor app-insights component show \
  --app ai-myapp \
  --resource-group rg-prod \
  --query connectionString -o tsv
```

**KQL — End-to-end trace for a single operation:**
```kusto
// Find all telemetry items for one operation (distributed trace)
union requests, dependencies, exceptions, traces, customEvents
| where operation_Id == "abc123def456"
| project timestamp, itemType, name, duration, success, message, operation_ParentId
| order by timestamp asc
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/azure-monitor/app/media/app-insights-overview/app-insights-overview-screenshot.png" alt="Application Insights Distributed Tracing - Application Map" width="800px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure availability tests in Application Insights?

**Availability tests** (URL ping or multi-step web tests) run from Azure-hosted test agents worldwide, alerting when your endpoint returns an error or exceeds a response-time threshold.

| Test type | Description |
|-----------|-------------|
| **Standard (URL ping)** | Single GET/POST request; checks status code, response body, SSL expiry |
| **Custom TrackAvailability** | Code-based test — supports authentication, multi-step logic, and custom assertions |

```bash
# Create a standard URL ping test via Azure CLI
az monitor app-insights web-test create \
  --resource-group rg-prod \
  --app-insights-name ai-myapp \
  --name "Homepage Availability" \
  --location eastus \
  --defined-web-test-type Standard \
  --http-verb GET \
  --request-url "https://myapp.azurewebsites.net/health" \
  --enabled true \
  --frequency 300 \
  --timeout 30 \
  --locations "['us-va-ash-azr','emea-nl-ams-azr','apac-sg-sin-azr']"
```

```csharp
// Custom availability test using TrackAvailability (e.g., run as Azure Function on a timer)
public class AvailabilityTestFunction(TelemetryClient telemetry, HttpClient http)
{
    [Function("AvailabilityTest")]
    public async Task Run([TimerTrigger("0 */5 * * * *")] TimerInfo timer)
    {
        var testName = "LoginFlowTest";
        var runLocation = "East US";
        var startTime = DateTimeOffset.UtcNow;
        var stopwatch = Stopwatch.StartNew();
        bool success = false;
        string message = string.Empty;

        try
        {
            // Step 1: Get login page
            var loginPage = await http.GetAsync("https://myapp.com/login");
            loginPage.EnsureSuccessStatusCode();

            // Step 2: Submit credentials
            var loginResult = await http.PostAsJsonAsync("https://myapp.com/api/auth/login",
                new { username = "testuser@contoso.com", password = Environment.GetEnvironmentVariable("TEST_PASSWORD") });

            loginResult.EnsureSuccessStatusCode();
            var token = (await loginResult.Content.ReadFromJsonAsync<LoginResponse>())?.Token;

            success = !string.IsNullOrEmpty(token);
            message = success ? "Login flow completed successfully" : "No token returned";
        }
        catch (Exception ex)
        {
            message = ex.Message;
        }
        finally
        {
            stopwatch.Stop();
            telemetry.TrackAvailability(testName, startTime, stopwatch.Elapsed, runLocation, success, message);
        }
    }
}
```

**Alert on availability drop:**
```bash
az monitor metrics alert create \
  --name "Availability < 100%" \
  --resource-group rg-prod \
  --scopes $(az monitor app-insights component show --app ai-myapp --resource-group rg-prod --query id -o tsv) \
  --condition "avg availabilityResults/availabilityPercentage < 100" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --severity 1 \
  --action ag-oncall
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure sampling in Application Insights to control data volume and cost?

**Sampling** reduces the volume of telemetry sent to Application Insights without losing statistical accuracy. Application Insights supports three sampling strategies:

| Strategy | How it works | Best for |
|----------|-------------|---------|
| **Adaptive** (default) | SDK auto-adjusts rate to stay within a target volume | Most apps — self-tuning |
| **Fixed-rate** | SDK + portal agree on a fixed percentage; correlated items sampled together | Consistent rate requirement |
| **Ingestion** | Applied at the portal after data is received — does NOT reduce SDK bandwidth | Legacy / no SDK access |

```csharp
// ASP.NET Core — configure adaptive sampling with limits
builder.Services.AddApplicationInsightsTelemetry(options =>
{
    options.ConnectionString = config["APPLICATIONINSIGHTS_CONNECTION_STRING"];
    options.EnableAdaptiveSampling = true;
});

// Override adaptive sampling settings
builder.Services.Configure<TelemetryConfiguration>(config =>
{
    var builder = config.DefaultTelemetrySink.TelemetryProcessorChainBuilder;
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond: 5, excludedTypes: "Exception");
    // Exceptions are always captured (not sampled out)
    builder.Build();
});

// Fixed-rate sampling (5% of requests sampled)
builder.Services.Configure<TelemetryConfiguration>(config =>
{
    config.DefaultTelemetrySink.TelemetryProcessorChainBuilder
        .UseSampling(5.0)  // 5 percent
        .Build();
});
```

```json
// appsettings.json — configure via ApplicationInsights section
{
  "ApplicationInsights": {
    "ConnectionString": "<connection-string>",
    "EnableAdaptiveSampling": true,
    "AdaptiveSamplingSettings": {
      "MaxTelemetryItemsPerSecond": 5,
      "ExcludedTypes": "Exception;Event"
    }
  }
}
```

> **Tip:** Always exclude `Exception` type from sampling so 100% of exceptions are captured regardless of overall sample rate.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Application Insights Application Map and Live Metrics?

**Application Map** is an auto-generated topology diagram showing all components (services, dependencies, databases, external calls) with failure rates and response times on each edge — making bottlenecks and failure points immediately visible.

**Live Metrics** (QuickPulse) streams real-time telemetry (requests/sec, failure rate, CPU, memory) with sub-second latency — useful during deployments and incident response.

```csharp
// Enable Live Metrics in Program.cs
builder.Services.AddApplicationInsightsTelemetry(options =>
{
    options.EnableQuickPulseMetricStream = true;  // Live Metrics
});

// Custom health metric reported to Live Metrics
public class LiveMetricsReporter(TelemetryClient telemetry) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        while (!ct.IsCancellationRequested)
        {
            var queueDepth = await GetQueueDepthAsync();
            telemetry.GetMetric("OrderQueueDepth").TrackValue(queueDepth);
            await Task.Delay(TimeSpan.FromSeconds(10), ct);
        }
    }
}
```

```bash
# Enable Live Metrics via the portal or by ensuring the SDK is configured
# The QuickPulse endpoint is: https://rt.services.visualstudio.com/QuickPulseService.svc

# Query Application Map data via KQL (dependencies table)
# Run in Log Analytics workspace linked to the Application Insights resource:
```

```kusto
// Application Map: failure rates per dependency
dependencies
| where timestamp >= ago(1h)
| summarize
    Calls = count(),
    Failures = countif(success == false),
    AvgDurationMs = avg(duration)
  by target, type
| extend FailureRate = round(100.0 * Failures / Calls, 2)
| order by Failures desc

// Identify slowest downstream dependencies
dependencies
| where timestamp >= ago(1h)
| summarize P95Duration = percentile(duration, 95) by target, name
| order by P95Duration desc
| take 20
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the Snapshot Debugger and Profiler in Application Insights?

**Application Insights Profiler** captures CPU flame graphs from production traffic to identify hot code paths — without attaching a debugger or modifying the app.

**Snapshot Debugger** automatically captures a memory snapshot (with local variable values and call stack) when an unhandled exception occurs in production — eliminating the need to reproduce the bug locally.

| Tool | Purpose | Overhead |
|------|---------|---------|
| **Profiler** | CPU profiling — find slow methods | < 5% CPU for 2-minute sessions |
| **Snapshot Debugger** | Capture variable state at exception site | Minimal — triggered only on exceptions |

```bash
# Enable Profiler on an App Service (via site extension)
az webapp config appsettings set \
  --name app-mywebapp \
  --resource-group rg-prod \
  --settings \
    APPINSIGHTS_INSTRUMENTATIONKEY=$(az monitor app-insights component show --app ai-myapp --resource-group rg-prod --query instrumentationKey -o tsv) \
    APPLICATIONINSIGHTS_CONNECTION_STRING=$(az monitor app-insights component show --app ai-myapp --resource-group rg-prod --query connectionString -o tsv) \
    ApplicationInsightsAgent_EXTENSION_VERSION=~3 \
    DiagnosticServices_EXTENSION_VERSION=~3   # enables Profiler + Snapshot Debugger
```

```csharp
// .NET: Enable Snapshot Debugger via NuGet package
// Install: Microsoft.ApplicationInsights.SnapshotCollector

// appsettings.json
{
  "SnapshotCollectorConfiguration": {
    "IsEnabled": true,
    "ThresholdForSnapshotting": 1,
    "MaximumSnapshotsRequired": 3,
    "MaximumCollectionPlanSize": 50,
    "ReconnectInterval": "00:15:00",
    "ProblemCounterResetInterval": "1.00:00:00",
    "SnapshotsPerTenMinutesLimit": 1,
    "SnapshotsPerDayLimit": 30,
    "SnapshotInLowPriorityThread": true,
    "ProvideAnonymousTelemetry": true,
    "FailedRequestLimit": 3
  }
}
```

```csharp
// Program.cs — wire up the snapshot collector
builder.Services.AddSnapshotCollector(config =>
    builder.Configuration.Bind(nameof(SnapshotCollectorConfiguration), config));
```

> **Security note:** Snapshots may contain sensitive data (PII, secrets in memory). Enable Snapshot Debugger only in environments where you control access to the Application Insights resource. Use RBAC to restrict snapshot viewing.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Application Insights workbooks and dashboards for operational monitoring?

**Workbooks** are interactive reports in Azure Monitor that combine KQL queries, text, parameters, and charts into shareable, parameterized analytical documents. **Dashboards** pin individual tiles (charts, metrics, KQL results) for at-a-glance monitoring.

```kusto
-- Workbook query 1: Request volume and failure rate over time
requests
| where timestamp >= ago({TimeRange:value})
| summarize
    RequestCount = count(),
    FailedCount = countif(success == false)
  by bin(timestamp, {Granularity:value})
| extend FailureRate = round(100.0 * FailedCount / RequestCount, 2)

-- Workbook query 2: Top 10 slowest pages/APIs (P95 latency)
requests
| where timestamp >= ago({TimeRange:value})
| summarize
    P50 = percentile(duration, 50),
    P95 = percentile(duration, 95),
    P99 = percentile(duration, 99),
    Count = count()
  by name
| order by P95 desc
| take 10

-- Workbook query 3: Exception frequency by type
exceptions
| where timestamp >= ago({TimeRange:value})
| summarize ExceptionCount = count() by type, outerMessage
| order by ExceptionCount desc
| take 20

-- Workbook query 4: Dependency failure heatmap
dependencies
| where timestamp >= ago({TimeRange:value})
| where success == false
| summarize FailureCount = count() by target, bin(timestamp, 1h)
| render heatmap
```

```bash
# Create an Application Insights dashboard via Azure CLI (ARM template)
az portal dashboard create \
  --resource-group rg-prod \
  --name "AppInsights-Ops-Dashboard" \
  --input-path dashboard-template.json \
  --location eastus

# Export existing dashboard to JSON (for version control)
az portal dashboard show \
  --resource-group rg-prod \
  --name "AppInsights-Ops-Dashboard" > dashboard-export.json
```

**Pinning to Azure Dashboard:**
- From any Application Insights chart → **Pin to dashboard** → select shared dashboard
- Key tiles to pin: Live Metrics, Failed Requests, Server Response Time, Availability, Exceptions

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you integrate Application Insights with Azure Log Analytics workspace?

**Workspace-based Application Insights** (the current model) stores all telemetry in an Azure Log Analytics workspace — enabling cross-resource queries, unified RBAC, longer retention, and data export.

```bash
# Create Log Analytics workspace
az monitor log-analytics workspace create \
  --name law-myapp \
  --resource-group rg-prod \
  --location eastus \
  --retention-time 90  # days (default 30, max 730)

# Create workspace-based Application Insights resource
az monitor app-insights component create \
  --app ai-myapp \
  --resource-group rg-prod \
  --location eastus \
  --application-type web \
  --workspace $(az monitor log-analytics workspace show --workspace-name law-myapp --resource-group rg-prod --query id -o tsv)

# Migrate a classic Application Insights resource to workspace-based
az monitor app-insights component update \
  --app ai-myapp-classic \
  --resource-group rg-prod \
  --workspace $(az monitor log-analytics workspace show --workspace-name law-myapp --resource-group rg-prod --query id -o tsv)
```

```kusto
// Cross-resource query: correlate App Service HTTP logs with Application Insights requests
// Run in Log Analytics workspace
AppRequests
| where TimeGenerated >= ago(1h)
| join kind=leftouter (
    AzureDiagnostics
    | where ResourceType == "SITES"
    | where Category == "AppServiceHTTPLogs"
    | project TimeGenerated, csUriStem, scStatus, timeTaken, csUserAgent
) on $left.url == $right.csUriStem
| project TimeGenerated, name, duration, resultCode, scStatus, timeTaken

// Set data retention per table (granular control)
// In portal: Log Analytics workspace → Tables → AppRequests → Manage table
// Or via CLI:
az monitor log-analytics workspace table update \
  --resource-group rg-prod \
  --workspace-name law-myapp \
  --name AppRequests \
  --retention-time 180

// Configure continuous export to Storage Account for long-term archival
az monitor diagnostic-settings create \
  --name "ai-export" \
  --resource $(az monitor app-insights component show --app ai-myapp --resource-group rg-prod --query id -o tsv) \
  --storage-account $(az storage account show --name starchive --resource-group rg-prod --query id -o tsv) \
  --logs '[{"category":"AppRequests","enabled":true},{"category":"AppExceptions","enabled":true}]'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you add Application Insights to an Angular SPA to capture UI events?

**Application Insights JavaScript SDK** (`@microsoft/applicationinsights-web`) instruments a browser-side Angular application to automatically collect page views, route changes, AJAX/fetch dependency calls, exceptions, and custom events — all correlated to the same Application Insights resource used by your backend.

**Step 1 — Install the SDK**

```bash
npm install @microsoft/applicationinsights-web
```

**Step 2 — Create an Application Insights service**

```typescript
// src/app/core/services/app-insights.service.ts
import { Injectable } from '@angular/core';
import { ApplicationInsights, IEventTelemetry, IPageViewTelemetry } from '@microsoft/applicationinsights-web';
import { environment } from '../../../environments/environment';

@Injectable({ providedIn: 'root' })
export class AppInsightsService {
  private appInsights: ApplicationInsights;

  constructor() {
    this.appInsights = new ApplicationInsights({
      config: {
        connectionString: environment.appInsightsConnectionString,
        enableAutoRouteTracking: true,   // tracks Angular route changes as page views
        enableCorsCorrelation: true,     // propagates trace headers to backend API calls
        enableRequestHeaderTracking: true,
        enableResponseHeaderTracking: true,
        disableFetchTracking: false,     // track fetch() calls as dependencies
      },
    });
    this.appInsights.loadAppInsights();
    this.appInsights.trackPageView();   // track initial page load
  }

  // Track a custom UI event (button click, form submit, etc.)
  trackEvent(name: string, properties?: { [key: string]: string }): void {
    const event: IEventTelemetry = { name, properties };
    this.appInsights.trackEvent(event);
  }

  // Track a custom page view (useful for modal/dialog "pages")
  trackPageView(name: string, uri?: string): void {
    const pageView: IPageViewTelemetry = { name, uri };
    this.appInsights.trackPageView(pageView);
  }

  // Track a caught exception
  trackException(error: Error, severityLevel?: number): void {
    this.appInsights.trackException({ exception: error, severityLevel });
  }

  // Attach an authenticated user identity for user-level analytics
  setAuthenticatedUser(userId: string, accountId?: string): void {
    this.appInsights.setAuthenticatedUserContext(userId, accountId, true);
  }

  clearUser(): void {
    this.appInsights.clearAuthenticatedUserContext();
  }
}
```

**Step 3 — Capture Angular Router navigation as page views**

```typescript
// src/app/core/services/app-insights-router.service.ts
import { Injectable } from '@angular/core';
import { Router, NavigationEnd } from '@angular/router';
import { filter } from 'rxjs/operators';
import { AppInsightsService } from './app-insights.service';

@Injectable({ providedIn: 'root' })
export class AppInsightsRouterService {
  constructor(private router: Router, private ai: AppInsightsService) {}

  init(): void {
    this.router.events
      .pipe(filter(event => event instanceof NavigationEnd))
      .subscribe((event: NavigationEnd) => {
        this.ai.trackPageView(event.urlAfterRedirects, event.urlAfterRedirects);
      });
  }
}
```

```typescript
// src/app/app.component.ts
import { Component, OnInit } from '@angular/core';
import { AppInsightsRouterService } from './core/services/app-insights-router.service';

@Component({ selector: 'app-root', templateUrl: './app.component.html' })
export class AppComponent implements OnInit {
  constructor(private aiRouter: AppInsightsRouterService) {}

  ngOnInit(): void {
    this.aiRouter.init();  // start tracking route changes
  }
}
```

**Step 4 — Track custom UI events (button clicks, form submissions)**

```typescript
// src/app/features/checkout/checkout.component.ts
import { Component } from '@angular/core';
import { AppInsightsService } from '../../core/services/app-insights.service';

@Component({ selector: 'app-checkout', templateUrl: './checkout.component.html' })
export class CheckoutComponent {
  constructor(private ai: AppInsightsService) {}

  onAddToCart(productId: string, category: string): void {
    // Custom UI event with properties for filtering in AI
    this.ai.trackEvent('AddToCart', {
      productId,
      category,
      page: 'checkout',
    });
  }

  onFormSubmit(): void {
    this.ai.trackEvent('CheckoutSubmitted', { step: 'payment' });
  }
}
```

**Step 5 — Global error handler to capture uncaught exceptions**

```typescript
// src/app/core/handlers/global-error.handler.ts
import { ErrorHandler, Injectable } from '@angular/core';
import { AppInsightsService } from '../services/app-insights.service';

@Injectable()
export class GlobalErrorHandler implements ErrorHandler {
  constructor(private ai: AppInsightsService) {}

  handleError(error: Error): void {
    this.ai.trackException(error);
    console.error(error);
  }
}
```

```typescript
// src/app/app.module.ts  — register the handler
import { ErrorHandler, NgModule } from '@angular/core';
import { GlobalErrorHandler } from './core/handlers/global-error.handler';

@NgModule({
  providers: [
    { provide: ErrorHandler, useClass: GlobalErrorHandler },
  ],
})
export class AppModule {}
```

**Step 6 — Environment configuration**

```typescript
// src/environments/environment.ts
export const environment = {
  production: false,
  appInsightsConnectionString:
    'InstrumentationKey=00000000-0000-0000-0000-000000000000;IngestionEndpoint=https://eastus-8.in.applicationinsights.azure.com/;LiveEndpoint=https://eastus.livediagnostics.monitor.azure.com/',
};
```

> Use the **Connection String** (not the legacy Instrumentation Key alone) — it includes regional ingestion endpoints and is required for Government/Sovereign clouds.

**Query captured UI telemetry in Log Analytics (KQL):**

```kusto
// Top custom events in the last 24 hours
customEvents
| where timestamp > ago(24h)
| summarize EventCount = count() by name
| order by EventCount desc

// Page views by route
pageViews
| where timestamp > ago(7d)
| summarize Views = count() by name
| order by Views desc

// Browser exceptions
exceptions
| where timestamp > ago(24h)
| where client_Type == "Browser"
| project timestamp, type, outerMessage, client_Browser, client_OS, url
| order by timestamp desc

// Average page load time per route
pageViews
| where timestamp > ago(7d)
| summarize AvgDuration = avg(duration), P95 = percentile(duration, 95) by name
| order by P95 desc
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. DATABASES

<br>

## Q. What are the main database services in Azure and when do you choose each?

| Service | Engine | Best For |
|---------|--------|----------|
| **Azure SQL Database** | SQL Server (PaaS) | Relational OLTP, existing SQL Server apps |
| **Azure SQL Managed Instance** | SQL Server (near 100% compat.) | Lift-and-shift SQL Server with linked servers, CLR, replication |
| **Azure Database for PostgreSQL Flexible** | PostgreSQL 16 | Open-source relational, complex queries |
| **Azure Database for MySQL Flexible** | MySQL 8.0 | LAMP stack, WordPress, high read throughput |
| **Azure Cosmos DB** | Multi-model (NoSQL) | Globally distributed, low-latency, schemaless |
| **Azure Synapse Analytics** | Dedicated SQL pool | Data warehousing, analytics at petabyte scale |
| **Azure Cache for Redis** | Redis | In-memory cache, pub/sub, session state |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure SQL Database and how does it differ from on-premises SQL Server?

**Azure SQL Database** is a fully managed PaaS relational database engine running the latest stable SQL Server version. Microsoft handles patching, backups, high availability, and scaling.

**Key differences from on-premises SQL Server:**

| Feature | Azure SQL Database | On-Premises SQL Server |
|---------|-------------------|----------------------|
| OS/Patching | Managed by Microsoft | Customer responsibility |
| HA/DR | Built-in (99.99% SLA) | Manual AG/FCI setup |
| Backup | Automatic PITR (1–35 days) | Manual or SQL Agent |
| Scaling | Elastic, vCore or serverless | Hardware replacement |
| Cross-DB queries | Limited (Elastic Query) | Full cross-DB support |
| SQL Agent | Yes (Managed Instance only for full Agent) | Yes |
| Pricing | Per second/vCore | License + hardware |

**Create and connect to Azure SQL Database:**

```bash
# Create SQL Server logical server
az sql server create \
  --name sql-myapp-prod \
  --resource-group rg-myapp \
  --location eastus \
  --admin-user sqladmin \
  --admin-password "$(openssl rand -base64 24)"

# Enable Microsoft Entra Admin
az sql server ad-admin create \
  --server sql-myapp-prod \
  --resource-group rg-myapp \
  --display-name "SQL Admins" \
  --object-id $(az ad group show --group "SQL Admins" --query id -o tsv)

# Create database — General Purpose, 4 vCores, zone-redundant
az sql db create \
  --server sql-myapp-prod \
  --resource-group rg-myapp \
  --name sqldb-myapp \
  --service-objective GP_Gen5_4 \
  --zone-redundant true \
  --backup-storage-redundancy Geo
```

**Connect using Managed Identity from .NET:**

```csharp
// appsettings.json
// "ConnectionStrings": { "DefaultConnection": "Server=sql-myapp-prod.database.windows.net;Database=sqldb-myapp;Authentication=Active Directory Default" }

// EF Core with Managed Identity
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(
        builder.Configuration.GetConnectionString("DefaultConnection"),
        sql => sql.EnableRetryOnFailure(3)));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Cosmos DB and what are its key features?

**Azure Cosmos DB** is a globally distributed, multi-model NoSQL database designed for low latency (< 10 ms p99) and infinite scale. It supports multiple APIs: NoSQL (native), MongoDB, Cassandra, Gremlin (graph), and Table.

**Key features:**

| Feature | Detail |
|---------|--------|
| **Global distribution** | Replicate to any Azure region with a single click |
| **Multiple consistency levels** | Strong → Bounded Staleness → Session → Consistent Prefix → Eventual |
| **Multi-write regions** | Write to any region simultaneously |
| **Automatic indexing** | All properties indexed by default |
| **Serverless mode** | Pay per operation, no provisioned throughput |
| **Partition key** | Horizontal scaling key — choose carefully |

**CRUD operations (.NET SDK v3):**

```csharp
// Create client
var cosmosClient = new CosmosClient(
    accountEndpoint: "https://cosmos-myapp.documents.azure.com:443/",
    new DefaultAzureCredential());

var container = cosmosClient
    .GetDatabase("mydb")
    .GetContainer("orders");

// Create/upsert a document
var order = new Order
{
    Id = Guid.NewGuid().ToString(),
    CustomerId = "C001",       // partition key
    Total = 299.99m,
    CreatedAt = DateTime.UtcNow
};
await container.UpsertItemAsync(order, new PartitionKey(order.CustomerId));

// Query with SDK LINQ
var query = container.GetItemLinqQueryable<Order>()
    .Where(o => o.CustomerId == "C001" && o.Total > 100m)
    .ToFeedIterator();

while (query.HasMoreResults)
{
    var page = await query.ReadNextAsync();
    foreach (var item in page) Console.WriteLine(item.Id);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the consistency models in Azure Cosmos DB?

Cosmos DB offers 5 well-defined consistency levels as a slider between strongest consistency (highest latency) and weakest consistency (highest availability/throughput):

| Level | Description | SLA | Use Case |
|-------|-------------|-----|----------|
| **Strong** | Reads always return latest committed write | Highest latency | Financial transactions |
| **Bounded Staleness** | Reads lag behind by at most K operations or T time | High | Leader boards, gaming |
| **Session** (default) | Consistent within a client session | Moderate | Shopping carts, user profiles |
| **Consistent Prefix** | Reads never see out-of-order writes | Low latency | Social media feeds |
| **Eventual** | No ordering guarantees | Lowest latency | Metrics, counts |

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/cosmos-db/media/consistency-levels/five-consistency-levels.png" alt="Azure Cosmos DB Consistency Levels" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are SQL Elastic Pools in Azure SQL Database?

**Elastic Pools** allow multiple databases to share a common pool of compute resources (eDTUs or vCores). Ideal when individual databases have unpredictable, variable workloads that don\'t all peak simultaneously — reducing total cost vs. provisioning peak capacity per database.

```bash
# Create an elastic pool
az sql elastic-pool create \
  --server sql-myapp-prod \
  --resource-group rg-myapp \
  --name ep-tenants \
  --edition GeneralPurpose \
  --capacity 8 \
  --db-min-capacity 0 \
  --db-max-capacity 4 \
  --zone-redundant true

# Add a database to the pool
az sql db create \
  --server sql-myapp-prod \
  --resource-group rg-myapp \
  --name sqldb-tenant-001 \
  --elastic-pool ep-tenants
```

**Best for:** SaaS multi-tenant applications where each tenant has a dedicated database.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you model data and choose a partition key in Azure Cosmos DB?

**Data modeling principles:**
- Cosmos DB is a schema-free JSON document store — embed related data when it is always read/written together; reference (normalize) when data is accessed independently or is large.
- **Partition key** determines how data is distributed across physical partitions. A poor key creates "hot" partitions and throttles throughput.

**Partition key selection rules:**

| Rule | Explanation |
|------|-------------|
| **High cardinality** | Many distinct values — avoids hot partition |
| **Even distribution** | Writes spread evenly across values |
| **Appears in most queries** | Enables single-partition (cheap) reads |
| **Immutable** | Cannot change after insert |

```csharp
// Good partition keys: userId, tenantId, deviceId, orderId (high cardinality)
// Bad:  status ("Active"/"Inactive"), country (few values → hot partition)

// Create container with partition key /tenantId
CosmosClient client = new CosmosClient(
    accountEndpoint: "https://cosmos-myapp.documents.azure.com:443/",
    tokenCredential: new DefaultAzureCredential());

Database db = await client.CreateDatabaseIfNotExistsAsync("ecommerce");

ContainerProperties props = new ContainerProperties
{
    Id = "orders",
    PartitionKeyPath = "/tenantId",
    DefaultTimeToLive = -1   // TTL disabled; set per-item for auto-expiry
};

Container container = await db.CreateContainerIfNotExistsAsync(props, throughput: 1000);

// Insert a document
var order = new
{
    id        = Guid.NewGuid().ToString(),
    tenantId  = "tenant-001",     // partition key
    orderId   = "ORD-42",
    customerId= "C001",
    total     = 199.99,
    status    = "Pending",
    createdAt = DateTime.UtcNow
};

await container.UpsertItemAsync(order, new PartitionKey("tenant-001"));

// Efficient single-partition query
var iterator = container.GetItemQueryIterator<dynamic>(
    "SELECT * FROM c WHERE c.tenantId = 'tenant-001' AND c.status = 'Pending'");

while (iterator.HasMoreResults)
{
    foreach (var item in await iterator.ReadNextAsync())
        Console.WriteLine(item.orderId);
}
```

**Hierarchical partition keys (SDK v3.33+)** — combine up to 3 paths to avoid cross-partition queries in multi-tenant + multi-entity scenarios:

```csharp
ContainerProperties props = new ContainerProperties("orders", "/tenantId")
{
    PartitionKeyDefinition = new PartitionKeyDefinition
    {
        Paths = { "/tenantId", "/customerId" },
        Kind  = PartitionKind.MultiHash
    }
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Azure Cosmos DB change feed and how do you use it?

The **change feed** is a persistent, ordered log of all inserts and updates (not deletes by default) to a Cosmos DB container. Consumers can read the feed to trigger real-time downstream processing — without polling the container.

**Use cases:**
- Real-time event propagation (order created → send notification)
- Materialized views / projections
- Data synchronization to Azure Search, Redis, or another container
- CQRS write-side triggers

**Consuming the change feed with the Change Feed Processor (.NET):**

```csharp
// Create a lease container (tracks read position per processor instance)
Container leaseContainer = await db.CreateContainerIfNotExistsAsync(
    new ContainerProperties("leases", "/id"), throughput: 400);

// Build the change feed processor
ChangeFeedProcessor processor = container
    .GetChangeFeedProcessorBuilder<Order>(
        processorName: "order-notifications",
        onChangesDelegate: HandleChangesAsync)
    .WithInstanceName(Environment.MachineName)
    .WithLeaseContainer(leaseContainer)
    .WithStartTime(DateTime.MinValue.ToUniversalTime())  // start from beginning
    .Build();

await processor.StartAsync();

// Handler — called with a batch of changes
static async Task HandleChangesAsync(
    ChangeFeedProcessorContext context,
    IReadOnlyCollection<Order> changes,
    CancellationToken cancellationToken)
{
    foreach (var order in changes)
    {
        Console.WriteLine($"Change: {order.OrderId} status={order.Status}");
        // Dispatch notification, update read model, etc.
    }
}
```

**Azure Functions CosmosDB trigger (simplest approach):**

```csharp
[Function("CosmosOrderProcessor")]
public void Run(
    [CosmosDBTrigger(
        databaseName: "ecommerce",
        containerName: "orders",
        Connection = "CosmosConnection",
        LeaseContainerName = "leases",
        CreateLeaseContainerIfNotExists = true)]
    IReadOnlyList<Order> changes,
    FunctionContext context)
{
    foreach (var order in changes)
        _logger.LogInformation("Order changed: {OrderId}", order.OrderId);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. ARCHITECTURE

<br>

## Q. How do you design a highly available multi-region application on Azure?

**Design principles:**
- Deploy to ≥2 Azure regions in an active-active or active-passive topology.
- Use **Azure Front Door** for global traffic management and health-based failover.
- Use **zone-redundant** services within each region (ZRS Storage, AZ-redundant SQL, AZ-redundant AKS).
- Data layer: geo-replicated database with automated failover groups.

```
                         ┌─── Azure Front Door (WAF + Global LB) ───┐
                         │                                           │
               Region A: East US                         Region B: West Europe
               ├── App Service (Active)                  ├── App Service (Active/Standby)
               ├── Azure SQL DB Primary ──────── Geo-replica ──── Azure SQL DB Secondary
               ├── Cosmos DB (Multi-write)  ←──── Replication ──── Cosmos DB (Multi-write)
               ├── Redis Cache (Zone-redundant)           ├── Redis Cache
               └── Storage (ZRS)                         └── Storage (RA-GRS secondary)
```

```bash
# Configure SQL auto-failover group (handles DNS failover transparently)
az sql failover-group create \
  --name fg-myapp \
  --server sql-myapp-eastus \
  --resource-group rg-demo \
  --partner-server sql-myapp-westeurope \
  --failover-policy Automatic \
  --grace-period 1   # hours before automatic failover

# Front Door with health probe and priority routing
az afd origin-group create \
  --profile-name afd-myapp \
  --resource-group rg-demo \
  --origin-group-name og-webapp \
  --probe-request-type GET \
  --probe-protocol Https \
  --probe-path /health \
  --probe-interval-in-seconds 30 \
  --sample-size 4 \
  --successful-samples-required 3

az afd origin create \
  --profile-name afd-myapp \
  --resource-group rg-demo \
  --origin-group-name og-webapp \
  --origin-name origin-eastus \
  --host-name myapp-eastus.azurewebsites.net \
  --priority 1 \
  --weight 1000

az afd origin create \
  --profile-name afd-myapp \
  --resource-group rg-demo \
  --origin-group-name og-webapp \
  --origin-name origin-westeurope \
  --host-name myapp-westeurope.azurewebsites.net \
  --priority 2 \
  --weight 1000
```

**RTO / RPO targets:**

| Tier | RTO | RPO | Strategy |
|------|-----|-----|---------|
| Mission-critical | < 1 min | 0 | Active-active + Cosmos multi-write |
| Business-critical | < 15 min | < 5 min | Active-passive + SQL failover group |
| Standard | < 4 hrs | < 1 hr | Backup restore + geo-replicated storage |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a Landing Zone and how does it implement governance at scale?

An **Azure Landing Zone** is a pre-configured, governed Azure environment composed of management groups, policies, RBAC, networking, identity, and logging baseline — ready for workload deployment. Microsoft\'s Cloud Adoption Framework (CAF) defines the reference architecture.

```
Root Management Group
├── Platform MG
│   ├── Identity Subscription        (Entra Connect, Domain Services)
│   ├── Management Subscription      (Log Analytics, Sentinel, Azure Monitor)
│   └── Connectivity Subscription    (Hub VNet, Azure Firewall, ExpressRoute GW)
└── Landing Zones MG
    ├── Corp MG                       (no internet egress, VNet peered to hub)
    │   ├── Subscription: Finance App
    │   └── Subscription: HR App
    └── Online MG                     (internet-facing workloads)
        └── Subscription: E-Commerce
```

**Policy examples applied at Management Group scope:**

```bash
# Enforce all storage accounts must use HTTPS only
az policy assignment create \
  --name "storage-https-only" \
  --policy "$(az policy definition list \
      --query "[?displayName=='Secure transfer to storage accounts should be enabled'].name" \
      -o tsv)" \
  --scope "/providers/Microsoft.Management/managementGroups/LandingZones" \
  --enforcement-mode Default

# Deny public IP creation in Corp landing zone
az policy definition create \
  --name "deny-public-ip" \
  --display-name "Deny Public IP Creation" \
  --mode All \
  --rules '{
    "if": {
      "field": "type",
      "equals": "Microsoft.Network/publicIPAddresses"
    },
    "then": { "effect": "Deny" }
  }'

az policy assignment create \
  --name "no-public-ip-corp" \
  --policy "deny-public-ip" \
  --scope "/providers/Microsoft.Management/managementGroups/Corp"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you architect a Zero Trust network on Azure?

**Zero Trust** assumes breach — verify explicitly, use least-privilege access, and assume all network traffic is potentially hostile, including internal traffic.

**Azure Zero Trust pillars:**

| Pillar | Azure Service | Control |
|--------|-------------|---------|
| **Identity** | Microsoft Entra ID + PIM + Conditional Access | MFA, device compliance, risk-based access |
| **Devices** | Microsoft Intune + Entra ID Join | Device health attestation |
| **Network** | Azure Firewall + NSG + Private Endpoints | Micro-segmentation, encrypted traffic |
| **Applications** | App Proxy + APIM + WAF | Authenticated, WAF-protected access |
| **Data** | Purview + Key Vault + Customer-managed keys | Classify, encrypt, audit |
| **Infrastructure** | Azure Policy + Defender for Cloud + JIT VM access | Harden posture, audit compliance |

```bash
# Enable Just-In-Time VM access (Zero Trust for VMs — no always-open RDP/SSH)
az security jit-policy create \
  --resource-group rg-demo \
  --name jit-vm-webserver \
  --vm-id $(az vm show --name vm-webserver --resource-group rg-demo --query id -o tsv) \
  --ports '[{
    "number": 22,
    "protocol": "TCP",
    "allowedSourceAddressPrefix": "MY.CORP.IP.RANGE",
    "maxRequestAccessDuration": "PT3H"
  }]'

# Enable Defender for Cloud (Posture Management + Threat Protection)
az security pricing create \
  --name VirtualMachines \
  --tier Standard

az security pricing create \
  --name SqlServers \
  --tier Standard

# Enforce Private Endpoints — disable public network access on SQL Server
az sql server update \
  --name sql-myapp \
  --resource-group rg-demo \
  --public-network-access Disabled
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/security/zero-trust/media/diagram-zero-trust-security-elements.png" alt="Azure Zero Trust Model" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you design a hub-spoke network topology in Azure?

The **hub-spoke** (or hub-and-spoke) topology uses a central **hub VNet** hosting shared services (Azure Firewall, VPN Gateway, Bastion, DNS) peered to multiple **spoke VNets** (one per application or environment). All inter-spoke traffic routes through the hub firewall.

```
                        Hub VNet (10.0.0.0/16)
                        ├── snet-firewall   → Azure Firewall (inspect all traffic)
                        ├── snet-gateway    → VPN/ExpressRoute Gateway
                        ├── AzureBastionSubnet → Azure Bastion (secure VM access)
                        └── snet-shared     → DNS, monitoring, shared services
                               ↑         ↑
                VNet Peering   │         │   VNet Peering
                               │         │
          Spoke A (10.1.0.0/16)         Spoke B (10.2.0.0/16)
          Finance App                   E-Commerce App
```

```bash
# Create hub VNet
az network vnet create \
  --name vnet-hub \
  --resource-group rg-network \
  --address-prefix 10.0.0.0/16

# Create Azure Firewall subnet (must be named AzureFirewallSubnet)
az network vnet subnet create \
  --name AzureFirewallSubnet \
  --vnet-name vnet-hub \
  --resource-group rg-network \
  --address-prefix 10.0.0.0/26

# Create spoke VNet
az network vnet create \
  --name vnet-spoke-finance \
  --resource-group rg-demo \
  --address-prefix 10.1.0.0/16

# Peer hub → spoke (with gateway transit)
az network vnet peering create \
  --name hub-to-finance \
  --resource-group rg-network \
  --vnet-name vnet-hub \
  --remote-vnet vnet-spoke-finance \
  --allow-gateway-transit \
  --allow-vnet-access \
  --allow-forwarded-traffic

# Peer spoke → hub (use remote gateway)
az network vnet peering create \
  --name finance-to-hub \
  --resource-group rg-demo \
  --vnet-name vnet-spoke-finance \
  --remote-vnet vnet-hub \
  --use-remote-gateways \
  --allow-vnet-access \
  --allow-forwarded-traffic

# Route all spoke egress through Azure Firewall (UDR)
az network route-table create \
  --name rt-spoke-finance \
  --resource-group rg-demo

az network route-table route create \
  --route-table-name rt-spoke-finance \
  --resource-group rg-demo \
  --name default-to-firewall \
  --address-prefix 0.0.0.0/0 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.0.4   # Azure Firewall private IP
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/images/hub-spoke.png" alt="Hub-Spoke Network Topology in Azure" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you design an event-driven microservices architecture on Azure?

**Event-driven architecture** decouples producers from consumers using an asynchronous message broker. Each microservice reacts to events rather than being called directly, enabling independent scaling and fault isolation.

```
User Action (HTTP) → API Management
                          ↓
                    Order Service (App Service)
                          ↓ publish event
                    Azure Service Bus Topic: "orders"
                    ├── Subscription: inventory-service   → Inventory Service (Function)
                    ├── Subscription: payment-service     → Payment Service (Container App)
                    └── Subscription: notification-svc    → Notification Service (Function)
                                                                      ↓
                                                             Azure Communication Services (email/SMS)

High-volume streaming (IoT / clickstream) → Azure Event Hubs → Azure Stream Analytics → Cosmos DB
```

```csharp
// Publish an order event to Service Bus (producer)
using Azure.Messaging.ServiceBus;

var client = new ServiceBusClient(
    "sb-myapp.servicebus.windows.net",
    new DefaultAzureCredential());

var sender = client.CreateSender("orders");

var orderEvent = new OrderPlacedEvent
{
    OrderId    = Guid.NewGuid().ToString(),
    CustomerId = "C1234",
    Total      = 299.99m,
    OccurredAt = DateTimeOffset.UtcNow
};

await sender.SendMessageAsync(
    new ServiceBusMessage(JsonSerializer.Serialize(orderEvent))
    {
        Subject = "OrderPlaced",
        ContentType = "application/json",
        SessionId = orderEvent.CustomerId   // FIFO ordering per customer
    });

// Consume event (Service Bus trigger — inventory microservice)
[Function("ProcessInventory")]
public async Task Run(
    [ServiceBusTrigger("orders", "inventory-service",
        Connection = "ServiceBusConnection")] ServiceBusReceivedMessage msg,
    ServiceBusMessageActions actions)
{
    var order = JsonSerializer.Deserialize<OrderPlacedEvent>(msg.Body);
    await _inventoryService.ReserveItemsAsync(order!);
    await actions.CompleteMessageAsync(msg);   // remove from queue on success
}
```

```bash
# Create Service Bus namespace, topic, and subscriptions
az servicebus namespace create \
  --name sb-myapp \
  --resource-group rg-demo \
  --sku Standard

az servicebus topic create \
  --namespace-name sb-myapp \
  --resource-group rg-demo \
  --name orders \
  --max-size 1024

az servicebus topic subscription create \
  --namespace-name sb-myapp \
  --resource-group rg-demo \
  --topic-name orders \
  --name inventory-service \
  --dead-letter-on-message-expiration true \
  --max-delivery-count 5
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a disaster recovery strategy with RTO < 15 minutes?

**DR strategy for RTO < 15 min (active-passive with automated failover):**

1. **Compute**: Azure App Service / AKS in secondary region (warm standby — min 1 instance running).
2. **Database**: SQL auto-failover group or Cosmos DB multi-region writes.
3. **DNS / Traffic**: Azure Front Door health probe detects failure and re-routes within 1-3 minutes.
4. **Storage**: RA-GRS or GZRS — read endpoint in secondary region available immediately.
5. **Secrets / Config**: Key Vault in secondary region or access via Private Link from secondary.

```bash
# SQL Auto-Failover Group — transparent DNS failover, RTO ~30 seconds
az sql failover-group create \
  --name fg-myapp \
  --server sql-myapp-eastus \
  --resource-group rg-prod \
  --partner-server sql-myapp-westeurope \
  --failover-policy Automatic \
  --grace-period 1

# Manual failover test (non-destructive — switches roles, no data loss)
az sql failover-group set-primary \
  --name fg-myapp \
  --server sql-myapp-westeurope \
  --resource-group rg-prod

# Connection string uses failover group listener — app code unchanged
# Server: fg-myapp.database.windows.net  (always points to primary)
```

```yaml
# Front Door: health probe detects primary failure, routes to secondary automatically
# Configure health probe on /health endpoint that returns 200 only when app is healthy
# Front Door SLA: failover in < 3 minutes when all probes from multiple PoPs fail
```

**DR Runbook (automated with Azure Automation or Logic App):**

```bash
# Azure Automation runbook — trigger DR failover
# Triggered by: Azure Monitor alert OR manual invocation
az automation runbook create \
  --resource-group rg-ops \
  --automation-account-name aa-ops \
  --name DR-Failover \
  --type PowerShell

# Test DR: initiate failover drill quarterly
az sql failover-group set-primary \
  --name fg-myapp \
  --server sql-myapp-westeurope \
  --resource-group rg-prod
# Validate → fail back
az sql failover-group set-primary \
  --name fg-myapp \
  --server sql-myapp-eastus \
  --resource-group rg-prod
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you architect a data lake and analytics platform on Azure?

**Modern Data Architecture (Medallion pattern) on Azure:**

```
External Sources / On-Prem DBs / SaaS APIs
        ↓  (Azure Data Factory / Event Hubs)
Bronze Layer  → Raw data, unchanged (Azure Data Lake Storage Gen2)
        ↓  (Azure Databricks / Synapse Spark)
Silver Layer  → Cleansed, conformed, deduplicated (ADLS Gen2 - Delta format)
        ↓  (Databricks / Synapse Analytics)
Gold Layer    → Aggregated, business-ready tables (Azure Synapse Dedicated Pool / Delta)
        ↓
Consumption:
├── Power BI (dashboards and reports)
├── Azure Synapse Serverless SQL (ad-hoc analysis)
├── Cosmos DB (serving layer for APIs)
└── Azure Machine Learning (model training)
```

```bash
# Create Data Lake Storage Gen2 (hierarchical namespace)
az storage account create \
  --name adlsmydatalake \
  --resource-group rg-data \
  --location eastus \
  --sku Standard_GZRS \
  --kind StorageV2 \
  --enable-hierarchical-namespace true \
  --min-tls-version TLS1_2

# Create medallion containers
for layer in bronze silver gold; do
  az storage fs create \
    --name $layer \
    --account-name adlsmydatalake \
    --auth-mode login
done

# Create Azure Synapse workspace
az synapse workspace create \
  --name synapse-analytics \
  --resource-group rg-data \
  --location eastus \
  --storage-account adlsmydatalake \
  --file-system gold \
  --sql-admin-login-user synapseadmin \
  --sql-admin-login-password "$(openssl rand -base64 16)!"

# Ingest data with ADF pipeline from SQL to Bronze
az datafactory pipeline create \
  --factory-name adf-ingest \
  --resource-group rg-data \
  --name ingest-orders-bronze \
  --pipeline @pipeline-sql-to-adls.json
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement cost optimization at enterprise scale on Azure?

**Cost optimization is a continuous process across multiple dimensions:**

| Lever | Technique | Typical Saving |
|-------|-----------|---------------|
| **Right-sizing** | Advisor recommendations, auto-shutdown dev VMs | 20–40% |
| **Reserved Instances** | 1- or 3-year commit for steady-state workloads | 30–72% |
| **Spot VMs** | Use for batch, stateless, fault-tolerant jobs | Up to 90% |
| **Serverless** | Functions/Consumption vs always-on VMs | 60–80% |
| **Storage tiering** | Auto lifecycle policies (Hot → Cool → Archive) | 40–60% on blobs |
| **Azure Hybrid Benefit** | Bring Windows/SQL Server licenses | Up to 40% |
| **Dev/Test pricing** | Non-prod subscriptions with Visual Studio subs | 40–60% |
| **Tagging + chargebacks** | Tag all resources; alert per-team budgets | Accountability |

```bash
# Get Advisor cost recommendations
az advisor recommendation list \
  --category Cost \
  --query "[].{Resource:impactedValue,Impact:impact,Recommendation:shortDescription.solution}" \
  --output table

# Create a lifecycle management policy (auto-tier blobs)
az storage account management-policy create \
  --account-name stproddata2025 \
  --resource-group rg-demo \
  --policy '{
    "rules": [{
      "name": "auto-tier-policy",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": { "blobTypes": ["blockBlob"], "prefixMatch": ["logs/"] },
        "actions": {
          "baseBlob": {
            "tierToCool":    { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete":        { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }]
  }'

# Set budget with alert at 80% and 100% thresholds
az consumption budget create \
  --budget-name prod-monthly \
  --amount 10000 \
  --category Cost \
  --time-grain Monthly \
  --start-date 2025-06-01 \
  --end-date 2026-06-01 \
  --notification-threshold 80

# Scope budgets per department using tags
az consumption budget create \
  --budget-name team-backend \
  --amount 2000 \
  --category Cost \
  --time-grain Monthly \
  --start-date 2025-06-01 \
  --end-date 2026-06-01 \
  --notification-threshold 90 \
  --filter '{"tags": {"Team": ["Backend"]}}'

# Apply Azure Hybrid Benefit to existing VM
az vm update \
  --name vm-sqlserver \
  --resource-group rg-demo \
  --license-type Windows_Server
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you evaluate performance bottlenecks in a production Azure system?

**Systematic performance investigation across tiers:**

```
Request → Front Door / CDN → App Service / AKS → Database / Cache → Storage
   ↓             ↓                  ↓                   ↓
Latency?     Cache miss?       CPU/Memory?         Slow queries?
             TLS overhead?     Connection pool?     Missing index?
                               GC pressure?         Lock contention?
```

```bash
# 1. Identify slow endpoints from Application Insights (KQL)
# Run in Log Analytics workspace:
```

```kusto
// Top 10 slowest and most called endpoints (P95 latency)
requests
| where timestamp >= ago(1h)
| summarize
    CallCount   = count(),
    P50         = percentile(duration, 50),
    P95         = percentile(duration, 95),
    P99         = percentile(duration, 99),
    FailureRate = round(100.0 * countif(success == false) / count(), 2)
  by name
| order by P95 desc
| take 15

// Slow SQL dependency calls
dependencies
| where timestamp >= ago(1h)
| where type == "SQL"
| where duration > 1000
| summarize SlowCallCount = count(), AvgDuration = avg(duration) by name, target
| order by SlowCallCount desc
```

```bash
# 2. Check App Service metrics (CPU, memory, HTTP queue)
az monitor metrics list \
  --resource $(az webapp show --name mywebapp-demo2025 --resource-group rg-demo --query id -o tsv) \
  --metric "CpuPercentage,MemoryWorkingSet,HttpResponseTime,Http5xx" \
  --interval PT1M \
  --start-time $(date -u -d '-1 hour' '+%Y-%m-%dT%H:%MZ') \
  --output table

# 3. Enable profiler on App Service (captures CPU flame graphs from production traffic)
az webapp config appsettings set \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --settings ApplicationInsightsProfiler=1

# 4. Detect missing SQL indexes via Query Performance Insight (KQL on sys tables)
# Connect to Azure SQL and run:
# SELECT TOP 20 qs.total_elapsed_time / qs.execution_count AS avg_elapsed,
#        qs.execution_count, SUBSTRING(st.text, 1, 200) AS query_text
# FROM sys.dm_exec_query_stats qs
# CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
# ORDER BY avg_elapsed DESC

# 5. Scale out App Service to handle load
az monitor autoscale update \
  --name autoscale-webapp \
  --resource-group rg-demo \
  --min-count 3 \
  --max-count 20

# 6. Enable Redis cache (cache-aside pattern reduces DB load)
az redis create \
  --name redis-myapp \
  --resource-group rg-demo \
  --location eastus \
  --sku Standard \
  --vm-size c1
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Azure Well-Architected Framework and what are its five pillars?

The **Azure Well-Architected Framework (WAF)** is a set of guiding tenets to improve the quality of cloud workloads across five pillars:

| Pillar | Key Focus |
|--------|-----------|
| **Reliability** | Design for failure. Use Availability Zones, geo-redundancy, health probes, circuit breakers. |
| **Security** | Zero-trust, defence in depth, Managed Identities, Key Vault, RBAC, WAF. |
| **Cost Optimization** | Right-size resources, use Reserved Instances, autoscale, lifecycle policies. |
| **Operational Excellence** | IaC (Bicep/Terraform), CI/CD, observability, chaos engineering. |
| **Performance Efficiency** | Horizontal scaling, caching (Redis), CDN/Front Door, async messaging. |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you design a highly available multi-region architecture in Azure?

**Availability Zones (AZ)** — 3 physically separate datacenters within a region (milliseconds apart). Choose zone-redundant SKUs:
- **Standard Load Balancer** with zone-redundant frontend IP
- **Zone-redundant Azure SQL Database** (Business Critical / General Purpose)
- **ZRS Storage**
- **Zone-redundant AKS node pools**

**Multi-region active-passive architecture:**

```
Region A (Primary)           Region B (Secondary)
├── App Service (Prod)        ├── App Service (Standby)
├── Azure SQL DB (Primary)    ├── Azure SQL DB (Geo-replica)
├── Cosmos DB (Write)         └── Cosmos DB (Read replica)
└── Azure Front Door ←──────────── (Global traffic routing)
        ↑
    End Users
```

**Azure Front Door for global failover:**

```bash
az afd profile create \
  --profile-name afd-myapp \
  --resource-group rg-myapp \
  --sku Standard_AzureFrontDoor

az afd endpoint create \
  --profile-name afd-myapp \
  --resource-group rg-myapp \
  --endpoint-name myapp-global
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/images/multi-region-web-app-diagram.png" alt="Azure Multi-Region High Availability Architecture" width="800px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Service Bus queues, Event Grid, and Event Hubs?

| Aspect | Service Bus Queues/Topics | Event Grid | Event Hubs |
|--------|--------------------------|------------|------------|
| Pattern | Message broker (push-pull) | Event routing (push-push) | Event streaming |
| Ordering | Sessions (FIFO) | No ordering | Within partition |
| Retention | Up to 14 days | 24 hours retry | 1–90 days |
| Throughput | Millions/sec (Premium) | 10M events/sec | Millions events/sec |
| Dead-letter | Yes | No | No |
| Best for | Workflow, commands, RPC | Reactive triggers (resource events) | Log streaming, telemetry, CDC |

**Subscribe to Blob Storage events via Event Grid:**

```bash
az eventgrid event-subscription create \
  --name blob-events-sub \
  --source-resource-id $(az storage account show --name stmyapp001 --resource-group rg-myapp --query id -o tsv) \
  --endpoint "https://myapp.azurewebsites.net/api/storageevents" \
  --endpoint-type webhook \
  --included-event-types "Microsoft.Storage.BlobCreated" "Microsoft.Storage.BlobDeleted"
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/event-grid/media/overview/functional-model.png" alt="Azure Event Grid Functional Model" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are ARM templates and Bicep, and how do they support infrastructure as code?

**Azure Resource Manager (ARM) templates** are JSON files that declaratively describe Azure resources. **Bicep** is a DSL that compiles to ARM JSON — it is the recommended IaC language for Azure-native deployments (cleaner syntax, type-safe, IDE support).

**Bicep example — App Service:**

```bicep
param environment string = 'dev'
param location string = resourceGroup().location
param appName string = 'myapp-${environment}'

resource appServicePlan 'Microsoft.Web/serverfarms@2023-12-01' = {
  name: 'asp-${appName}'
  location: location
  sku: { name: environment == 'prod' ? 'P2V3' : 'B1' }
}

resource webApp 'Microsoft.Web/sites@2023-12-01' = {
  name: appName
  location: location
  identity: { type: 'SystemAssigned' }
  properties: {
    serverFarmId: appServicePlan.id
    httpsOnly: true
    siteConfig: { minTlsVersion: '1.2', ftpsState: 'Disabled' }
  }
}

output webAppUrl string = 'https://${webApp.properties.defaultHostName}'
```

```bash
az deployment group create \
  --resource-group rg-myapp \
  --template-file main.bicep \
  --parameters environment=prod
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement disaster recovery in Azure?

**Recovery objectives:**
- **RTO** (Recovery Time Objective) — maximum acceptable downtime.
- **RPO** (Recovery Point Objective) — maximum acceptable data loss.

**Azure DR services by scenario:**

| Scenario | Azure Service | RTO | RPO |
|----------|--------------|-----|-----|
| VM workloads | Azure Site Recovery | Minutes | ~1 min |
| SQL Database | Auto-failover groups | 30 sec | < 5 sec |
| App Service | Deployment slots + Traffic Manager | Minutes | 0 (code) |
| Multi-region | Azure Front Door + geo-replicated DB | Seconds | Seconds |

```bash
# Enable Site Recovery replication for a VM
az site-recovery protected-item create \
  --resource-group rg-asr \
  --vault-name rsv-dr \
  --fabric-name fabric-eastus \
  --container-name container-primary \
  --replication-protected-item-name vm-webserver-rep \
  --vm-id $(az vm show --name vm-webserver --resource-group rg-myapp --query id -o tsv)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. MIGRATION

<br>

## Q. What is Azure Migrate and how do you use the 6 R\'s to plan a migration?

**Azure Migrate** is the central hub for discovering, assessing, and migrating on-premises workloads (VMs, databases, web apps, virtual desktops) to Azure. The **6 R\'s** define the migration strategy for each workload:

| Strategy | Description | Example |
|----------|-------------|---------|
| **Rehost** (lift-and-shift) | Move as-is to Azure IaaS | On-prem VM → Azure VM |
| **Replatform** | Minimal changes to leverage managed services | IIS on VM → Azure App Service |
| **Refactor** | Code changes to adopt cloud-native patterns | Monolith → microservices on AKS |
| **Rearchitect** | Significant redesign for cloud capabilities | Redesign with Cosmos DB + Functions |
| **Rebuild** | Rewrite from scratch on cloud-native stack | New app on AKS + Azure SQL |
| **Retire** | Decommission unused workloads | Legacy app no longer needed |

```bash
# Install Azure Migrate appliance and start discovery
az migrate project create \
  --name "MyMigrateProject" \
  --resource-group rg-migration \
  --location eastus

# Create assessment for discovered servers
az offazure server site create \
  --name "OnPremSite" \
  --resource-group rg-migration \
  --project-name "MyMigrateProject"
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/migrate/media/migrate-services-overview/migrate-journey.png" alt="Azure Migrate - Discover, Assess, and Migrate" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you migrate on-premises VMs to Azure using Azure Migrate?

The migration process involves four phases — **Discover → Assess → Replicate → Migrate**:

1. **Discover**: Deploy the Azure Migrate appliance on-premises; it auto-discovers VMs using vCenter or Hyper-V APIs.
2. **Assess**: Evaluate workloads for Azure readiness, sizing, and estimated monthly cost.
3. **Replicate**: Enable replication of VMs to Azure (uses Azure Site Recovery under the hood).
4. **Test failover**: Validate the migrated VM in an isolated VNet without impacting production.
5. **Migrate (cutover)**: Stop replication, shut down source VM, and complete failover.

```bash
# Install the Migrate and modernize replication appliance, then enable replication via portal.
# Monitor replication health
az site-recovery replication-protected-item list \
  --resource-group rg-migration \
  --vault-name rsv-migrate

# Complete migration (cutover)
az site-recovery replication-protected-item planned-failover \
  --resource-group rg-migration \
  --vault-name rsv-migrate \
  --fabric-name fabric-primary \
  --protection-container-name container-primary \
  --replication-protected-item-name vm-webapp-rep
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Database Migration Service and how do you migrate an on-premises SQL Server to Azure SQL?

**Azure Database Migration Service (DMS)** enables online (near-zero-downtime) and offline migrations from SQL Server, MySQL, PostgreSQL, MongoDB, and Oracle to Azure managed database services.

**Online migration** uses CDC (Change Data Capture) to sync changes continuously while cutover happens with minimal downtime. **Offline migration** copies a one-time snapshot, so the source must be taken offline.

```bash
# Create a DMS instance
az dms create \
  --name dms-sql-migration \
  --resource-group rg-migration \
  --location eastus \
  --sku-name Premium_4vCores \
  --subnet $(az network vnet subnet show \
    --resource-group rg-migration \
    --vnet-name vnet-migration \
    --name snet-dms \
    --query id -o tsv)

# Create migration project
az dms project create \
  --name SqlMigrationProject \
  --service-name dms-sql-migration \
  --resource-group rg-migration \
  --location eastus \
  --source-platform SQL \
  --target-platform AzureSqlMi

# Start an online migration task
az dms project task create \
  --database-options-json @db-options.json \
  --name SqlMigrationTask \
  --project-name SqlMigrationProject \
  --resource-group rg-migration \
  --service-name dms-sql-migration \
  --source-connection-json @source-conn.json \
  --target-connection-json @target-conn.json \
  --task-type OnlineMigration
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When should you use Azure Data Box and how does it compare to AzCopy for data migration?

**Azure Data Box** is a physical device shipped to your datacenter to move large datasets (40 TB–1 PB) offline. Use it when:
- Transfer over the network would take **more than 2 weeks**
- Network bandwidth is limited or costly
- Data is too sensitive to traverse the public internet
- Connectivity to Azure is unreliable

**AzCopy** is a CLI tool for online transfers; ideal when network bandwidth is sufficient and data volumes are smaller.

| Factor | Azure Data Box | AzCopy |
|--------|--------------|--------|
| Transfer mode | Offline (physical shipment) | Online (network) |
| Max capacity | 1 PB (Data Box Heavy) | Unlimited (depends on bandwidth) |
| Speed | ~1 Gbps (USB) → ship to Azure | Up to 10+ Gbps on ExpressRoute |
| Setup | Order device → copy data → ship | Install CLI → run command |
| Best for | Multi-TB offline migrations | Day-to-day transfers, ongoing sync |

```bash
# AzCopy: copy entire on-prem folder to Blob Storage
azcopy copy "C:\data\exports" \
  "https://stmigration.blob.core.windows.net/rawdata?<SAS-token>" \
  --recursive=true \
  --check-md5=FailIfDifferent

# Monitor AzCopy job progress
azcopy jobs list
azcopy jobs show <job-id>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure Data Factory vs Azure Database Migration Service for data migration?

| Aspect | Azure Data Factory (ADF) | Azure Database Migration Service (DMS) |
|--------|--------------------------|----------------------------------------|
| Purpose | ETL/ELT data pipelines (ongoing or one-time) | Lift-and-shift database schema + data migration |
| Supports | 90+ connectors (files, DBs, SaaS, REST) | SQL Server, MySQL, PostgreSQL, MongoDB, Oracle |
| Schema migration | No (data movement only) | Yes (full schema + data + stored procedures) |
| CDC / online migration | Via Self-Hosted IR + Change Tracking | Native CDC-based online migration |
| Best for | Complex transformations, data lake ingestion | Homogeneous/heterogeneous DB migrations |

```bash
# ADF: Create a pipeline that copies SQL Server table to Azure SQL Database
az datafactory pipeline create \
  --factory-name adf-migration \
  --resource-group rg-migration \
  --name "SqlCopyPipeline" \
  --pipeline @pipeline-definition.json

# Trigger the pipeline
az datafactory pipeline create-run \
  --factory-name adf-migration \
  --resource-group rg-migration \
  --name "SqlCopyPipeline"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Integration Runtime in Azure Data Factory and what are its types?

**Integration Runtime (IR)** is the compute infrastructure used by Azure Data Factory to perform data movement, activity dispatch, and SSIS package execution. There are three types:

1. **Azure IR** — Fully managed, serverless; connects to cloud data stores and services within Azure or across public endpoints. Auto-scales, no management required.

2. **Self-Hosted IR** — Installed on an on-premises machine or Azure VM inside a private network. Enables ADF to connect to on-premises data sources, data behind firewalls, or in private VNets.

3. **Azure-SSIS IR** — Managed cluster of Azure VMs for running legacy SSIS packages natively in ADF without code changes. Connects to SSISDB in Azure SQL or Managed Instance.

```bash
# Create a Self-Hosted IR (then install the agent on-premises)
az datafactory integration-runtime self-hosted create \
  --factory-name adf-migration \
  --resource-group rg-migration \
  --name "OnPremIR"

# Get the authentication key to register the on-prem agent
az datafactory integration-runtime list-auth-key \
  --factory-name adf-migration \
  --resource-group rg-migration \
  --name "OnPremIR"

# Create Azure-SSIS IR for SSIS workloads
az datafactory integration-runtime managed create \
  --factory-name adf-migration \
  --resource-group rg-migration \
  --name "AzureSSISIR" \
  --location eastus \
  --type Managed \
  --compute-properties '{\"location\":\"eastus\",\"nodeSize\":\"Standard_D4_v3\",\"numberOfNodes\":1,\"maxParallelExecutionsPerNode\":4}'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main components of Azure Data Factory and how do you build a pipeline?

**Azure Data Factory (ADF)** is a cloud-native ETL/ELT service for building, scheduling, and orchestrating data workflows across 90+ connectors.

**Core components:**

| Component | Description |
|-----------|-------------|
| **Pipeline** | Logical container grouping activities into a workflow |
| **Activity** | A single processing step (Copy, Mapping Data Flow, Lookup, ForEach, etc.) |
| **Dataset** | Named reference to data in a linked service (e.g., a Blob file, a SQL table) |
| **Linked Service** | Connection string / credentials to a data store or compute (Azure SQL, Blob, Databricks) |
| **Integration Runtime (IR)** | Compute infrastructure that executes activities (Azure IR, Self-Hosted IR, Azure-SSIS IR) |
| **Trigger** | Schedule (tumbling window / recurrence), event (Blob created), or manual |

**Build a Copy pipeline (JSON definition):**

```json
{
  "name": "CopyBlobToSql",
  "properties": {
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs":  [{ "referenceName": "SourceBlobDataset",  "type": "DatasetReference" }],
        "outputs": [{ "referenceName": "SinkSqlDataset",     "type": "DatasetReference" }],
        "typeProperties": {
          "source": {
            "type": "DelimitedTextSource",
            "storeSettings": { "type": "AzureBlobStorageReadSettings", "recursive": false }
          },
          "sink": {
            "type": "AzureSqlSink",
            "writeBehavior": "upsert",
            "upsertSettings": { "useTempDB": true, "keys": ["OrderId"] }
          },
          "enableStaging": false,
          "translator": { "type": "TabularTranslator", "typeConversion": true }
        }
      }
    ],
    "parameters": {
      "sourceFolder": { "type": "string", "defaultValue": "raw/orders/" }
    }
  }
}
```

**Deploy pipeline via CLI:**

```bash
# Create ADF instance
az datafactory create \
  --name adf-myapp \
  --resource-group rg-data \
  --location eastus

# Create a linked service (Azure Blob Storage using Managed Identity)
az datafactory linked-service create \
  --factory-name adf-myapp \
  --resource-group rg-data \
  --name "BlobLinkedService" \
  --properties '{
    "type": "AzureBlobStorage",
    "typeProperties": {
      "serviceEndpoint": "https://stmyapp.blob.core.windows.net/",
      "accountKind": "StorageV2"
    },
    "connectVia": { "referenceName": "AutoResolveIntegrationRuntime", "type": "IntegrationRuntimeReference" }
  }'

# Trigger pipeline run manually
az datafactory pipeline create-run \
  --factory-name adf-myapp \
  --resource-group rg-data \
  --name CopyBlobToSql \
  --parameters '{"sourceFolder": "raw/orders/2025-05/"}'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you monitor and troubleshoot Azure Data Factory pipelines?

ADF provides a built-in **Monitor** hub and integrates with Azure Monitor, Log Analytics, and Azure Alerts for operational visibility.

**Monitoring approaches:**

| Approach | Details |
|----------|---------|
| **ADF Monitor Hub** | Built-in UI — pipeline runs, activity runs, trigger runs, duration, status |
| **Azure Monitor Metrics** | Pipeline succeeded/failed/cancelled count, activity run duration |
| **Log Analytics** | ADF diagnostic logs (ADFPipelineRun, ADFActivityRun) for custom KQL queries |
| **Azure Alerts** | Alert on pipeline failure, trigger failure, or high duration |

**Send ADF diagnostic logs to Log Analytics:**

```bash
az monitor diagnostic-settings create \
  --name adf-logs \
  --resource $(az datafactory show --name adf-myapp --resource-group rg-data --query id -o tsv) \
  --workspace $(az monitor log-analytics workspace show --workspace-name law-myapp --resource-group rg-data --query id -o tsv) \
  --logs '[
    {"category":"PipelineRuns","enabled":true},
    {"category":"ActivityRuns","enabled":true},
    {"category":"TriggerRuns","enabled":true}
  ]'
```

**KQL — find failed pipeline runs in the last 24 hours:**

```kusto
ADFPipelineRun
| where TimeGenerated >= ago(24h)
| where Status == "Failed"
| project TimeGenerated, PipelineName, Status, FailureType, ErrorCode, ErrorMessage
| order by TimeGenerated desc
```

**KQL — identify slow Copy activities:**

```kusto
ADFActivityRun
| where TimeGenerated >= ago(7d)
| where ActivityType == "Copy"
| extend DurationMin = End - Start
| summarize AvgDuration = avg(DurationMin), MaxDuration = max(DurationMin) by ActivityName
| order by MaxDuration desc
```

**Create an alert for pipeline failure:**

```bash
az monitor metrics alert create \
  --name "ADF Pipeline Failure Alert" \
  --resource-group rg-data \
  --scopes $(az datafactory show --name adf-myapp --resource-group rg-data --query id -o tsv) \
  --condition "count PipelineFailedRuns > 0" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --action $(az monitor action-group show --name ag-oncall --resource-group rg-data --query id -o tsv)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. ACCESS MANAGEMENT

<br>

## Q. What is Microsoft Entra ID and how does it differ from on-premises Active Directory?

**Microsoft Entra ID** (formerly Azure Active Directory) is Microsoft\'s cloud-based identity and access management (IAM) service. It is **not** a cloud version of on-premises Active Directory — it is a fundamentally different directory designed for modern web protocols.

| Feature | On-premises Active Directory | Microsoft Entra ID |
|---------|-----------------------------|--------------------|
| Protocols | Kerberos, NTLM, LDAP | OAuth 2.0, OpenID Connect, SAML |
| Structure | OUs, GPOs, Domain Controllers | Flat tenant, no OUs or GPOs |
| Device join | Domain join | Entra ID join / Hybrid join |
| App integration | On-prem apps via LDAP | SaaS apps, Microsoft 365, custom apps |
| MFA | Third-party / NPS extension | Built-in (per-user or Conditional Access) |
| Scalability | Limited to DCs | Global, multi-tenant cloud scale |

```bash
# Create a user in Entra ID
az ad user create \
  --display-name "Jane Smith" \
  --user-principal-name "jsmith@contoso.onmicrosoft.com" \
  --password "P@ssword123!" \
  --force-change-password-next-sign-in true

# Add a custom domain to the tenant
az ad signed-in-user show --query "userPrincipalName"
# Then verify DNS TXT record in the portal before enabling the domain
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Role-Based Access Control (RBAC) in Azure and how do you create custom roles?

**Azure RBAC** controls who can do what on which Azure resources. A role assignment has three parts:
- **Security principal** — who (user, group, service principal, managed identity)
- **Role definition** — what permissions (built-in or custom)
- **Scope** — where (management group > subscription > resource group > resource)

**Built-in roles:** Owner, Contributor, Reader, and 100+ service-specific roles.

**Custom roles** are JSON-defined when built-in roles don\'t fit:

```bash
# List assignable built-in roles at a scope
az role definition list --query "[].{Name:roleName,Description:description}" -o table

# Create a custom role from a JSON definition
cat > custom-vm-operator.json << 'EOF'
{
  "Name": "VM Operator",
  "IsCustom": true,
  "Description": "Can start, stop and restart VMs",
  "Actions": [
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/powerOff/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Compute/virtualMachines/read"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": ["/subscriptions/{subscription-id}"]
}
EOF

az role definition create --role-definition @custom-vm-operator.json

# Assign the role to a user at resource group scope
az role assignment create \
  --assignee "jsmith@contoso.onmicrosoft.com" \
  --role "VM Operator" \
  --resource-group rg-production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Conditional Access and how do you configure a policy that requires MFA from outside the corporate network?

**Conditional Access** is the Zero Trust policy engine in Microsoft Entra ID. Policies evaluate signals (user, location, device, app, risk) and enforce controls (require MFA, block access, require compliant device).

```bash
# Conditional Access policies are created via Graph API or the portal.
# Using Microsoft Graph PowerShell:

# Example: Require MFA for all users accessing any cloud app from outside trusted IPs
$params = @{
  displayName = "Require MFA outside corporate network"
  state = "enabled"
  conditions = @{
    users = @{ includeUsers = @("All") }
    applications = @{ includeApplications = @("All") }
    locations = @{
      includeLocations = @("All")
      excludeLocations = @("00000000-0000-0000-0000-000000000000")  # Named location ID for corporate IP
    }
  }
  grantControls = @{
    operator = "OR"
    builtInControls = @("mfa")
  }
}

New-MgIdentityConditionalAccessPolicy -BodyParameter $params
```

**Key policy components:**
- **Assignments** — users/groups, cloud apps, conditions (location, device platform, sign-in risk)
- **Access controls** — Grant (require MFA, compliant device, Entra ID joined) or Block
- **Break-glass accounts** — Exclude at least one emergency admin account to avoid lockout

<p align="center">
  <img src="https://learn.microsoft.com/en-us/entra/identity/conditional-access/media/overview/conditional-access-central-policy-engine-zero-trust.png" alt="Microsoft Entra Conditional Access Policy Flow" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Privileged Identity Management (PIM) and how does it provide just-in-time access?

**Microsoft Entra PIM** provides time-bound, just-in-time (JIT) elevation of privileged roles. Instead of permanent assignment, users are **eligible** for a role and must **activate** it (with MFA + justification) when needed. Activation can be limited to minutes or hours.

**PIM benefits:**
- Reduces standing access to sensitive roles (Global Admin, Subscription Owner)
- Requires MFA and business justification for activation
- Auto-expires elevated access (1–72 hours configurable)
- Full audit log of all activations

```bash
# Using Microsoft Graph PowerShell to make a user eligible for Global Admin (PIM)
$eligibilityScheduleRequest = @{
  action = "adminAssign"
  justification = "Eligible for Global Admin via PIM"
  roleDefinitionId = "62e90394-69f5-4237-9190-012177145e10"  # Global Admin
  directoryScopeId = "/"
  principalId = "{user-object-id}"
  scheduleInfo = @{
    startDateTime = (Get-Date -Format "o")
    expiration = @{ type = "NoExpiration" }
  }
}

New-MgRoleManagementDirectoryRoleEligibilityScheduleRequest -BodyParameter $eligibilityScheduleRequest

# User self-activates the role (from PIM portal or via Graph API)
$activationRequest = @{
  action = "selfActivate"
  principalId = "{user-object-id}"
  roleDefinitionId = "62e90394-69f5-4237-9190-012177145e10"
  directoryScopeId = "/"
  justification = "Emergency configuration change"
  scheduleInfo = @{
    startDateTime = (Get-Date -Format "o")
    expiration = @{ type = "AfterDuration"; duration = "PT4H" }  # 4 hours
  }
}

New-MgRoleManagementDirectoryRoleAssignmentScheduleRequest -BodyParameter $activationRequest
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/entra/id-governance/privileged-identity-management/media/pim-how-to-add-role-to-user/select-role.png" alt="Microsoft Entra Privileged Identity Management" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Managed Identities in Azure and when should you use system-assigned vs user-assigned?

**Managed Identity** is an automatically managed Azure AD identity for Azure services, eliminating the need to store credentials in code or config. Azure manages the lifecycle of the underlying service principal.

| | System-Assigned | User-Assigned |
|---|---|---|
| Lifecycle | Tied to the resource (deleted with it) | Exists independently |
| Sharing | One resource only | Can be shared across multiple resources |
| Use case | Single resource with unique identity | Multiple resources needing same permissions |

```csharp
// .NET 8: Access Key Vault secret using Managed Identity — no credentials in code
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var client = new SecretClient(
    new Uri("https://kv-myapp.vault.azure.net/"),
    new DefaultAzureCredential()  // Uses system-assigned MI when running in Azure
);

KeyVaultSecret secret = await client.GetSecretAsync("ConnectionString");
string connectionString = secret.Value;
```

```bash
# Enable system-assigned managed identity on a VM
az vm identity assign --name vm-webapp --resource-group rg-prod

# Grant the VM\'s identity access to Key Vault secrets
az keyvault set-policy \
  --name kv-myapp \
  --object-id $(az vm identity show --name vm-webapp --resource-group rg-prod --query principalId -o tsv) \
  --secret-permissions get list

# Create a user-assigned managed identity
az identity create --name id-shared-reader --resource-group rg-identities

# Assign it to multiple resources
az webapp identity assign \
  --name app-mywebapp \
  --resource-group rg-prod \
  --identities $(az identity show --name id-shared-reader --resource-group rg-identities --query id -o tsv)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Azure AD B2B and B2C and when do you use each?

**Azure AD B2B (Business-to-Business):** Allows external users (partners, vendors, suppliers) to access your organization\'s resources using their own identities (Microsoft accounts, Google, any IdP). They appear as **guest users** in your tenant.

**Azure AD B2C (Business-to-Consumer):** A fully separate, customer-facing identity service. It handles sign-up, sign-in, profile management, and MFA for millions of external customers using social accounts (Google, Facebook) or local email/password.

| | B2B | B2C |
|---|---|---|
| Users | Partners, vendors (trusted external users) | End customers (consumers) |
| Identity | Their existing org/social identity | Social or local account |
| Tenant | Guest in YOUR tenant | Separate B2C tenant |
| Branding | Your org\'s UX | Fully customizable customer UX |
| Volume | Hundreds to thousands | Millions |

```bash
# B2B: Invite an external partner as a guest user
az ad invitation create \
  --invited-user-email-address "partner@external.com" \
  --invite-redirect-url "https://myapp.azurewebsites.net" \
  --invited-user-display-name "External Partner"

# B2C: Create a B2C tenant (done in portal), then configure user flows via CLI
az ad b2c tenant create \
  --domain-name myappcustomers \
  --resource-group rg-b2c \
  --location "United States" \
  --display-name "My App Customers"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are password hash synchronization and pass-through authentication in Microsoft Entra ID Connect?

**Microsoft Entra Connect** (Azure AD Connect) synchronizes on-premises Active Directory identities to Microsoft Entra ID, enabling hybrid identity. Two key authentication methods:

**Password Hash Synchronization (PHS):** Syncs a hashed representation of the user\'s password to Entra ID. Authentication happens **in the cloud** — no on-premises dependency during sign-in. Best resilience.

**Pass-Through Authentication (PTA):** Authentication happens **on-premises** in real time. The cloud forwards credentials to an on-prem agent that validates against AD. No password hash stored in cloud — satisfies strict compliance requirements.

| | PHS | PTA |
|---|---|---|
| Auth location | Cloud (Entra ID) | On-premises AD |
| Password stored in cloud | Hash (one-way) | No |
| On-prem dependency | No (fails open if on-prem is down) | Yes (requires PTA agent running) |
| Leaked credential detection | Yes (via Identity Protection) | Limited |
| Seamless SSO | Yes | Yes |

```powershell
# Install Azure AD Connect on a Windows Server, then configure via wizard
# Enable Password Hash Sync (can be done post-install)
Import-Module ADSync
Set-ADSyncScheduler -SyncCycleEnabled $true

# Force a full sync
Start-ADSyncSyncCycle -PolicyType Initial

# Enable Pass-Through Authentication (in the AADConnect wizard or:)
Enable-AzureADPassThroughAuthentication -CertificateThumbprint "{thumbprint}"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure Multi-Factor Authentication (MFA) in Microsoft Entra ID?

**Microsoft Entra MFA** adds a second verification step beyond password, supporting: Microsoft Authenticator app (push/TOTP), FIDO2 security keys, SMS/voice (legacy), and Windows Hello for Business.

**MFA enforcement options:**

| Method | When to Use |
|--------|-------------|
| **Per-user MFA** (legacy) | Small orgs, simple enforcement; not recommended for large deployments |
| **Conditional Access policy** | Recommended — policy-based enforcement based on user, device, location, risk |
| **Security Defaults** | Free tier; enables MFA for all users, disables legacy auth |
| **Identity Protection risk-based MFA** | Trigger MFA only on risky sign-ins (requires Entra ID P2) |

**Create a Conditional Access policy requiring MFA for all users:**

```bash
# Enable MFA via Conditional Access using Microsoft Graph (PowerShell)
Connect-MgGraph -Scopes "Policy.ReadWrite.ConditionalAccess"

$policy = @{
  displayName = "Require MFA for All Users"
  state = "enabled"
  conditions = @{
    users = @{
      includeUsers = @("All")
      excludeUsers = @("break-glass-account-object-id")  # always exclude emergency accounts
    }
    applications = @{
      includeApplications = @("All")
    }
  }
  grantControls = @{
    operator = "OR"
    builtInControls = @("mfa")
  }
}

New-MgIdentityConditionalAccessPolicy -BodyParameter $policy
```

**Authentication Methods policy (Graph API) — enable Authenticator app:**

```bash
# Get current authentication methods policy
az rest --method GET \
  --url "https://graph.microsoft.com/v1.0/policies/authenticationMethodsPolicy"

# Enable Microsoft Authenticator for all users
az rest --method PATCH \
  --url "https://graph.microsoft.com/v1.0/policies/authenticationMethodsPolicy/authenticationMethodConfigurations/MicrosoftAuthenticator" \
  --body '{"@odata.type":"#microsoft.graph.microsoftAuthenticatorAuthenticationMethodConfiguration","state":"enabled"}'
```

**Best practices:**
- Never enforce MFA without a **break-glass (emergency access) account** excluded from all CA policies.
- Block **legacy authentication protocols** (IMAP, SMTP, POP3) — they cannot complete MFA challenges.
- Use **named locations** to exclude trusted office IPs from MFA to reduce friction.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement OAuth 2.0 and OpenID Connect authentication with Microsoft Entra ID?

**Microsoft Entra ID** is a full OAuth 2.0 / OpenID Connect (OIDC) authorization server. It issues access tokens (for API authorization) and ID tokens (for user authentication).

**Key flows:**

| Flow | When to Use |
|------|-------------|
| **Authorization Code + PKCE** | Web apps, SPAs — user interactive login |
| **Client Credentials** | Service-to-service (daemon apps, background jobs) |
| **On-Behalf-Of (OBO)** | Middle-tier API calling a downstream API on behalf of the signed-in user |
| **Device Code** | CLI tools, IoT devices with no browser |

**Register an app and configure API permissions (CLI):**

```bash
# Register the API app
az ad app create \
  --display-name "MyApp API" \
  --sign-in-audience "AzureADMyOrg"

# Expose an API scope
az ad app update \
  --id <app-id> \
  --set "api={\"oauth2PermissionScopes\":[{\"adminConsentDescription\":\"Access MyApp API\",\"adminConsentDisplayName\":\"Access MyApp API\",\"id\":\"<scope-guid>\",\"isEnabled\":true,\"type\":\"User\",\"userConsentDescription\":\"Access MyApp API\",\"userConsentDisplayName\":\"Access MyApp\",\"value\":\"access_as_user\"}]}"

# Register the client app and grant permission to the API
az ad app create --display-name "MyApp Client"
az ad app permission add \
  --id <client-app-id> \
  --api <api-app-id> \
  --api-permissions "<scope-guid>=Scope"

# Admin consent
az ad app permission admin-consent --id <client-app-id>
```

**ASP.NET Core Web API — validate Entra ID bearer tokens:**

```csharp
// Program.cs
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(builder.Configuration.GetSection("AzureAd"));

builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("RequireScope", policy =>
        policy.RequireClaim("http://schemas.microsoft.com/identity/claims/scope", "access_as_user"));
});

// appsettings.json
// {
//   "AzureAd": {
//     "Instance": "https://login.microsoftonline.com/",
//     "TenantId": "{tenant-id}",
//     "ClientId": "{api-app-id}",
//     "Audience": "api://{api-app-id}"
//   }
// }

// Controller
[Authorize(Policy = "RequireScope")]
[HttpGet("products")]
public IActionResult GetProducts() => Ok(_catalog.GetAll());
```

**Client Credentials flow (service-to-service):**

```csharp
// Acquire token for a downstream API using Managed Identity
var credential = new DefaultAzureCredential();
var tokenRequestContext = new TokenRequestContext(new[] { "api://{api-app-id}/.default" });
var token = await credential.GetTokenAsync(tokenRequestContext);

httpClient.DefaultRequestHeaders.Authorization =
    new AuthenticationHeaderValue("Bearer", token.Token);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. PERFORMANCE MANAGEMENT

<br>

## Q. How do you implement zero-downtime deployments on Azure App Service?

**Deployment slots** enable blue-green deployments. Deploy to the staging slot, run smoke tests, then swap traffic instantly with zero downtime.

```bash
# 1. Create staging slot
az webapp deployment slot create \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --slot staging

# 2. Set slot-sticky settings (don\'t swap with code)
az webapp config appsettings set \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --slot staging \
  --slot-settings DB_CONNECTION_STRING="<staging-conn>"

# 3. Deploy new version to staging
az webapp deploy \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --slot staging \
  --src-path ./app-v2.zip \
  --type zip

# 4. Validate staging at: https://mywebapp-demo2025-staging.azurewebsites.net

# 5. Swap staging → production (zero downtime)
az webapp deployment slot swap \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --slot staging \
  --target-slot production

# 6. Rollback: swap back if issues
az webapp deployment slot swap \
  --name mywebapp-demo2025 \
  --resource-group rg-demo \
  --slot production \
  --target-slot staging
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Kubernetes Service (AKS) and how do you deploy a containerized app?

**AKS** is a managed Kubernetes service where Azure handles the control plane (API server, etcd, scheduler). You manage the node pools (worker VMs), workloads, and networking.

```bash
# 1. Create ACR and AKS cluster with ACR integration
az acr create --name acrprod2025 --resource-group rg-demo --sku Basic

az aks create \
  --name aks-demo \
  --resource-group rg-demo \
  --node-count 3 \
  --node-vm-size Standard_D2s_v3 \
  --attach-acr acrprod2025 \
  --enable-managed-identity \
  --network-plugin azure \
  --generate-ssh-keys

# 2. Connect kubectl
az aks get-credentials --name aks-demo --resource-group rg-demo

# 3. Build and push image
az acr build --registry acrprod2025 --image myapp:v1.0 .

# 4. Deploy application
kubectl apply -f deployment.yaml
```

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: acrprod2025.azurecr.io/myapp:v1.0
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: 250m
            memory: 256Mi
          limits:
            cpu: 500m
            memory: 512Mi
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-svc
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 8080
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement RBAC and Conditional Access in Microsoft Entra ID?

**RBAC (Role-Based Access Control)** grants the minimum permissions required (least privilege principle) at the correct scope.

```bash
# List built-in roles
az role definition list --query "[].{Name:roleName}" --output table | head -20

# Assign Contributor role to a user on a specific resource group
az role assignment create \
  --assignee "dev@contoso.com" \
  --role "Contributor" \
  --resource-group rg-demo

# Create a custom role — read-only + restart VM
cat > vm-restart-role.json << 'EOF'
{
  "Name": "VM Restart Operator",
  "IsCustom": true,
  "Description": "Can read and restart Azure VMs",
  "Actions": [
    "Microsoft.Compute/virtualMachines/read",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [],
  "AssignableScopes": ["/subscriptions/<sub-id>"]
}
EOF

az role definition create --role-definition @vm-restart-role.json
```

**Conditional Access policy (require MFA from non-corporate IPs):**

```powershell
# Using Microsoft Graph PowerShell SDK
$policy = @{
  displayName  = "Require MFA outside trusted locations"
  state        = "enabled"
  conditions   = @{
    users        = @{ includeUsers = @("All") }
    applications = @{ includeApplications = @("All") }
    locations    = @{
      includeLocations = @("All")
      excludeLocations = @("<named-location-id>")  # corporate IP range
    }
  }
  grantControls = @{
    operator         = "OR"
    builtInControls  = @("mfa")
  }
}
New-MgIdentityConditionalAccessPolicy -BodyParameter $policy
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Cosmos DB partitioning and how do you choose a partition key?

Cosmos DB distributes data across **logical partitions** using a **partition key**. All queries and writes to the same partition key execute within a single physical partition — efficiently using RU (Request Units). Cross-partition queries fan out to all partitions and consume more RUs.

**Good partition key criteria:**
- High cardinality (many distinct values)
- Queries filter or write by this field
- Evenly distributes reads AND writes

```csharp
// GOOD: CustomerId as partition key for an orders container
// Orders are created and queried by customer
var container = db.GetContainer("orders");

var order = new Order
{
    Id     = Guid.NewGuid().ToString(),
    CustomerId = "C1234",        // partition key
    Total  = 199.99m,
    Items  = new[] { new OrderItem("SKU-001", 2, 99.99m) }
};

// Provide partition key in all operations for best performance
await container.UpsertItemAsync(order, new PartitionKey(order.CustomerId));

// Efficient query — stays within one partition
var queryDef = new QueryDefinition("SELECT * FROM c WHERE c.customerId = @cid")
    .WithParameter("@cid", "C1234");

var results = container.GetItemQueryIterator<Order>(queryDef,
    requestOptions: new QueryRequestOptions { PartitionKey = new PartitionKey("C1234") });

while (results.HasMoreResults)
{
    var page = await results.ReadNextAsync();
    foreach (var o in page) Console.WriteLine(o.Id);
}
```

```bash
# Create Cosmos DB account + database + container
az cosmosdb create \
  --name cosmos-myapp \
  --resource-group rg-demo \
  --default-consistency-level Session \
  --locations regionName=eastus failoverPriority=0

az cosmosdb sql database create \
  --account-name cosmos-myapp \
  --resource-group rg-demo \
  --name mydb

az cosmosdb sql container create \
  --account-name cosmos-myapp \
  --resource-group rg-demo \
  --database-name mydb \
  --name orders \
  --partition-key-path "/customerId" \
  --throughput 400
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you write KQL queries in Azure Monitor Log Analytics?

**KQL (Kusto Query Language)** uses a pipe-based syntax to query tables in Log Analytics. Each pipe operator transforms the previous result set.

```kusto
// 1. Top 10 slowest API endpoints in the last 1 hour
requests
| where timestamp >= ago(1h)
| where success == false or duration > 2000
| summarize
    CallCount   = count(),
    AvgDuration = avg(duration),
    P95Duration = percentile(duration, 95)
  by name
| top 10 by P95Duration desc

// 2. Error rate by 5-minute bucket
requests
| where timestamp >= ago(6h)
| summarize
    Total  = count(),
    Failed = countif(success == false)
  by bin(timestamp, 5m)
| extend ErrorRate = round(100.0 * Failed / Total, 2)
| render timechart

// 3. CPU > 80% for any VM in last 30 min
Perf
| where TimeGenerated >= ago(30m)
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where InstanceName == "_Total"
| summarize AvgCPU = avg(CounterValue) by Computer, bin(TimeGenerated, 5m)
| where AvgCPU > 80
| order by TimeGenerated desc

// 4. Count exceptions by type in last 24 hours
exceptions
| where timestamp >= ago(24h)
| summarize ExceptionCount = count() by type, outerMessage
| order by ExceptionCount desc
| take 20
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is VNet Peering and when would you use a Private Endpoint?

**VNet Peering** connects two VNets so that resources in either VNet communicate using private IPs (low latency, no gateway overhead). Global peering works across regions.

**Private Endpoint** gives an Azure PaaS service (Storage, SQL, Key Vault, Cosmos DB) a private IP address inside your VNet — traffic never leaves the Microsoft backbone network.

```bash
# Create VNet peering (both directions required)
az network vnet peering create \
  --name hub-to-spoke \
  --resource-group rg-demo \
  --vnet-name vnet-hub \
  --remote-vnet vnet-spoke \
  --allow-vnet-access \
  --allow-forwarded-traffic

az network vnet peering create \
  --name spoke-to-hub \
  --resource-group rg-demo \
  --vnet-name vnet-spoke \
  --remote-vnet vnet-hub \
  --allow-vnet-access \
  --allow-forwarded-traffic

# Create a Private Endpoint for Azure SQL Database
az network private-endpoint create \
  --name pe-sql \
  --resource-group rg-demo \
  --vnet-name vnet-hub \
  --subnet snet-private \
  --private-connection-resource-id $(az sql server show \
      --name sql-myapp \
      --resource-group rg-demo \
      --query id -o tsv) \
  --group-id sqlServer \
  --connection-name sql-private-conn

# Configure private DNS for the private endpoint
az network private-dns zone create \
  --resource-group rg-demo \
  --name "privatelink.database.windows.net"

az network private-dns link vnet create \
  --resource-group rg-demo \
  --zone-name "privatelink.database.windows.net" \
  --name dns-link-hub \
  --virtual-network vnet-hub \
  --registration-enabled false
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a multi-stage CI/CD pipeline in Azure DevOps with environment gates?

```yaml
# azure-pipelines.yml — Build → Deploy to Dev → Deploy to Prod (with approval)
trigger:
  branches:
    include: [main]

variables:
  buildConfiguration: Release
  containerRegistry: acrprod2025.azurecr.io
  imageName: myapp

stages:

  - stage: Build
    displayName: Build & Push Image
    jobs:
      - job: Build
        pool:
          vmImage: ubuntu-latest
        steps:
          - task: Docker@2
            inputs:
              containerRegistry: acr-service-connection
              repository: $(imageName)
              command: buildAndPush
              tags: |
                $(Build.BuildId)
                latest

  - stage: DeployDev
    displayName: Deploy to Development
    dependsOn: Build
    jobs:
      - deployment: DeployDev
        environment: development            # environment defined in Azure DevOps
        pool:
          vmImage: ubuntu-latest
        strategy:
          runOnce:
            deploy:
              steps:
                - task: KubernetesManifest@1
                  inputs:
                    action: deploy
                    kubernetesServiceConnection: aks-dev-connection
                    namespace: dev
                    manifests: k8s/deployment.yaml
                    containers: $(containerRegistry)/$(imageName):$(Build.BuildId)

  - stage: DeployProd
    displayName: Deploy to Production
    dependsOn: DeployDev
    jobs:
      - deployment: DeployProd
        environment: production             # has manual approval gate configured
        pool:
          vmImage: ubuntu-latest
        strategy:
          runOnce:
            deploy:
              steps:
                - task: KubernetesManifest@1
                  inputs:
                    action: deploy
                    kubernetesServiceConnection: aks-prod-connection
                    namespace: production
                    manifests: k8s/deployment.yaml
                    containers: $(containerRegistry)/$(imageName):$(Build.BuildId)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is data redundancy in Azure Storage and how do LRS, ZRS, GRS differ?

| Option | Acronym | Copies | Survives | Cost |
|--------|---------|--------|---------|------|
| Locally Redundant | **LRS** | 3 in one datacenter | Single disk/rack failure | Lowest |
| Zone-Redundant | **ZRS** | 3 across AZs in one region | Single AZ failure | Medium |
| Geo-Redundant | **GRS** | 3 LRS + 3 LRS in paired region | Regional outage | Higher |
| Geo-Zone-Redundant | **GZRS** | 3 ZRS + 3 LRS in paired region | Regional + AZ failure | Highest |
| Read-Access GRS | **RA-GRS** | Same as GRS + read from secondary | Regional outage with read | Higher |

```bash
# Create storage account with ZRS (recommended for most production workloads)
az storage account create \
  --name stproddata2025 \
  --resource-group rg-demo \
  --location eastus \
  --sku Standard_ZRS \
  --kind StorageV2 \
  --min-tls-version TLS1_2 \
  --allow-blob-public-access false

# Upgrade to GRS for disaster recovery
az storage account update \
  --name stproddata2025 \
  --resource-group rg-demo \
  --sku Standard_GRS
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you design auto-scaling for an AKS cluster?

AKS supports two layers of auto-scaling:
- **HPA (Horizontal Pod Autoscaler)** — scales pod replicas based on CPU/memory or custom metrics.
- **Cluster Autoscaler** — adds or removes VMs (nodes) based on pending pod demand.

```bash
# Enable cluster autoscaler on node pool
az aks nodepool update \
  --cluster-name aks-demo \
  --resource-group rg-demo \
  --name nodepool1 \
  --enable-cluster-autoscaler \
  --min-count 2 \
  --max-count 10
```

```yaml
# HPA — scale pods when CPU > 60%
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 2
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 75
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you troubleshoot a CrashLoopBackOff error in AKS?

**CrashLoopBackOff** means a container starts, crashes, and Kubernetes retries indefinitely. Common causes: wrong environment variables, missing secrets, application startup exceptions, or misconfigured liveness probes.

```bash
# Step 1: Get pod status
kubectl get pods -n production

# Step 2: Read events and state
kubectl describe pod <pod-name> -n production
# Look for: Exit Code, OOMKilled, Liveness probe failed, Error from image pull

# Step 3: Read application logs from crashed container
kubectl logs <pod-name> -n production --previous

# Step 4: Check environment variables and secrets
kubectl get secret myapp-secret -n production -o jsonpath='{.data}' | \
  python3 -c "import sys,json,base64; [print(k,base64.b64decode(v).decode()) for k,v in json.load(sys.stdin).items()]"

# Step 5: Override entrypoint to debug interactively
kubectl debug pod/<pod-name> -n production \
  --image=busybox --target=myapp -- /bin/sh

# Step 6: Common fixes
# Missing env var → add to deployment.yaml
kubectl set env deployment/myapp DB_HOST=my-sql-server.database.windows.net -n production

# OOMKilled → increase memory limit
kubectl set resources deployment/myapp \
  --limits=memory=512Mi \
  --requests=memory=256Mi \
  -n production

# Image pull error → verify ACR attachment
az aks update --name aks-demo --resource-group rg-demo --attach-acr acrprod2025
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure autoscaling for Azure App Service and Virtual Machine Scale Sets?

**Autoscaling** automatically adjusts the number of instances based on demand, schedule, or custom metrics, ensuring performance while controlling cost.

**App Service autoscale** scales the number of instances of the App Service Plan:

```bash
# Enable autoscale on App Service Plan
az monitor autoscale create \
  --resource-group rg-prod \
  --resource $(az appservice plan show --name asp-webapp --resource-group rg-prod --query id -o tsv) \
  --resource-type Microsoft.Web/serverFarms \
  --name autoscale-webapp \
  --min-count 2 \
  --max-count 10 \
  --count 2

# Add scale-out rule: scale out when CPU > 70% for 5 minutes
az monitor autoscale rule create \
  --autoscale-name autoscale-webapp \
  --resource-group rg-prod \
  --scale out 2 \
  --condition "Percentage CPU > 70 avg 5m"

# Add scale-in rule: scale in when CPU < 30% for 5 minutes
az monitor autoscale rule create \
  --autoscale-name autoscale-webapp \
  --resource-group rg-prod \
  --scale in 1 \
  --condition "Percentage CPU < 30 avg 5m"
```

**VMSS autoscale** works the same way but targets VM instances:

```bash
# Create VMSS with autoscale
az vmss create \
  --name vmss-web --resource-group rg-prod \
  --image Ubuntu2204 --vm-sku Standard_D2s_v3 \
  --instance-count 2 --admin-username azureuser \
  --generate-ssh-keys --lb "" --public-ip-address ""

az monitor autoscale create \
  --resource-group rg-prod \
  --resource $(az vmss show --name vmss-web --resource-group rg-prod --query id -o tsv) \
  --resource-type Microsoft.Compute/virtualMachineScaleSets \
  --name vmss-autoscale --min-count 2 --max-count 20 --count 2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure CDN and Azure Front Door to improve global application performance?

**Azure CDN** caches static content at edge locations worldwide, reducing latency for geographically distributed users. **Azure Front Door** adds global load balancing, SSL offload, WAF, and health-based routing across multiple origins (active-active or active-passive).

```bash
# Create a CDN profile and endpoint
az cdn profile create \
  --name cdn-myapp \
  --resource-group rg-prod \
  --sku Standard_Microsoft

az cdn endpoint create \
  --name myapp-static \
  --profile-name cdn-myapp \
  --resource-group rg-prod \
  --origin stmyapp.blob.core.windows.net \
  --origin-host-header stmyapp.blob.core.windows.net

# Create Azure Front Door with two backends (active-active)
az afd profile create \
  --profile-name afd-myapp \
  --resource-group rg-prod \
  --sku Standard_AzureFrontDoor

az afd origin-group create \
  --profile-name afd-myapp \
  --resource-group rg-prod \
  --origin-group-name og-webapp \
  --probe-request-type GET \
  --probe-protocol Https \
  --probe-interval-in-seconds 30 \
  --sample-size 4 \
  --successful-samples-required 3

az afd origin create \
  --profile-name afd-myapp \
  --resource-group rg-prod \
  --origin-group-name og-webapp \
  --origin-name origin-eastus \
  --host-name myapp-eastus.azurewebsites.net \
  --origin-host-header myapp-eastus.azurewebsites.net \
  --http-port 80 --https-port 443 --priority 1 --weight 500
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure Cache for Redis to improve application performance?

**Azure Cache for Redis** is a fully managed, in-memory data store. Use it for:
- **Output caching** — cache rendered HTML or API responses
- **Session state** — distribute session across multiple app instances
- **Data caching** — reduce database load for frequently read data
- **Message broker** — pub/sub between application components

```csharp
// .NET 8: Using StackExchange.Redis with Azure Cache for Redis
using StackExchange.Redis;

// Connection (use Managed Identity or Key Vault for the connection string in production)
var redis = ConnectionMultiplexer.Connect(
    "myredis.redis.cache.windows.net:6380,password=<key>,ssl=True,abortConnect=False");

IDatabase db = redis.GetDatabase();

// Cache-aside pattern
async Task<Product> GetProductAsync(int id)
{
    string cacheKey = $"product:{id}";
    string? cached = await db.StringGetAsync(cacheKey);

    if (cached is not null)
        return JsonSerializer.Deserialize<Product>(cached)!;

    var product = await _dbContext.Products.FindAsync(id);
    await db.StringSetAsync(cacheKey, JsonSerializer.Serialize(product), TimeSpan.FromMinutes(15));
    return product!;
}
```

```bash
# Create Redis cache
az redis create \
  --name redis-myapp \
  --resource-group rg-prod \
  --location eastus \
  --sku Standard \
  --vm-size c1  # 1 GB

# Get connection string
az redis list-keys --name redis-myapp --resource-group rg-prod
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize Azure SQL Database performance using Query Performance Insight and Elastic Pools?

**Query Performance Insight (QPI)** identifies top CPU-consuming and long-running queries. **Elastic Pools** share a pool of DTUs/vCores across multiple databases, handling variable workloads cost-efficiently.

```bash
# Enable automatic tuning (creates indexes, removes unused indexes, force good plans)
az sql db update \
  --name mydb \
  --server sql-myapp \
  --resource-group rg-prod \
  --auto-pause-delay -1 \
  --set '{"properties":{"autoPauseDelay":-1}}'

# Query Performance Insight data via REST (or use portal)
az sql db query-store-list --resource-group rg-prod --server sql-myapp --name mydb

# Create Elastic Pool
az sql elastic-pool create \
  --name ep-shared \
  --server sql-myapp \
  --resource-group rg-prod \
  --edition GeneralPurpose \
  --family Gen5 \
  --capacity 4 \
  --max-size 32GB

# Move databases into the pool
az sql db update \
  --name mydb \
  --server sql-myapp \
  --resource-group rg-prod \
  --elastic-pool ep-shared
```

**Tuning tips:**
- Use **Automatic Tuning** to let Azure create/drop indexes automatically
- Use **Serverless tier** for intermittent workloads (auto-pause when idle)
- Check **sys.dm_exec_query_stats** for query plan analysis
- Enable **Intelligent Query Processing** (batch mode, adaptive joins) via compat level 150+

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you right-size Azure VMs and use Azure Advisor recommendations?

**Azure Advisor** analyzes your Azure usage telemetry and provides actionable recommendations across Cost, Performance, Reliability, Security, and Operational Excellence.

For VM right-sizing:
- Advisor identifies VMs with **average CPU < 5%** and recommends downsizing or shutdown
- Use **Azure Monitor VM Insights** to view CPU, memory, disk, and network usage over 30 days
- Use **Azure Compute Optimizer** for workload-aware sizing

```bash
# Get Advisor recommendations for a subscription
az advisor recommendation list \
  --category Cost \
  --output table

# Get specific VM right-sizing recommendations
az advisor recommendation list \
  --category Cost \
  --query "[?impactedField=='microsoft.compute/virtualmachines']" \
  --output json

# Resize a VM based on Advisor recommendation
az vm deallocate --name vm-oversized --resource-group rg-prod
az vm resize \
  --name vm-oversized \
  --resource-group rg-prod \
  --size Standard_B2s  # Downsized from D4s_v3
az vm start --name vm-oversized --resource-group rg-prod

# Schedule auto-shutdown for dev/test VMs to reduce cost
az vm auto-shutdown \
  --name vm-dev \
  --resource-group rg-dev \
  --time 1900  # 7 PM UTC
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. TROUBLESHOOTING

<br>

## Q. How do you use Azure Network Watcher to troubleshoot VM connectivity issues?

**Azure Network Watcher** provides network monitoring and diagnostic tools for Azure IaaS resources. Key tools:

| Tool | Purpose |
|------|---------|
| **Connection troubleshoot** | Tests connectivity between source/destination (TCP/ICMP), shows latency and hops |
| **NSG flow logs** | Logs all IP traffic through NSGs (allowed/denied, src/dst IP, port) to a storage account |
| **Packet capture** | Captures VM network traffic remotely without logging into the VM |
| **IP flow verify** | Checks if a specific packet would be allowed or denied by NSG rules |
| **Next hop** | Identifies the next routing hop from a VM to a destination |

```bash
# Enable Network Watcher in a region
az network watcher configure \
  --locations eastus \
  --enabled true \
  --resource-group NetworkWatcherRG

# Test connectivity from VM to a destination (port 443)
az network watcher test-connectivity \
  --source-resource $(az vm show --name vm-web --resource-group rg-prod --query id -o tsv) \
  --dest-address google.com \
  --dest-port 443

# Enable NSG flow logs
az network watcher flow-log create \
  --name nsg-flow-log \
  --resource-group rg-prod \
  --nsg nsg-web \
  --storage-account $(az storage account show --name stlogs --resource-group rg-prod --query id -o tsv) \
  --location eastus \
  --retention 30 \
  --log-version 2 \
  --traffic-analytics true \
  --workspace $(az monitor log-analytics workspace show --workspace-name la-myapp --resource-group rg-prod --query id -o tsv)

# IP flow verify: check if port 3389 (RDP) is allowed to a VM
az network watcher test-ip-flow \
  --direction Inbound \
  --local "10.0.0.4:*" \
  --protocol TCP \
  --remote "203.0.113.10:3389" \
  --vm vm-web \
  --resource-group rg-prod
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you diagnose and fix slow performance in an Azure App Service?

**Diagnostic steps for a slow App Service:**

1. **App Service Diagnose & Solve Problems** (portal) — built-in guided diagnostics for CPU, memory, crashes, HTTP errors.
2. **Application Insights** — distributed traces, dependency calls, slow request waterfall.
3. **Log Stream** — real-time stdout/stderr logs.
4. **Kudu console** — process explorer, memory dumps, file system.
5. **Azure Monitor Metrics** — HTTP 5xx rate, response time, requests per second.

```bash
# Enable Application Insights for App Service
az monitor app-insights component create \
  --app ai-myapp \
  --location eastus \
  --resource-group rg-prod \
  --application-type web

az webapp config appsettings set \
  --name app-mywebapp \
  --resource-group rg-prod \
  --settings \
    APPINSIGHTS_INSTRUMENTATIONKEY=$(az monitor app-insights component show --app ai-myapp --resource-group rg-prod --query instrumentationKey -o tsv) \
    APPLICATIONINSIGHTS_CONNECTION_STRING=$(az monitor app-insights component show --app ai-myapp --resource-group rg-prod --query connectionString -o tsv) \
    ApplicationInsightsAgent_EXTENSION_VERSION=~3

# Query slow requests in Log Analytics (KQL)
# requests
# | where duration > 2000  // requests slower than 2 seconds
# | summarize count(), avg(duration) by name, url
# | order by avg_duration desc
# | take 20

# Enable diagnostic logs
az webapp log config \
  --name app-mywebapp \
  --resource-group rg-prod \
  --application-logging filesystem \
  --level verbose \
  --web-server-logging filesystem

# Stream logs in real-time
az webapp log tail --name app-mywebapp --resource-group rg-prod
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you troubleshoot an Azure Kubernetes Service (AKS) pod that is failing to start?

**Pod failure diagnostics follow this order:**

1. `kubectl get pods` — check pod status (Pending, CrashLoopBackOff, OOMKilled, ImagePullBackOff)
2. `kubectl describe pod <pod>` — events section shows scheduling failures, image pull errors, resource constraints
3. `kubectl logs <pod>` — application logs (add `--previous` for crashed container logs)
4. Check node resources: `kubectl describe node <node>`
5. Check AKS cluster health in Azure Monitor / Container Insights

```bash
# Get pod status
kubectl get pods -n production -o wide

# Describe pod to see events
kubectl describe pod myapp-7d6b9c8f4-xk2j9 -n production

# Get logs from a crashing container
kubectl logs myapp-7d6b9c8f4-xk2j9 -n production --previous

# Common issues and fixes:

# 1. ImagePullBackOff: Wrong image name or missing ACR credentials
az aks update --name aks-prod --resource-group rg-prod \
  --attach-acr $(az acr show --name acrprod --resource-group rg-prod --query id -o tsv)

# 2. Pending (insufficient resources): Check node capacity
kubectl get nodes
kubectl describe node aks-nodepool1-12345678-vmss000000

# Scale node pool
az aks nodepool scale \
  --cluster-name aks-prod \
  --resource-group rg-prod \
  --name nodepool1 \
  --node-count 5

# 3. CrashLoopBackOff: App crashes repeatedly. Fix: check env vars, secrets, config
kubectl get secret myapp-secret -n production -o jsonpath='{.data}' | base64 -d

# 4. OOMKilled: Container exceeds memory limit
kubectl set resources deployment myapp \
  --limits=memory=512Mi,cpu=500m \
  --requests=memory=256Mi,cpu=250m \
  -n production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you investigate and resolve Azure VM connectivity issues (RDP/SSH)?

**Common VM access issues and resolution steps:**

```bash
# 1. Check VM is running
az vm show --name vm-prod --resource-group rg-prod --query powerState

# 2. Check NSG rules - ensure port 22/3389 is allowed
az network nsg rule list \
  --nsg-name nsg-vm \
  --resource-group rg-prod \
  --output table

# Add SSH rule if missing
az network nsg rule create \
  --nsg-name nsg-vm \
  --resource-group rg-prod \
  --name Allow-SSH \
  --priority 100 \
  --direction Inbound \
  --protocol TCP \
  --destination-port-range 22 \
  --source-address-prefixes "MY.IP.ADDRESS/32" \
  --access Allow

# 3. Use Azure Bastion for secure browser-based access (no public IP needed)
az network bastion create \
  --name bastion-prod \
  --resource-group rg-prod \
  --vnet-name vnet-prod \
  --public-ip-address pip-bastion \
  --location eastus \
  --sku Standard

# 4. Reset SSH config or Windows admin password via Azure VM Extension
az vm user reset-ssh \
  --name vm-linux \
  --resource-group rg-prod

# Reset Windows admin password
az vm user update \
  --name vm-windows \
  --resource-group rg-prod \
  --username azureuser \
  --password "NewP@ssword123!"

# 5. Run command inside VM without SSH (Azure Run Command)
az vm run-command invoke \
  --name vm-linux \
  --resource-group rg-prod \
  --command-id RunShellScript \
  --scripts "systemctl status sshd" "journalctl -u sshd --no-pager -n 50"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you troubleshoot failed deployments in Azure using Activity Logs and Deployment history?

**Azure Activity Log** records all control-plane operations (create, update, delete) on Azure resources. Deployment history tracks ARM/Bicep deployment results.

```bash
# List recent Activity Log entries for a resource group
az monitor activity-log list \
  --resource-group rg-prod \
  --start-time "2025-01-01T00:00:00Z" \
  --status Failed \
  --query "[].{Time:eventTimestamp,Operation:operationName.value,Status:status.value,Message:properties.statusMessage}" \
  --output table

# Get deployment history for a resource group
az deployment group list \
  --resource-group rg-prod \
  --query "[].{Name:name,State:properties.provisioningState,Time:properties.timestamp}" \
  --output table

# Get detailed error for a failed deployment
az deployment group show \
  --resource-group rg-prod \
  --name "mainDeployment-20250101" \
  --query "properties.error"

# Get operation-level details (which resource failed and why)
az deployment operation group list \
  --resource-group rg-prod \
  --name "mainDeployment-20250101" \
  --query "[?properties.provisioningState=='Failed'].{Resource:properties.targetResource.resourceName,Error:properties.statusMessage}" \
  --output json
```

**KQL query in Log Analytics for failed ARM operations:**
```kql
AzureActivity
| where TimeGenerated > ago(24h)
| where ActivityStatusValue == "Failure"
| project TimeGenerated, OperationNameValue, ResourceGroup, Caller, Properties
| order by TimeGenerated desc
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. APPLICATION DEVELOPMENT

<br>

## Q. How do you use Azure App Service deployment slots for zero-downtime blue-green deployments?

**Deployment slots** are live environments within an App Service app (Staging, QA, Prod). Swap operations exchange the slot configurations while the app processes existing requests — achieving zero-downtime deployments with instant rollback.

**Swap process:**
1. Deploy new version to **staging** slot (warm up)
2. **Swap** staging → production (traffic switches instantly)
3. Staging slot now holds the previous production version (rollback by swapping back)

```bash
# Create staging slot
az webapp deployment slot create \
  --name app-mywebapp \
  --resource-group rg-prod \
  --slot staging

# Deploy to staging (CI/CD deploys here first)
az webapp deployment source config-zip \
  --name app-mywebapp \
  --resource-group rg-prod \
  --slot staging \
  --src ./app.zip

# Validate staging (run smoke tests against staging URL)
# https://app-mywebapp-staging.azurewebsites.net

# Swap staging to production
az webapp deployment slot swap \
  --name app-mywebapp \
  --resource-group rg-prod \
  --slot staging \
  --target-slot production

# Rollback: swap back
az webapp deployment slot swap \
  --name app-mywebapp \
  --resource-group rg-prod \
  --slot production \
  --target-slot staging
```

**Slot-sticky settings:** Mark settings as "slot setting" so they don\'t swap with the app (connection strings, app keys specific to each environment).

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/app-service/media/web-sites-staged-publishing/swap-configure-source-target-slots.png" alt="Azure App Service Deployment Slots Swap" width="700px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create an Azure Function with HTTP and Timer triggers using the isolated worker model?

**Azure Functions** (v4, .NET 8 isolated worker) runs in a separate process from the Functions runtime, giving full control over dependency injection, middleware, and .NET version.

```csharp
// Program.cs (Isolated worker host setup)
using Microsoft.Azure.Functions.Worker;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

var host = new HostBuilder()
    .ConfigureFunctionsWebApplication()
    .ConfigureServices(services =>
    {
        services.AddApplicationInsightsTelemetryWorkerService();
        services.ConfigureFunctionsApplicationInsights();
        services.AddSingleton<IMyService, MyService>();
    })
    .Build();

await host.RunAsync();

// HTTP Trigger function
using Microsoft.Azure.Functions.Worker;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;

public class OrderFunctions(IMyService svc)
{
    [Function("ProcessOrder")]
    public async Task<IActionResult> HttpTrigger(
        [HttpTrigger(AuthorizationLevel.Function, "post", Route = "orders")] HttpRequest req)
    {
        var order = await req.ReadFromJsonAsync<Order>();
        var result = await svc.ProcessOrderAsync(order!);
        return new OkObjectResult(result);
    }
}

// Timer Trigger function (runs at midnight UTC daily)
public class CleanupFunctions
{
    [Function("DailyCleanup")]
    public async Task TimerTrigger(
        [TimerTrigger("0 0 0 * * *")] TimerInfo timer,
        FunctionContext context)
    {
        var logger = context.GetLogger<CleanupFunctions>();
        logger.LogInformation("Cleanup triggered at: {time}", DateTime.UtcNow);
        // cleanup logic
    }
}
```

```bash
# Deploy to Azure
az functionapp create \
  --name func-myapp \
  --resource-group rg-prod \
  --storage-account stfunctions \
  --runtime dotnet-isolated \
  --runtime-version 8 \
  --functions-version 4 \
  --consumption-plan-location eastus

func azure functionapp publish func-myapp
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure Logic Apps to automate workflows between services?

**Azure Logic Apps** is a serverless workflow engine that automates processes using a visual designer. It has 400+ connectors for SaaS, on-premises, and Azure services — no code required for simple integrations.

**Common use cases:**
- Order processing (HTTP → validate → SQL insert → send email)
- File processing (Blob trigger → OCR → Cosmos DB write)
- Approval workflows (Form submission → Teams approval → SharePoint write)

```json
{
  "definition": {
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "triggers": {
      "When_a_blob_is_added": {
        "type": "ApiConnection",
        "inputs": {
          "host": { "connection": { "name": "@parameters('$connections')['azureblob']['connectionId']" } },
          "method": "get",
          "path": "/datasets/default/triggers/batch/onupdatedfile",
          "queries": { "folderId": "JTJmdXBsb2Fkcw==", "maxFileCount": 10 }
        },
        "recurrence": { "frequency": "Minute", "interval": 5 }
      }
    },
    "actions": {
      "Send_approval_email": {
        "type": "ApiConnection",
        "inputs": {
          "host": { "connection": { "name": "@parameters('$connections')['office365']['connectionId']" } },
          "method": "post",
          "path": "/approvalmail/$subscriptions",
          "body": {
            "To": "manager@contoso.com",
            "Subject": "New file uploaded: @{triggerBody()?['Name']}",
            "Options": "Approve, Reject"
          }
        }
      }
    }
  }
}
```

```bash
# Create Logic App
az logic workflow create \
  --resource-group rg-prod \
  --location eastus \
  --name la-order-processing \
  --definition @workflow-definition.json
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement event-driven architecture with Azure Event Grid, Service Bus, and Event Hubs?

| Service | Pattern | Guaranteed delivery | Scale | Use case |
|---------|---------|--------------------|----|---------|
| **Event Grid** | Push (reactive) | Yes (retry policy) | Millions/sec | React to Azure resource events, webhooks |
| **Service Bus** | Pull (queues/topics) | Yes (FIFO, sessions, DLQ) | Thousands/sec | Order processing, reliable messaging |
| **Event Hubs** | Pull (partitioned log) | Yes (retention) | Millions/sec | IoT telemetry, log aggregation, streaming |

```bash
# Event Grid: Subscribe to Blob Storage events (file uploaded)
az eventgrid event-subscription create \
  --name sub-blob-uploads \
  --source-resource-id $(az storage account show --name stdata --resource-group rg-prod --query id -o tsv) \
  --endpoint https://func-myapp.azurewebsites.net/api/ProcessBlob \
  --endpoint-type webhook \
  --included-event-types Microsoft.Storage.BlobCreated

# Service Bus: Create topic + subscription
az servicebus namespace create \
  --name sb-myapp \
  --resource-group rg-prod \
  --sku Standard

az servicebus topic create \
  --namespace-name sb-myapp \
  --resource-group rg-prod \
  --name orders

az servicebus topic subscription create \
  --namespace-name sb-myapp \
  --resource-group rg-prod \
  --topic-name orders \
  --name inventory-service

# Event Hubs: Create for telemetry ingestion
az eventhubs namespace create \
  --name ehns-telemetry \
  --resource-group rg-prod \
  --sku Standard

az eventhubs eventhub create \
  --namespace-name ehns-telemetry \
  --resource-group rg-prod \
  --name device-events \
  --partition-count 4 \
  --message-retention 3
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy and manage containerized applications on Azure Kubernetes Service (AKS)?

```bash
# Create AKS cluster with system-assigned identity and ACR integration
az aks create \
  --name aks-prod \
  --resource-group rg-prod \
  --node-count 3 \
  --node-vm-size Standard_D4s_v3 \
  --network-plugin azure \
  --enable-managed-identity \
  --attach-acr acrprod \
  --enable-cluster-autoscaler \
  --min-count 2 --max-count 10 \
  --generate-ssh-keys

# Connect kubectl
az aks get-credentials --name aks-prod --resource-group rg-prod

# Deploy application
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: acrprod.azurecr.io/myapp:v2.1.0
        resources:
          requests:
            cpu: "250m"
            memory: "256Mi"
          limits:
            cpu: "500m"
            memory: "512Mi"
        env:
        - name: CONNECTION_STRING
          valueFrom:
            secretKeyRef:
              name: myapp-secrets
              key: connectionString
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-svc
  namespace: production
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 8080
EOF

# Rolling update to new image
kubectl set image deployment/myapp myapp=acrprod.azurecr.io/myapp:v2.2.0 -n production
kubectl rollout status deployment/myapp -n production

# Rollback if issues
kubectl rollout undo deployment/myapp -n production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Azure Durable Functions and how do you implement stateful workflows?

**Azure Durable Functions** extends Azure Functions with state, orchestration, and long-running workflow capabilities using the virtual actor pattern. Execution state is automatically checkpointed to Azure Storage — no manual state management needed.

**Durable Function patterns:**

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Function Chaining** | Sequential activity calls with output passed forward | Multi-step ETL |
| **Fan-Out / Fan-In** | Parallel activities collected into one result | Batch processing |
| **Async HTTP API** | Long-running operation with status polling endpoint | File processing, AI inference |
| **Monitor** | Recurring polling until condition met | Wait for external event |
| **Human Interaction** | Pause workflow awaiting human approval with timeout | Expense approval |

**Example — Fan-Out/Fan-In: process orders in parallel:**

```csharp
// Orchestrator (runs as a replay-safe coroutine — no side effects!)
[Function("ProcessOrdersBatch")]
public static async Task<List<OrderResult>> RunOrchestrator(
    [OrchestrationTrigger] TaskOrchestrationContext context)
{
    // Step 1: Get batch of pending order IDs (activity call)
    var orderIds = await context.CallActivityAsync<List<string>>("GetPendingOrders", null);

    // Step 2: Fan-out — process all orders in parallel
    var tasks = orderIds.Select(id =>
        context.CallActivityAsync<OrderResult>("ProcessSingleOrder", id));

    var results = await Task.WhenAll(tasks);   // Fan-in

    // Step 3: Send summary notification
    await context.CallActivityAsync("SendBatchSummary", results.Length);

    return results.ToList();
}

// Activity — runs once, can have side effects
[Function("ProcessSingleOrder")]
public static async Task<OrderResult> ProcessOrder(
    [ActivityTrigger] string orderId,
    FunctionContext context)
{
    var logger = context.GetLogger("ProcessSingleOrder");
    logger.LogInformation("Processing order {OrderId}", orderId);

    // Call database, external API, etc.
    return new OrderResult { OrderId = orderId, Status = "Completed" };
}

// HTTP starter — triggers the orchestration
[Function("HttpStart")]
public static async Task<HttpResponseData> HttpStart(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post", Route = "orders/process")] HttpRequestData req,
    [DurableClient] DurableTaskClient client)
{
    string instanceId = await client.ScheduleNewOrchestrationInstanceAsync("ProcessOrdersBatch");
    return await client.CreateCheckStatusResponseAsync(req, instanceId);
}
```

**Human interaction pattern (approval workflow):**

```csharp
[Function("ApprovalWorkflow")]
public static async Task RunApproval(
    [OrchestrationTrigger] TaskOrchestrationContext context)
{
    var request = context.GetInput<ExpenseRequest>();

    // Send approval email (activity)
    await context.CallActivityAsync("SendApprovalEmail", request);

    // Wait up to 48 hours for external approval event
    using var timeoutCts = new CancellationTokenSource();
    var approvalTask = context.WaitForExternalEvent<bool>("ApprovalReceived");
    var timeoutTask  = context.CreateTimer(context.CurrentUtcDateTime.AddHours(48), timeoutCts.Token);

    var winner = await Task.WhenAny(approvalTask, timeoutTask);

    if (winner == approvalTask)
    {
        bool approved = await approvalTask;
        await context.CallActivityAsync(approved ? "ApproveExpense" : "RejectExpense", request);
    }
    else
    {
        await context.CallActivityAsync("EscalateExpense", request);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Logic Apps Standard and Consumption tier?

| Feature | **Consumption** | **Standard** |
|---------|-----------------|--------------|
| Hosting | Multi-tenant (shared) | Single-tenant (dedicated App Service Plan or ASE) |
| Pricing | Per-action execution | Fixed plan + per vCore/hour |
| VNet integration | Not supported (connectors are public) | Full VNet injection |
| Deployment | Portal / ARM only | CI/CD via VS Code, GitHub Actions, Bicep |
| State storage | Azure Storage (managed) | Azure Storage (configurable) |
| Stateful workflows | Yes | Yes |
| Stateless workflows | No | Yes (faster, no state persistence) |
| Built-in connectors | 400+ managed | Built-in (in-process) + managed |
| Multiple workflows | One workflow per resource | Multiple workflows per resource |
| Best for | Simple integrations, low volume | Enterprise, private network, high volume |

```bash
# Create Logic Apps Standard (single-tenant)
az logicapp create \
  --name la-myapp-std \
  --resource-group rg-integration \
  --plan asp-la-plan \
  --storage-account stlamyapp \
  --location eastus

# Create Logic Apps Consumption (multi-tenant) via ARM
az deployment group create \
  --resource-group rg-integration \
  --template-file logicapp-consumption.json \
  --parameters workflowName=la-myapp-consumption
```

**Error handling and retry policies in Logic Apps:**

```json
{
  "actions": {
    "CallExternalAPI": {
      "type": "Http",
      "inputs": {
        "method": "POST",
        "uri": "https://api.partner.com/orders",
        "body": "@triggerBody()"
      },
      "retryPolicy": {
        "type": "exponential",
        "count": 4,
        "interval": "PT10S",
        "minimumInterval": "PT10S",
        "maximumInterval": "PT1H"
      },
      "runAfter": {}
    },
    "HandleError": {
      "type": "Compose",
      "inputs": "@{actions('CallExternalAPI').error.message}",
      "runAfter": {
        "CallExternalAPI": ["Failed", "TimedOut"]
      }
    }
  }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 14. DEVOPS

<br>

## Q. How do you create a multi-stage Azure DevOps YAML pipeline for a .NET application?

A **multi-stage YAML pipeline** in Azure DevOps separates CI (build + test) from CD (deploy to multiple environments) in a single file, with environment approvals for gating.

```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
      - main
      - release/*

variables:
  buildConfiguration: Release
  dotnetVersion: '8.x'

stages:
  - stage: Build
    displayName: Build & Test
    jobs:
      - job: BuildJob
        pool:
          vmImage: ubuntu-latest
        steps:
          - task: UseDotNet@2
            inputs:
              version: $(dotnetVersion)

          - script: dotnet restore
            displayName: Restore packages

          - script: dotnet build --configuration $(buildConfiguration) --no-restore
            displayName: Build

          - script: dotnet test --no-build --configuration $(buildConfiguration) --logger trx --results-directory $(Agent.TempDirectory)
            displayName: Run tests

          - task: PublishTestResults@2
            inputs:
              testResultsFormat: VSTest
              testResultsFiles: '**/*.trx'
              testRunTitle: Unit Tests

          - task: DotNetCoreCLI@2
            displayName: Publish
            inputs:
              command: publish
              arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'

          - publish: $(Build.ArtifactStagingDirectory)
            artifact: drop

  - stage: DeployStaging
    displayName: Deploy to Staging
    dependsOn: Build
    condition: succeeded()
    jobs:
      - deployment: DeployStaging
        environment: Staging
        pool:
          vmImage: ubuntu-latest
        strategy:
          runOnce:
            deploy:
              steps:
                - task: AzureWebApp@1
                  inputs:
                    azureSubscription: 'azure-service-connection'
                    appName: 'app-mywebapp'
                    slotName: staging
                    package: $(Pipeline.Workspace)/drop/*.zip

  - stage: DeployProduction
    displayName: Deploy to Production
    dependsOn: DeployStaging
    condition: succeeded()
    jobs:
      - deployment: DeployProd
        environment: Production    # Environment has approval gate configured
        pool:
          vmImage: ubuntu-latest
        strategy:
          runOnce:
            deploy:
              steps:
                - task: AzureAppServiceManage@0
                  displayName: Swap slots
                  inputs:
                    azureSubscription: 'azure-service-connection'
                    Action: Swap Slots
                    WebAppName: app-mywebapp
                    ResourceGroupName: rg-prod
                    SourceSlot: staging
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use GitHub Actions to deploy an Azure App Service?

```yaml
# .github/workflows/deploy.yml
name: Deploy to Azure App Service

on:
  push:
    branches: [main]

env:
  AZURE_WEBAPP_NAME: app-mywebapp
  DOTNET_VERSION: '8.x'

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: ${{ env.DOTNET_VERSION }}

      - name: Build and publish
        run: |
          dotnet restore
          dotnet build --configuration Release --no-restore
          dotnet test --no-build --configuration Release
          dotnet publish --configuration Release --output ./publish

      - name: Login to Azure
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}

      - name: Deploy to staging slot
        uses: azure/webapps-deploy@v3
        with:
          app-name: ${{ env.AZURE_WEBAPP_NAME }}
          slot-name: staging
          package: ./publish

      - name: Swap staging to production
        uses: azure/CLI@v2
        with:
          inlineScript: |
            az webapp deployment slot swap \
              --name ${{ env.AZURE_WEBAPP_NAME }} \
              --resource-group rg-prod \
              --slot staging \
              --target-slot production
```

**Authentication:** Use **federated credentials (OIDC)** with a service principal — no client secret stored in GitHub secrets. Configure via `az ad app federated-credential create`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement Infrastructure as Code with Bicep in a DevOps pipeline?

**Bicep** is Microsoft\'s first-class IaC DSL that compiles to ARM JSON. Key features: concise syntax, type safety, modules, and native Azure toolchain integration.

```bicep
// main.bicep — deploys App Service + SQL Database
param location string = resourceGroup().location
param environment string = 'prod'
param sqlAdminPassword string

@description('App Service Plan')
resource appServicePlan 'Microsoft.Web/serverfarms@2023-12-01' = {
  name: 'asp-${environment}'
  location: location
  sku: { name: 'P1v3', tier: 'PremiumV3', capacity: 2 }
  kind: 'linux'
  properties: { reserved: true }
}

resource webApp 'Microsoft.Web/sites@2023-12-01' = {
  name: 'app-myapp-${environment}'
  location: location
  properties: {
    serverFarmId: appServicePlan.id
    siteConfig: { linuxFxVersion: 'DOTNETCORE|8.0' }
  }
  identity: { type: 'SystemAssigned' }
}

resource sqlServer 'Microsoft.Sql/servers@2023-08-01-preview' = {
  name: 'sql-myapp-${environment}'
  location: location
  properties: {
    administratorLogin: 'sqladmin'
    administratorLoginPassword: sqlAdminPassword
    minimalTlsVersion: '1.2'
  }
}

output webAppName string = webApp.name
output webAppPrincipalId string = webApp.identity.principalId
```

```yaml
# In Azure DevOps pipeline:
- task: AzureCLI@2
  displayName: Deploy Bicep
  inputs:
    azureSubscription: 'azure-service-connection'
    scriptType: bash
    scriptLocation: inlineScript
    inlineScript: |
      az deployment group create \
        --resource-group rg-$(environment) \
        --template-file infra/main.bicep \
        --parameters environment=$(environment) \
                     sqlAdminPassword=$(sqlAdminPassword) \
        --what-if  # Preview changes before deploying
      
      az deployment group create \
        --resource-group rg-$(environment) \
        --template-file infra/main.bicep \
        --parameters environment=$(environment) \
                     sqlAdminPassword=$(sqlAdminPassword)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Container Registry and how does it integrate with AKS in a DevOps pipeline?

**Azure Container Registry (ACR)** is a managed, private Docker registry. It integrates with AKS via the `--attach-acr` flag, enabling AKS to pull images without credentials using managed identity.

```bash
# Create ACR with geo-replication
az acr create \
  --name acrprod \
  --resource-group rg-prod \
  --sku Premium \
  --location eastus

# Enable geo-replication for HA
az acr replication create \
  --name westus \
  --registry acrprod \
  --location westus

# Build and push image directly in ACR (no local Docker needed)
az acr build \
  --registry acrprod \
  --image myapp:$(git rev-parse --short HEAD) \
  --file Dockerfile .

# In CI/CD pipeline (GitHub Actions):
```

```yaml
- name: Build and push to ACR
  uses: azure/docker-login@v2
  with:
    login-server: acrprod.azurecr.io
    username: ${{ secrets.ACR_USERNAME }}
    password: ${{ secrets.ACR_PASSWORD }}

- run: |
    docker build -t acrprod.azurecr.io/myapp:${{ github.sha }} .
    docker push acrprod.azurecr.io/myapp:${{ github.sha }}

- name: Deploy to AKS
  uses: azure/k8s-deploy@v5
  with:
    action: deploy
    manifests: k8s/deployment.yaml
    images: acrprod.azurecr.io/myapp:${{ github.sha }}
    namespace: production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement secrets management in Azure DevOps pipelines using Azure Key Vault?

```yaml
# Reference Key Vault secrets directly in Azure DevOps pipeline
# First: create a Variable Group linked to Key Vault in Azure DevOps Library

variables:
  - group: kv-myapp-secrets  # Variable group linked to Key Vault

steps:
  - task: AzureKeyVault@2
    displayName: Get secrets from Key Vault
    inputs:
      azureSubscription: 'azure-service-connection'
      KeyVaultName: kv-myapp
      SecretsFilter: 'ConnectionString,ApiKey,SqlPassword'
      RunAsPreJob: true

  # Secret values are now available as pipeline variables (masked in logs)
  - script: |
      echo "Deploying with masked secrets..."
      # $(ConnectionString) is available but masked in logs
    displayName: Use secrets
    env:
      CONNECTION_STRING: $(ConnectionString)
```

```bash
# Grant the service principal used by the DevOps service connection access to Key Vault
$spObjectId = az ad sp show --id "http://azure-service-connection" --query id -o tsv

az keyvault set-policy \
  --name kv-myapp \
  --object-id $spObjectId \
  --secret-permissions get list

# Using OIDC (federated identity) — preferred: no long-lived secrets
az ad app federated-credential create \
  --id $(az ad app show --id "<app-id>" --query appId -o tsv) \
  --parameters '{
    "name": "github-actions-federated",
    "issuer": "https://token.actions.githubusercontent.com",
    "subject": "repo:myorg/myrepo:ref:refs/heads/main",
    "audiences": ["api://AzureADTokenExchange"]
  }'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is DevSecOps and how do you implement it in Azure DevOps pipelines?

**DevSecOps** integrates security practices continuously throughout the entire CI/CD pipeline — shifting security left so vulnerabilities are found at development time rather than in production.

**DevSecOps pipeline stages and tooling:**

```
Code → SAST → Build → Container Scan → Deploy → DAST → Production
  ↑                                                         ↓
  └──────────── Continuous Monitoring (Sentinel/Defender) ──┘
```

| Stage | Activity | Azure / OSS Tool |
|-------|----------|-----------------|
| **Pre-commit** | Secret detection | git-secrets, Gitleaks, Microsoft GHAS |
| **PR / CI** | Static analysis (SAST) | Microsoft Security DevOps (MSDO), SonarQube, Checkmarx |
| **CI** | Dependency scanning (SCA) | OWASP Dependency-Check, Snyk, Dependabot |
| **CI** | IaC scanning | Checkov, tfsec, Terrascan |
| **CI** | Container image scanning | Trivy, Grype, Defender for Containers |
| **CD** | Dynamic analysis (DAST) | OWASP ZAP, Burp Suite Enterprise |
| **Production** | Runtime protection | Microsoft Defender for Cloud, Sentinel |

**Azure DevOps pipeline with Microsoft Security DevOps (MSDO):**

```yaml
# azure-pipelines.yml — DevSecOps pipeline
trigger:
  - main

pool:
  vmImage: ubuntu-latest

variables:
  imageTag: $(Build.BuildId)
  acrName: myappacr

stages:
  - stage: SecurityScan
    displayName: Security Scanning
    jobs:
      - job: SAST
        steps:
          # Microsoft Security DevOps — runs AntiMalware, Binskim, Checkov, ESLint, TemplateAnalyzer, Terrascan, Trivy
          - task: MicrosoftSecurityDevOps@1
            displayName: MSDO Security Scan
            inputs:
              categories: 'IaC,code,containers'
              break: true   # fail pipeline on high-severity findings

          # OWASP Dependency-Check for third-party vulnerabilities
          - task: dependency-check-build-task@6
            displayName: OWASP Dependency Check
            inputs:
              projectName: myapp
              scanPath: '$(Build.SourcesDirectory)'
              format: 'HTML,JUNIT'
              failOnCVSS: 7   # fail on CVSS >= 7.0

  - stage: Build
    dependsOn: SecurityScan
    jobs:
      - job: BuildAndPush
        steps:
          - task: AzureCLI@2
            displayName: Build and push container image
            inputs:
              azureSubscription: azure-service-connection
              scriptType: bash
              scriptLocation: inlineScript
              inlineScript: |
                az acr build \
                  --registry $(acrName) \
                  --image myapp:$(imageTag) \
                  --file Dockerfile .

          # Scan the built image before promoting
          - script: |
              trivy image $(acrName).azurecr.io/myapp:$(imageTag) \
                --severity HIGH,CRITICAL \
                --exit-code 1 \
                --format sarif \
                --output trivy-results.sarif
            displayName: Trivy Container Scan

          - task: PublishBuildArtifacts@1
            inputs:
              pathToPublish: trivy-results.sarif
              artifactName: SecurityResults

  - stage: Deploy
    dependsOn: Build
    jobs:
      - deployment: DeployToStaging
        environment: staging
        strategy:
          runOnce:
            deploy:
              steps:
                - task: AzureWebAppContainer@1
                  inputs:
                    azureSubscription: azure-service-connection
                    appName: myapp-staging
                    containers: $(acrName).azurecr.io/myapp:$(imageTag)
```

**Enable Microsoft Defender for DevOps in Azure DevOps:**

```bash
# Connect Azure DevOps org to Defender for Cloud
az security devops azure-dev-ops-org create \
  --name myorg \
  --resource-group rg-security \
  --location eastus
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement blue-green and canary deployment strategies using Azure DevOps?

**Deployment strategies** reduce risk by gradually routing traffic to new versions before full rollout.

| Strategy | Mechanism | Risk | Rollback |
|----------|-----------|------|---------|
| **Blue-Green** | Two identical environments; switch traffic 100% | Low — instant rollback | Delete/redeploy blue slot |
| **Canary** | Route a % of traffic to new version; increase incrementally | Very low | Reduce canary weight to 0 |
| **Rolling** | Replace instances one batch at a time | Medium | Redeploy previous image |
| **Feature Flags** | Release code dark; toggle features per user/cohort | Very low | Toggle flag off |

**Blue-Green with App Service deployment slots:**

```yaml
# azure-pipelines.yml
- stage: Deploy
  jobs:
    - deployment: DeployToSlot
      environment: production
      strategy:
        runOnce:
          deploy:
            steps:
              # Deploy to staging slot (blue)
              - task: AzureWebApp@1
                displayName: Deploy to staging slot
                inputs:
                  azureSubscription: azure-service-connection
                  appName: myapp-prod
                  deployToSlotOrASE: true
                  resourceGroupName: rg-prod
                  slotName: staging
                  package: $(Pipeline.Workspace)/drop/*.zip

              # Smoke test the staging slot
              - script: |
                  RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" https://myapp-prod-staging.azurewebsites.net/health)
                  [ "$RESPONSE" == "200" ] || exit 1
                displayName: Smoke test staging slot

              # Swap slots — zero-downtime promotion to production
              - task: AzureAppServiceManage@0
                displayName: Swap slots
                inputs:
                  azureSubscription: azure-service-connection
                  Action: Swap Slots
                  WebAppName: myapp-prod
                  ResourceGroupName: rg-prod
                  SourceSlot: staging
                  SwapWithProduction: true
```

**Canary deployment with Azure Container Apps (traffic split):**

```bash
# Deploy new revision at 10% traffic
az containerapp revision set-mode \
  --name myapp \
  --resource-group rg-prod \
  --mode multiple

az containerapp update \
  --name myapp \
  --resource-group rg-prod \
  --image myappacr.azurecr.io/myapp:v2.0 \
  --revision-suffix v2

# Split traffic: 90% to v1, 10% to v2
az containerapp ingress traffic set \
  --name myapp \
  --resource-group rg-prod \
  --revision-weight myapp--v1=90 myapp--v2=10

# Promote to 100% after validation
az containerapp ingress traffic set \
  --name myapp \
  --resource-group rg-prod \
  --revision-weight myapp--v2=100
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Load Testing and how do you create and run a load test?

**Azure Load Testing** is a fully managed service for generating high-scale load using Apache JMeter scripts or a URL-based quick test. It spins up managed load agents, runs the test, and provides real-time and post-test analytics integrated with Azure Monitor and Application Insights.

**Key concepts:**

| Concept | Description |
|---------|-------------|
| **Test** | A load test configuration referencing a JMeter script, parameters, and engine instances |
| **Test Run** | One execution of a test — produces metrics and reports |
| **Engine instances** | Number of parallel JMeter VMs (1 instance ≈ 250 virtual users) |
| **Test criteria** | Pass/fail thresholds: error rate, response time p90/p99 |
| **App components** | Azure resources to monitor during the test (App Service, SQL, etc.) |

**Create and run a load test via CLI:**

```bash
# Install extension
az extension add --name load

# Create an Azure Load Testing resource
az load create \
  --name alt-myapp \
  --resource-group rg-perf \
  --location eastus

# Create a test from a JMeter file
az load test create \
  --test-id order-api-load-test \
  --load-test-resource alt-myapp \
  --resource-group rg-perf \
  --display-name "Order API Load Test" \
  --description "Ramp to 500 VU over 5 min, hold 10 min" \
  --test-plan ./load-tests/order-api.jmx \
  --engine-instances 2 \
  --env targetUrl="https://myapp-prod.azurewebsites.net"

# Run the test
az load test-run create \
  --test-id order-api-load-test \
  --test-run-id run-$(date +%Y%m%d-%H%M) \
  --load-test-resource alt-myapp \
  --resource-group rg-perf \
  --display-name "Run $(date)"
```

**JMeter script — HTTP load test pattern:**

```xml
<!-- order-api.jmx — key elements -->
<ThreadGroup>
  <numThreads>250</numThreads>      <!-- VUs per engine instance -->
  <rampTime>300</rampTime>          <!-- ramp-up seconds -->
  <duration>900</duration>          <!-- total test duration -->
  <HTTPSamplerProxy>
    <stringProp name="HTTPSampler.domain">${targetUrl}</stringProp>
    <stringProp name="HTTPSampler.path">/api/orders</stringProp>
    <stringProp name="HTTPSampler.method">POST</stringProp>
  </HTTPSamplerProxy>
</ThreadGroup>
```

**Integrate load test as a CI/CD gate in Azure DevOps:**

```yaml
- task: AzureLoadTesting@1
  displayName: Run load test
  inputs:
    azureSubscription: azure-service-connection
    loadTestConfigFile: load-tests/config.yaml
    resourceGroup: rg-perf
    loadTestResource: alt-myapp
    env: |
      [
        { "name": "targetUrl", "value": "https://$(webAppName).azurewebsites.net" }
      ]
    # Fail the pipeline if p90 response time > 2 seconds or error rate > 1%
    passFailCriteria: |
      [
        { "clientMetric": "response_time_ms", "aggregate": "p90", "condition": ">", "value": "2000", "action": "continue" },
        { "clientMetric": "error",            "aggregate": "percentage", "condition": ">", "value": "1",    "action": "continue" }
      ]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure PaaS and how do you choose the right PaaS service for your workload?

**Azure PaaS (Platform as a Service)** abstracts away OS, runtime, and middleware management — you only deploy and manage application code and data. This reduces operational overhead, enables automatic scaling, and improves time-to-market.

**Azure PaaS classification by workload type:**

| Workload | PaaS Service | When to Choose |
|----------|-------------|----------------|
| **Web / REST API** | Azure App Service | Stateless web apps, APIs needing autoscale and deployment slots |
| **Serverless / event-driven** | Azure Functions | Short-lived tasks, event triggers, scale-to-zero cost model |
| **Workflow integration** | Azure Logic Apps | Low-code integration across 400+ connectors |
| **Containerized apps** | Azure Container Apps | Microservices with Dapr, KEDA-driven autoscale, no k8s ops |
| **Batch / HPC** | Azure Batch | Massively parallel compute jobs |
| **Relational DB** | Azure SQL Database | Fully managed SQL Server with auto-tune and serverless tier |
| **NoSQL / global** | Azure Cosmos DB | Globally distributed, multi-model, low-latency at any scale |
| **Cache** | Azure Cache for Redis | In-memory session cache, leaderboards, pub/sub |
| **Messaging** | Azure Service Bus | Reliable, ordered enterprise messaging |
| **Event streaming** | Azure Event Hubs | High-throughput telemetry ingestion |
| **Data integration** | Azure Data Factory | ETL/ELT pipelines across cloud and on-premises |
| **Analytics** | Azure Synapse Analytics | Unified data warehouse + Spark analytics |
| **Search** | Azure AI Search | Full-text and vector search over your data |

**IaaS vs PaaS decision framework:**

```
Use IaaS (VMs) when:
  ✔ Full OS control required (custom kernel, drivers)
  ✔ Lift-and-shift legacy app that cannot run on PaaS
  ✔ Licensing requires dedicated hardware (Oracle, IBM)
  ✔ Complex networking that PaaS cannot support

Use PaaS otherwise:
  ✔ Focus on code/logic, not patching or infrastructure
  ✔ Need auto-scale, built-in HA, managed backups
  ✔ Reduce operational overhead and TCO
  ✔ Standardize on well-supported runtimes (.NET, Node, Python, Java)
```

**Well-Architected trade-off — PaaS cost model:**

```bash
# PaaS services can be sized to workload — example: App Service serverless (dynamic SKU)
az appservice plan create \
  --name asp-myapp \
  --resource-group rg-prod \
  --sku EP1 \         # Elastic Premium: scales to 0 between bursts
  --is-linux

# Compare: IaaS VM Standard_D4s_v5 = ~$140/month always-on
# vs Functions Consumption: $0 idle + $0.20/million executions
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 15. COST MANAGEMENT

<br>

## Q. How do you create budgets and configure cost alerts in Azure Cost Management?

**Azure Cost Management + Billing** provides budgets, cost analysis, and recommendations to monitor and control Azure spending. Budgets trigger alerts (email, Action Group) when actual or forecasted spend reaches defined thresholds.

```bash
# Create a budget at subscription scope with email alerts at 80% and 100%
az consumption budget create \
  --budget-name "MonthlyBudget-Prod" \
  --amount 5000 \
  --time-grain Monthly \
  --start-date "2025-01-01" \
  --end-date "2026-12-31" \
  --resource-group rg-prod \
  --notifications '[
    {
      "enabled": true,
      "operator": "GreaterThanOrEqualTo",
      "threshold": 80,
      "contactEmails": ["team@contoso.com"],
      "contactRoles": ["Owner"],
      "thresholdType": "Actual"
    },
    {
      "enabled": true,
      "operator": "GreaterThanOrEqualTo",
      "threshold": 100,
      "contactEmails": ["team@contoso.com"],
      "thresholdType": "Forecasted"
    }
  ]'

# Query cost for a specific time period
az costmanagement query \
  --type Usage \
  --timeframe Custom \
  --time-period from="2025-01-01" to="2025-01-31" \
  --dataset-granularity Monthly \
  --dataset-aggregation '{"totalCost":{"name":"PreTaxCost","function":"Sum"}}' \
  --dataset-grouping '[{"type":"Dimension","name":"ServiceName"}]'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Reserved Instances, Savings Plans, and Spot VMs?

| Option | Commitment | Discount vs. Pay-as-you-go | Flexibility | Best for |
|--------|-----------|---------------------------|------------|---------|
| **Pay-as-you-go** | None | 0% | Full | Unpredictable workloads |
| **Savings Plan** | 1 or 3 year ($/hr commitment) | Up to 65% | Any region, any VM family | General compute savings |
| **Reserved Instances** | 1 or 3 year (specific VM) | Up to 72% | Specific region/size (exchangeable) | Stable, predictable workloads |
| **Spot VMs** | None | Up to 90% | Evicted when Azure needs capacity | Batch jobs, HPC, fault-tolerant |

```bash
# Purchase a Reserved Instance (via Azure portal or REST API)
# Check savings with az billing

# Use Spot VMs for batch workloads
az vm create \
  --name vm-batch \
  --resource-group rg-batch \
  --image Ubuntu2204 \
  --size Standard_D4s_v3 \
  --priority Spot \
  --eviction-policy Deallocate \
  --max-price 0.05  # Maximum price per hour you're willing to pay

# Use Azure Hybrid Benefit to use existing Windows Server / SQL licenses
az vm create \
  --name vm-windows \
  --resource-group rg-prod \
  --image Win2022Datacenter \
  --size Standard_D4s_v3 \
  --license-type Windows_Server  # Use existing SA license

# Check Azure Hybrid Benefit savings
az vm list --query "[].{Name:name,LicenseType:licenseType}" --output table
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use tags and policies to allocate and control Azure costs by team or project?

Tags are key-value metadata applied to Azure resources for cost allocation and governance. Azure Policy enforces tagging standards.

```bash
# Apply tags to a resource group
az group update \
  --name rg-prod \
  --tags Environment=Production Team=BackendAPI CostCenter=CC-1234 Project=CustomerPortal

# Apply tags to an existing resource
az resource update \
  --ids $(az webapp show --name app-mywebapp --resource-group rg-prod --query id -o tsv) \
  --set tags.Environment=Production tags.Team=BackendAPI

# Enforce tag requirement via Azure Policy (deny resources without CostCenter tag)
az policy definition create \
  --name "require-costcenter-tag" \
  --display-name "Require CostCenter tag" \
  --mode Indexed \
  --rules '{
    "if": {
      "field": "[concat('"'"'tags['"'"', parameters('"'"'tagName'"'"'), '"'"']'"'"')]",
      "exists": "false"
    },
    "then": { "effect": "deny" }
  }' \
  --params '{"tagName":{"type":"String","defaultValue":"CostCenter"}}'

az policy assignment create \
  --name "require-costcenter-tag" \
  --policy "require-costcenter-tag" \
  --scope "/subscriptions/{subscription-id}"

# Export cost by tag in Cost Management
az costmanagement query \
  --type Usage \
  --timeframe MonthToDate \
  --dataset-grouping '[{"type":"TagKey","name":"Team"}]' \
  --dataset-aggregation '{"totalCost":{"name":"PreTaxCost","function":"Sum"}}'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure Blob lifecycle management to reduce storage costs?

**Blob lifecycle management** automatically transitions blobs between access tiers (Hot → Cool → Cold → Archive) or deletes them based on age and last-access time.

```json
{
  "rules": [
    {
      "name": "transition-old-blobs",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["logs/", "backups/"]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterLastAccessTimeGreaterThan": 30 },
            "tierToCold": { "daysAfterModificationGreaterThan": 90 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 180 },
            "delete": { "daysAfterModificationGreaterThan": 365 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

```bash
# Apply lifecycle policy to storage account
az storage account management-policy create \
  --account-name stproddata \
  --resource-group rg-prod \
  --policy @lifecycle-policy.json

# Access tier pricing comparison
# Hot: Higher storage cost, lowest access cost
# Cool: Lower storage cost, higher access cost (min 30-day storage)
# Cold: Even lower storage cost (min 90-day storage)
# Archive: Lowest storage cost, hours to rehydrate (min 180-day storage)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Azure Advisor and Cost Management to identify and act on savings opportunities?

```bash
# Get all Cost recommendations from Azure Advisor
az advisor recommendation list \
  --category Cost \
  --output table

# Get specific recommendations with estimated savings
az advisor recommendation list \
  --category Cost \
  --query "[].{Resource:impactedValue,Impact:impact,Description:shortDescription.problem,Savings:extendedProperties.savingsAmount}" \
  --output json

# Right-size underutilized VMs (Advisor identifies VMs with <5% avg CPU over 7 days)
# After reviewing recommendations, resize:
az vm deallocate --name vm-dev --resource-group rg-dev
az vm resize --name vm-dev --resource-group rg-dev --size Standard_B2s
az vm start --name vm-dev --resource-group rg-dev

# Delete unattached managed disks (Advisor identifies these)
az disk list \
  --query "[?diskState=='Unattached'].{Name:name,Size:diskSizeGb,RG:resourceGroup}" \
  --output table

# Delete after verification
az disk delete --name orphaned-disk-01 --resource-group rg-dev --yes

# Get Reserved Instance purchase recommendations
az advisor recommendation list \
  --category Cost \
  --query "[?contains(shortDescription.problem, 'reserved')]"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 16. AI SERVICES

<br>

## Q. What is Azure OpenAI Service and how do you integrate GPT-4o into a .NET application?

**Azure OpenAI Service** provides access to OpenAI models (GPT-4o, GPT-4o mini, o1, text-embedding-ada-002, DALL-E 3) deployed in Azure — with enterprise security, private networking, RBAC, compliance, and no data sent to train OpenAI models.

```csharp
// .NET 8: Azure OpenAI SDK — Chat Completion with GPT-4o
using Azure;
using Azure.AI.OpenAI;
using OpenAI.Chat;

var client = new AzureOpenAIClient(
    new Uri("https://oai-myapp.openai.azure.com/"),
    new DefaultAzureCredential()  // Use Managed Identity, not API key
);

ChatClient chatClient = client.GetChatClient("gpt-4o");  // deployment name

var messages = new List<ChatMessage>
{
    new SystemChatMessage("You are a helpful customer support assistant."),
    new UserChatMessage("What is the status of my order #12345?")
};

var options = new ChatCompletionOptions
{
    MaxOutputTokenCount = 1000,
    Temperature = 0.7f
};

ChatCompletion response = await chatClient.CompleteChatAsync(messages, options);
Console.WriteLine(response.Content[0].Text);

// Streaming response
await foreach (StreamingChatCompletionUpdate update in chatClient.CompleteChatStreamingAsync(messages))
{
    foreach (var part in update.ContentUpdate)
        Console.Write(part.Text);
}
```

```bash
# Deploy Azure OpenAI resource
az cognitiveservices account create \
  --name oai-myapp \
  --resource-group rg-ai \
  --location eastus \
  --kind OpenAI \
  --sku S0

# Deploy GPT-4o model
az cognitiveservices account deployment create \
  --name oai-myapp \
  --resource-group rg-ai \
  --deployment-name gpt-4o \
  --model-name gpt-4o \
  --model-version "2024-11-20" \
  --model-format OpenAI \
  --sku-capacity 30 \
  --sku-name GlobalStandard
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure AI Foundry and how does it support building generative AI applications?

**Azure AI Foundry** (formerly Azure AI Studio) is the unified platform for building, testing, and deploying AI applications and custom copilots. Key capabilities:

- **Model catalog** — Access 1,800+ models (Azure OpenAI, Meta Llama, Mistral, Phi, Hugging Face)
- **Prompt flow** — Visual LLM orchestration pipeline (RAG, multi-step reasoning)
- **Evaluations** — Automated quality metrics (groundedness, coherence, relevance, safety)
- **Azure AI Search** — Vector search for RAG (Retrieval-Augmented Generation) patterns
- **Safety filters** — Content filtering, responsible AI controls, groundedness detection

```python
# Python SDK: RAG pattern with Azure AI Foundry + Azure AI Search
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential

client = AIProjectClient.from_connection_string(
    conn_str="<project-connection-string>",
    credential=DefaultAzureCredential()
)

# Get the Azure OpenAI client from the project
aoai_client = client.inference.get_azure_openai_client(api_version="2024-12-01-preview")

response = aoai_client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "You are a helpful assistant. Answer based on the provided context."},
        {"role": "user", "content": "What are the key features of our product?"}
    ],
    extra_body={
        "data_sources": [{
            "type": "azure_search",
            "parameters": {
                "endpoint": "https://search-myapp.search.windows.net",
                "index_name": "product-docs",
                "authentication": {"type": "system_assigned_managed_identity"}
            }
        }]
    }
)
print(response.choices[0].message.content)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What Azure AI services are available and what are their main use cases?

**Azure AI Services** (formerly Cognitive Services) provide pre-built AI capabilities:

| Category | Service | Use Cases |
|----------|---------|-----------|
| **Vision** | Azure AI Vision | OCR, image analysis, face detection, spatial analysis |
| **Vision** | Azure AI Custom Vision | Train custom image classifiers and object detectors |
| **Speech** | Azure AI Speech | Speech-to-text, text-to-speech, translation, speaker ID |
| **Language** | Azure AI Language | Sentiment, key phrases, NER, summarization, QA |
| **Language** | Azure AI Translator | Real-time translation (100+ languages) |
| **Decision** | Azure AI Content Moderator | Detect offensive text/images |
| **Search** | Azure AI Search | Full-text + vector search, RAG pipelines |
| **Document** | Azure AI Document Intelligence | Extract structured data from forms, invoices, IDs |

```python
# Azure AI Document Intelligence: Extract data from an invoice
from azure.ai.formrecognizer import DocumentAnalysisClient
from azure.core.credentials import AzureKeyCredential

client = DocumentAnalysisClient(
    endpoint="https://docai-myapp.cognitiveservices.azure.com/",
    credential=AzureKeyCredential("<key>")
)

with open("invoice.pdf", "rb") as f:
    poller = client.begin_analyze_document("prebuilt-invoice", f)
    result = poller.result()

for invoice in result.documents:
    vendor = invoice.fields.get("VendorName")
    total = invoice.fields.get("InvoiceTotal")
    print(f"Vendor: {vendor.value}, Total: {total.value.amount} {total.value.currency_symbol}")
```

<p align="center">
  <img src="https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/media/overview/analyze-general-document.png" alt="Azure AI Services Overview" width="750px" />
</p>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you train and deploy a machine learning model using Azure Machine Learning?

**Azure Machine Learning (Azure ML)** is the enterprise MLOps platform — from data preparation and model training to deployment and monitoring.

**Core components:**
- **Workspace** — Central hub for all ML assets
- **Compute clusters** — Managed GPU/CPU clusters for training
- **Datasets/Data Assets** — Versioned data references
- **Pipelines** — Reusable ML workflow steps
- **Model registry** — Versioned model storage with metadata
- **Endpoints** — Online (real-time) and batch inference endpoints

```python
# Azure ML Python SDK v2: Train + register + deploy a model
from azure.ai.ml import MLClient, command
from azure.ai.ml.entities import Model, ManagedOnlineEndpoint, ManagedOnlineDeployment
from azure.identity import DefaultAzureCredential

ml_client = MLClient(
    DefaultAzureCredential(),
    subscription_id="<sub-id>",
    resource_group_name="rg-ml",
    workspace_name="aml-myapp"
)

# Submit a training job
job = command(
    code="./src",
    command="python train.py --data ${{inputs.training_data}} --model-output ${{outputs.model}}",
    inputs={"training_data": Input(type="uri_folder", path="azureml://datastores/workspaceblobstore/paths/data/")},
    outputs={"model": Output(type="uri_folder")},
    environment="AzureML-sklearn-1.5-ubuntu22.04-py39-cpu@latest",
    compute="gpu-cluster",
    experiment_name="model-training"
)

returned_job = ml_client.jobs.create_or_update(job)
ml_client.jobs.stream(returned_job.name)

# Register the model
model = ml_client.models.create_or_update(
    Model(name="my-classifier", version="1", path=f"azureml://jobs/{returned_job.name}/outputs/model")
)

# Deploy to online endpoint
endpoint = ManagedOnlineEndpoint(name="ep-classifier", auth_mode="key")
ml_client.online_endpoints.begin_create_or_update(endpoint).result()

deployment = ManagedOnlineDeployment(
    name="blue",
    endpoint_name="ep-classifier",
    model=f"azureml:{model.name}:{model.version}",
    instance_type="Standard_DS3_v2",
    instance_count=1
)
ml_client.online_deployments.begin_create_or_update(deployment).result()
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Responsible AI in Azure and what guardrails does Azure OpenAI provide?

**Microsoft\'s Responsible AI principles:** Fairness, Reliability & Safety, Privacy & Security, Inclusiveness, Transparency, Accountability.

**Azure OpenAI content safety** is layered:
1. **Platform-level filters** — Automatic moderation of hate, self-harm, violence, sexual content (severity 0-7 per category). Configured per deployment.
2. **Prompt injection detection** — Detects attempts to override system instructions.
3. **Groundedness detection** — For RAG scenarios, flags hallucinated responses not supported by retrieved documents.
4. **Azure AI Content Safety** — Standalone service for moderation of both text and images.

```python
# Configure content filters on an Azure OpenAI deployment
from azure.ai.contentsafety import ContentSafetyClient
from azure.ai.contentsafety.models import AnalyzeTextOptions, TextCategory
from azure.core.credentials import AzureKeyCredential

client = ContentSafetyClient(
    endpoint="https://cs-myapp.cognitiveservices.azure.com/",
    credential=AzureKeyCredential("<key>")
)

request = AnalyzeTextOptions(
    text="User-submitted content here",
    categories=[TextCategory.HATE, TextCategory.SELF_HARM, TextCategory.VIOLENCE, TextCategory.SEXUAL],
    output_type="FourSeverityLevels"
)

response = client.analyze_text(request)

for result in response.categories_analysis:
    print(f"{result.category}: severity={result.severity}")
    if result.severity >= 2:
        print("  -> Block this content")
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 17. MISCELLANEOUS

<br>

## Q. What is Azure Policy and how do you enforce governance across subscriptions?

**Azure Policy** evaluates resources against business rules (policy definitions) and enforces compliance. It can **audit**, **deny**, **append**, **modify**, or **deploy-if-not-exists** configurations.

**Policy initiatives** (policy sets) group related policies — e.g., the built-in "Azure Security Benchmark" initiative contains 200+ policies.

```bash
# View built-in policies related to storage
az policy definition list \
  --query "[?policyType=='BuiltIn' && contains(displayName, 'storage')].{Name:displayName,ID:name}" \
  --output table

# Assign built-in policy: "Require HTTPS on storage accounts"
az policy assignment create \
  --name "require-https-storage" \
  --display-name "Require HTTPS on Storage Accounts" \
  --policy $(az policy definition list --query "[?displayName=='Secure transfer to storage accounts should be enabled'].name" -o tsv) \
  --scope "/subscriptions/{subscription-id}" \
  --enforcement-mode Default  # Deny non-compliant resources

# Create custom deny policy: only allow specific VM SKUs
az policy definition create \
  --name "allowed-vm-skus" \
  --display-name "Restrict VM sizes to approved SKUs" \
  --mode Indexed \
  --rules '{
    "if": {
      "allOf": [
        { "field": "type", "equals": "Microsoft.Compute/virtualMachines" },
        { "not": { "field": "Microsoft.Compute/virtualMachines/sku.name",
          "in": ["Standard_B2s","Standard_D2s_v3","Standard_D4s_v3"] } }
      ]
    },
    "then": { "effect": "deny" }
  }'

# Check compliance state
az policy state summarize \
  --subscription "{subscription-id}" \
  --filter "PolicyAssignmentName eq 'require-https-storage'"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Arc and how does it enable hybrid and multi-cloud management?

**Azure Arc** extends Azure management plane to non-Azure infrastructure — on-premises servers, Kubernetes clusters, and SQL instances running anywhere (AWS, GCP, on-premises, edge).

**Azure Arc-enabled servers:** Register any server (Windows/Linux) with Azure → apply Azure Policy, RBAC, Defender for Cloud, Update Manager, Log Analytics — as if it were an Azure resource.

**Azure Arc-enabled Kubernetes:** Connect any Kubernetes cluster to Azure → deploy workloads with GitOps (Flux), apply Kubernetes policies (Gatekeeper), monitor with Container Insights.

```bash
# Arc-enable an on-premises Linux server (run on the server)
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

az connectedmachine connect \
  --resource-group rg-arc \
  --name server-onprem-01 \
  --location eastus \
  --subscription "{subscription-id}"

# After onboarding, apply Azure Policy to Arc server
az policy assignment create \
  --name "arc-enable-monitoring" \
  --policy "a4034bc6-ae50-406d-bf76-2aba7e9a1f2a" \  # Deploy Log Analytics agent
  --scope "/subscriptions/{sub}/resourceGroups/rg-arc"

# Arc-enable an on-premises Kubernetes cluster
az connectedk8s connect \
  --name k8s-onprem-cluster \
  --resource-group rg-arc

# Deploy workloads via GitOps (Flux)
az k8s-configuration flux create \
  --cluster-name k8s-onprem-cluster \
  --cluster-type connectedClusters \
  --resource-group rg-arc \
  --name gitops-config \
  --namespace gitops \
  --url https://github.com/myorg/k8s-config \
  --branch main \
  --kustomization name=infra path=./infra
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Synapse Analytics and how does it differ from Azure Data Factory?

**Azure Synapse Analytics** is an integrated analytics service combining big data and data warehousing. It includes Synapse SQL (serverless and dedicated), Apache Spark, Data Integration (built-in ADF-like pipelines), and native integration with Power BI and Azure ML.

| Aspect | Azure Synapse | Azure Data Factory |
|--------|--------------|-------------------|
| Purpose | Analytics + ETL + warehousing | ETL/ELT pipelines |
| SQL | Serverless + Dedicated SQL pools | External sink/source |
| Spark | Native Spark pools | Via HDInsight linked service |
| Code | Notebooks (Python, Scala, SQL) | GUI pipelines primarily |
| Best for | End-to-end analytics workloads | Pure data movement/transformation |

```python
# Synapse Spark notebook: Process data from ADLS Gen2 and write to dedicated SQL pool
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, year, month, dayofmonth

spark = SparkSession.builder.getOrCreate()

# Read Parquet files from Azure Data Lake Storage Gen2
df = spark.read.parquet("abfss://raw@datalake.dfs.core.windows.net/events/2025/")

# Transform
df_transformed = df \
    .filter(col("status") == "completed") \
    .withColumn("year", year(col("event_time"))) \
    .withColumn("month", month(col("event_time"))) \
    .groupBy("year", "month", "product_id") \
    .agg({"amount": "sum", "order_id": "count"}) \
    .withColumnRenamed("sum(amount)", "total_revenue") \
    .withColumnRenamed("count(order_id)", "order_count")

# Write to Synapse dedicated SQL pool
df_transformed.write \
    .format("com.microsoft.sqlserver.jdbc.spark") \
    .option("url", "jdbc:sqlserver://synapse-ws.sql.azuresynapse.net:1433;database=sqldw") \
    .option("dbtable", "dbo.monthly_revenue") \
    .option("authentication", "ActiveDirectoryMSI") \
    .mode("overwrite") \
    .save()
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Automation and how do you use Runbooks for scheduled tasks?

**Azure Automation** provides process automation using PowerShell or Python **Runbooks**, Desired State Configuration (DSC) for config management, Update Management for OS patching, and Change Tracking.

```powershell
# Example Runbook: Stop all VMs in a resource group tagged "AutoShutdown=true"
param (
    [Parameter(Mandatory = $true)]
    [string]$ResourceGroupName
)

# Authenticate using system-assigned Managed Identity
Connect-AzAccount -Identity

# Get all VMs with the AutoShutdown tag
$vms = Get-AzVM -ResourceGroupName $ResourceGroupName |
    Where-Object { $_.Tags["AutoShutdown"] -eq "true" }

foreach ($vm in $vms) {
    $state = (Get-AzVM -ResourceGroupName $ResourceGroupName -Name $vm.Name -Status).Statuses |
        Where-Object { $_.Code -like "PowerState/*" }

    if ($state.Code -eq "PowerState/running") {
        Write-Output "Stopping VM: $($vm.Name)"
        Stop-AzVM -ResourceGroupName $ResourceGroupName -Name $vm.Name -Force
    }
}
```

```bash
# Create Automation Account
az automation account create \
  --name aa-myapp \
  --resource-group rg-ops \
  --location eastus \
  --sku Basic

# Enable system-assigned managed identity on the Automation Account
az automation account identity assign \
  --name aa-myapp \
  --resource-group rg-ops \
  --system-assigned

# Grant VM Contributor role to the Automation Account identity
az role assignment create \
  --assignee $(az automation account show --name aa-myapp --resource-group rg-ops --query identity.principalId -o tsv) \
  --role "Virtual Machine Contributor" \
  --scope "/subscriptions/{subscription-id}"

# Create and schedule a Runbook
az automation runbook create \
  --automation-account-name aa-myapp \
  --resource-group rg-ops \
  --name StopTaggedVMs \
  --type PowerShell

az automation schedule create \
  --automation-account-name aa-myapp \
  --resource-group rg-ops \
  --name "Nightly-7PM" \
  --frequency Day \
  --interval 1 \
  --start-time "2025-01-01T19:00:00+00:00"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 18. GENERAL QUESTIONS

<br>

## Q. What is the difference between IaaS, PaaS, and SaaS in Azure?

**Cloud service models** define how much infrastructure the cloud provider manages vs. the customer:

| Layer | IaaS | PaaS | SaaS |
|-------|------|------|------|
| Applications | Customer | Customer | Provider |
| Data | Customer | Customer | Provider |
| Runtime | Customer | Provider | Provider |
| OS | Customer | Provider | Provider |
| Virtualization | Provider | Provider | Provider |
| Hardware | Provider | Provider | Provider |

**Azure examples:**

| Model | Azure Service | Use Case |
|-------|--------------|----------|
| IaaS | Azure Virtual Machines, Azure Disk Storage | Lift-and-shift, full OS control |
| PaaS | App Service, Azure SQL, AKS, Functions | App development without OS management |
| SaaS | Microsoft 365, Dynamics 365, Power BI | Fully managed business applications |

```bash
# IaaS: Full control - manage OS, runtime, app
az vm create --name vm-app --image Ubuntu2204 --size Standard_D2s_v3 ...

# PaaS: Managed runtime - focus on code only
az webapp create --name app-myapp --runtime "DOTNETCORE:8.0" --plan asp-prod ...

# SaaS: No infrastructure concern - just use the app
# Example: Teams, SharePoint, Microsoft 365 via browser
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Azure Regions and Availability Zones and why are they important?

**Azure Regions** are geographic areas containing one or more physically isolated datacenters. They allow data residency compliance and latency optimization (e.g., East US, West Europe, Southeast Asia).

**Availability Zones (AZs)** are physically separate locations within a region, each with independent power, cooling, and networking. Distributing resources across AZs provides **99.99% SLA** for VMs (vs. 99.9% for a single-AZ VM).

**Paired regions:** Each Azure region is paired with another region 300+ miles away. Microsoft serializes updates across pairs and prioritizes recovery of one region in a disaster.

```bash
# List regions with availability zones
az account list-locations \
  --query "[?metadata.regionCategory=='Recommended'].{Region:name,DisplayName:displayName}" \
  --output table

# Deploy a zone-redundant resource (VM across AZs)
az vm create \
  --name vm-zone1 \
  --resource-group rg-prod \
  --image Ubuntu2204 \
  --zone 1 \
  --size Standard_D2s_v3 \
  --generate-ssh-keys

# Zone-redundant storage (ZRS) — data replicated across 3 AZs
az storage account create \
  --name stproddata \
  --resource-group rg-prod \
  --sku Standard_ZRS \  # Zone-redundant
  --location eastus

# Zone-redundant Azure SQL Database (Business Critical tier)
az sql db create \
  --name mydb \
  --server sql-prod \
  --resource-group rg-prod \
  --service-objective BusinessCritical \
  --zone-redundant true
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Azure Resource Manager and what are the benefits of using ARM templates or Bicep over manual deployments?

**Azure Resource Manager (ARM)** is the management layer for all Azure resources. Every Azure action (CLI, portal, SDK, Terraform, Bicep) goes through ARM. ARM provides:
- **Consistent API** — same behavior regardless of tool used
- **RBAC** — enforced at ARM layer for all resources
- **Tagging and grouping** — resource groups and tags
- **Declarative templates** — ARM JSON or Bicep describe the desired state

**Benefits of IaC (Bicep/ARM) over manual deployment:**

| Benefit | Explanation |
|---------|-------------|
| Repeatability | Same template deploys identical environments every time |
| Version control | Infrastructure changes tracked in Git |
| What-if previews | Preview changes before applying |
| Idempotency | Re-running the template is safe — only changes what\'s different |
| Documentation | Template IS the documentation |

```bicep
// Bicep is the recommended IaC approach — compiles to ARM JSON
// Validate and preview before deployment
az deployment group what-if \
  --resource-group rg-prod \
  --template-file main.bicep \
  --parameters environment=prod

// Deploy
az deployment group create \
  --resource-group rg-prod \
  --template-file main.bicep \
  --parameters environment=prod

// Export existing infrastructure as ARM template (for starting point)
az group export \
  --name rg-prod \
  --output json > existing-infra.json
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you choose between Azure Functions and Azure App Service for hosting an application?

| Factor | Azure Functions | Azure App Service |
|--------|----------------|-------------------|
| Execution model | Event-driven, stateless functions | Long-running web applications |
| Billing | Per execution (Consumption plan) | Per instance (always-on) |
| Cold start | Yes (Consumption plan) | No |
| State | Stateless (use Durable Functions for state) | Stateful (session, in-memory cache) |
| Protocols | HTTP, queues, blobs, timers, events | HTTP(S), WebSockets, gRPC |
| Max duration | 5 min (Consumption) / unlimited (Premium) | Unlimited |
| Best for | Microservices, event processing, scheduled jobs | Web apps, APIs, background services |

```bash
# App Service: Good for a full web API with always-on requirement
az webapp create \
  --name app-myapi \
  --resource-group rg-prod \
  --plan asp-prod \
  --runtime "DOTNETCORE:8.0"

# Functions (Consumption): Good for event-triggered, infrequent tasks
az functionapp create \
  --name func-processor \
  --resource-group rg-prod \
  --storage-account stfunctions \
  --consumption-plan-location eastus \
  --runtime dotnet-isolated \
  --runtime-version 8

# Functions (Premium plan): Avoids cold start, supports VNet integration
az functionapp plan create \
  --name fap-premium \
  --resource-group rg-prod \
  --location eastus \
  --sku EP1  # Elastic Premium 1

az functionapp create \
  --name func-premium \
  --resource-group rg-prod \
  --plan fap-premium \
  --storage-account stfunctions \
  --runtime dotnet-isolated
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some Azure best practices for security, reliability, cost, and operational excellence?

**Azure Well-Architected Framework (WAF) best practices:**

**Security:**
- Use **Managed Identities** — never hardcode credentials
- Enable **Microsoft Defender for Cloud** (Secure Score)
- Apply **least-privilege RBAC** at the narrowest scope
- Store all secrets in **Azure Key Vault**, rotate regularly
- Enable **private endpoints** for PaaS services (no public internet exposure)
- Enable **Azure DDoS Protection Standard** for public-facing apps

**Reliability:**
- Deploy across **multiple Availability Zones** for 99.99% SLA
- Use **Azure Front Door** or **Traffic Manager** for multi-region routing
- Implement **retry patterns** with exponential backoff
- Define **RTO and RPO** → configure Azure Site Recovery accordingly
- Test failures with **Azure Chaos Studio**

**Cost optimization:**
- Buy **Reserved Instances or Savings Plans** for stable workloads
- Use **Spot VMs** for fault-tolerant batch jobs
- Apply **Azure Hybrid Benefit** for existing Windows/SQL licenses
- Set up **budgets and alerts** in Azure Cost Management
- Apply **lifecycle policies** for blob storage tiering

**Operational Excellence:**
- Use **Bicep/Terraform** for all infrastructure (version-controlled IaC)
- Implement **multi-stage CI/CD pipelines** (Azure DevOps / GitHub Actions)
- Enable **Application Insights** for all applications
- Use **deployment slots** for zero-downtime releases
- Tag all resources with Environment, Team, CostCenter, Project

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Network Security Groups (NSGs), and can they be applied at the VNet level?

A **Network Security Group (NSG)** is a stateful Layer-4 firewall that contains a list of inbound and outbound security rules. Each rule specifies source/destination IP, port range, protocol, and an Allow/Deny action.

**NSG attachment levels:**

| Level | Effect |
|-------|--------|
| **Subnet** | Rules apply to all NICs within that subnet (most common) |
| **NIC** | Rules apply only to the single VM network interface |
| **VNet level** | NSGs **cannot** be applied directly to a VNet; apply them to each subnet instead |

> **Key point:** There is no "VNet-level NSG." To achieve VNet-wide filtering, attach an NSG to every subnet in the VNet, or use **Azure Firewall** for centralized network-level control.

**Example — create and attach an NSG to a subnet:**

```bash
# Create the NSG
az network nsg create \
  --name nsg-web \
  --resource-group rg-prod \
  --location eastus

# Allow HTTPS inbound from the internet
az network nsg rule create \
  --nsg-name nsg-web \
  --resource-group rg-prod \
  --name Allow-HTTPS \
  --priority 100 \
  --direction Inbound \
  --protocol Tcp \
  --source-address-prefixes Internet \
  --destination-port-ranges 443 \
  --access Allow

# Deny all other inbound traffic (explicit catch-all)
az network nsg rule create \
  --nsg-name nsg-web \
  --resource-group rg-prod \
  --name Deny-All-Inbound \
  --priority 4000 \
  --direction Inbound \
  --protocol '*' \
  --source-address-prefixes '*' \
  --destination-port-ranges '*' \
  --access Deny

# Attach the NSG to the subnet (not to the VNet directly)
az network vnet subnet update \
  --vnet-name vnet-prod \
  --resource-group rg-prod \
  --name snet-web \
  --network-security-group nsg-web

# Verify effective rules on a VM NIC
az network nic show-effective-nsg \
  --name nic-vm-web \
  --resource-group rg-prod \
  --output table
```

**NSG processing order:**
- Inbound: subnet NSG rules → NIC NSG rules
- Outbound: NIC NSG rules → subnet NSG rules
- Lower priority number = evaluated first; first matching rule wins.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure App Services and Azure Functions?

Both are PaaS offerings for running application code in Azure, but they target different workload patterns:

| Factor | Azure App Service | Azure Functions |
|--------|------------------|-----------------|
| Execution model | Continuous, long-running process | Event-driven, short-lived executions |
| Billing | Per plan/instance (always-on) | Per execution + GB-s (Consumption) |
| Cold start | None (always warm) | Yes on Consumption plan |
| State | Stateful (sessions, in-memory cache) | Stateless by default (use Durable Functions for state) |
| Protocols | HTTP(S), WebSockets, gRPC | HTTP, queues, blobs, timers, Event Grid, Service Bus… |
| Max execution time | Unlimited | 5 min (Consumption) / unlimited (Premium/Dedicated) |
| VNet integration | Yes (Standard+ plan) | Yes (Premium plan) |
| Auto-scaling | Manual or rule-based | Automatic (Consumption/Premium) |
| Best for | Web apps, REST APIs, background workers | Microservices, event processing, scheduled jobs, webhooks |

**When to choose App Service:**
- Traditional web application or REST API that needs persistent connections
- WebSocket support (e.g., SignalR hub)
- Predictable, steady traffic where always-on cost is acceptable
- Deployment slots for blue-green releases

**When to choose Azure Functions:**
- Sporadic or unpredictable workloads (pay only when running)
- React to events (blob uploaded, queue message, HTTP webhook)
- Short-lived business logic (< 10 minutes per invocation)
- Fan-out / parallel processing patterns

```bash
# App Service — deploy a .NET 8 web API (always running)
az appservice plan create \
  --name asp-prod \
  --resource-group rg-prod \
  --sku S1 \
  --is-linux

az webapp create \
  --name app-myapi \
  --resource-group rg-prod \
  --plan asp-prod \
  --runtime "DOTNETCORE:8.0"

# Azure Functions — deploy an event-driven function (pay-per-use)
az storage account create \
  --name stfunctions \
  --resource-group rg-prod \
  --sku Standard_LRS

az functionapp create \
  --name func-processor \
  --resource-group rg-prod \
  --storage-account stfunctions \
  --consumption-plan-location eastus \
  --runtime dotnet-isolated \
  --runtime-version 8 \
  --functions-version 4
```

```csharp
// Azure Function: HTTP trigger — invoked only on demand
[Function("GetOrder")]
public IActionResult Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", Route = "orders/{id}")] HttpRequest req,
    string id)
{
    // Executes only when called; billed per invocation
    return new OkObjectResult(orderService.GetById(id));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How would you troubleshoot an Azure Virtual Machine that suddenly fails to start?

**Structured troubleshooting steps:**

**Step 1 — Check the VM power state and allocation status**

```bash
az vm get-instance-view \
  --name vm-prod \
  --resource-group rg-prod \
  --query "instanceView.statuses[*].displayStatus" \
  --output tsv
# Expected: "VM running" — if "VM stopped (deallocated)" or "Provisioning failed", proceed below
```

**Step 2 — Review Boot Diagnostics (serial console log + screenshot)**

```bash
# Enable boot diagnostics (requires storage account)
az vm boot-diagnostics enable \
  --name vm-prod \
  --resource-group rg-prod \
  --storage "https://stbootdiag.blob.core.windows.net/"

# Get the boot diagnostics screenshot and log URI
az vm boot-diagnostics get-boot-log \
  --name vm-prod \
  --resource-group rg-prod
# Portal: VM blade → Boot diagnostics → Screenshot shows last console state
```

**Step 3 — Review the Activity Log for error details**

```bash
az monitor activity-log list \
  --resource-group rg-prod \
  --start-time "$(Get-Date -Format 'yyyy-MM-ddTHH:mm:ssZ' -Date (Get-Date).AddHours(-4))" \
  --status Failed \
  --query "[].{Time:eventTimestamp,Operation:operationName.value,Error:properties.statusMessage}" \
  --output table
```

**Step 4 — Common failure causes and fixes**

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| `SkuNotAvailable` / allocation error | No capacity in that VM size/region | Resize VM or change region/zone |
| OS disk not found | Disk deleted or detached | Re-attach OS disk from the portal |
| `NTLDR is missing` / GRUB error | Boot record corruption | Attach OS disk to a repair VM; fix MBR/GRUB |
| Kernel panic (Linux) | Bad kernel update | Use serial console to boot into recovery mode |
| `Quota exceeded` | Subscription core quota hit | Request quota increase in Azure portal |
| NSG blocks traffic | Port 22/3389 blocked | Update NSG rule; use Azure Bastion instead |

**Step 5 — Use Azure Run Command (no SSH/RDP needed)**

```bash
# Run diagnostics inside the VM without network access
az vm run-command invoke \
  --name vm-prod \
  --resource-group rg-prod \
  --command-id RunShellScript \
  --scripts \
    "systemctl list-units --failed" \
    "df -h" \
    "dmesg | tail -50"
```

**Step 6 — Redeploy the VM to a new host node**

```bash
# Moves the VM to a healthy host without changing IP/data
az vm redeploy \
  --name vm-prod \
  --resource-group rg-prod
```

> If none of the above resolves the issue, open an Azure Support ticket and attach the boot diagnostics log and Activity Log errors.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Scenario: "We have an active-passive multi-region web application layout. How do we automate traffic failover if the primary region goes dark, and how do we handle database synchronization?"

**Solution architecture overview:**

```
Internet
   │
   ▼
Azure Front Door / Traffic Manager          ← health-probe-based failover
   │                    │
   ▼                    ▼
Primary Region       Secondary Region
(East US — active)  (West US — passive)
  App Service          App Service
  Azure SQL            Azure SQL (geo-replica, read-only → promoted on failover)
  Cosmos DB            Cosmos DB (multi-region write replica)
```

---

### Traffic Management

Implement **Azure Front Door** (recommended) or **Azure Traffic Manager** with health probes to continuously monitor the primary endpoint. When the primary region fails, traffic is automatically rerouted to the secondary region.

**Azure Front Door with automatic failover (priority routing):**

```bash
# Create Front Door profile
az afd profile create \
  --profile-name afd-myapp \
  --resource-group rg-global \
  --sku Standard_AzureFrontDoor

# Create origin group with health probes
az afd origin-group create \
  --profile-name afd-myapp \
  --resource-group rg-global \
  --origin-group-name og-webapp \
  --probe-request-type GET \
  --probe-protocol Https \
  --probe-path "/health" \
  --probe-interval-in-seconds 30 \
  --sample-size 4 \
  --successful-samples-required 3

# Add primary origin (priority 1)
az afd origin create \
  --profile-name afd-myapp \
  --resource-group rg-global \
  --origin-group-name og-webapp \
  --origin-name origin-primary \
  --host-name app-primary.azurewebsites.net \
  --origin-host-header app-primary.azurewebsites.net \
  --priority 1 \
  --weight 1000 \
  --enabled-state Enabled

# Add secondary origin (priority 2 — passive, receives traffic only if primary fails)
az afd origin create \
  --profile-name afd-myapp \
  --resource-group rg-global \
  --origin-group-name og-webapp \
  --origin-name origin-secondary \
  --host-name app-secondary.azurewebsites.net \
  --origin-host-header app-secondary.azurewebsites.net \
  --priority 2 \
  --weight 1000 \
  --enabled-state Enabled
```

> **How failover works:** Front Door probes the `/health` endpoint every 30 seconds. If the primary fails 4 consecutive probes, it is marked unhealthy and all traffic shifts to the secondary origin automatically — no manual intervention required.

---

### Data Replication

**Relational data — Azure SQL active geo-replication:**

```bash
# Create geo-replica in secondary region (asynchronous streaming)
az sql db replica create \
  --name mydb \
  --server sql-primary \
  --resource-group rg-eastus \
  --partner-server sql-secondary \
  --partner-resource-group rg-westus

# Check replication lag
az sql db show \
  --name mydb \
  --server sql-secondary \
  --resource-group rg-westus \
  --query "replicationLinks"

# Failover: promote secondary to primary (planned — zero data loss)
az sql db replica set-primary \
  --name mydb \
  --server sql-secondary \
  --resource-group rg-westus

# Failover: force failover (unplanned — possible data loss for last async transactions)
az sql db replica set-primary \
  --name mydb \
  --server sql-secondary \
  --resource-group rg-westus \
  --allow-data-loss
```

> Use **Auto-failover groups** to also fail over the DNS connection string automatically (no app connection string change needed):

```bash
az sql failover-group create \
  --name fog-myapp \
  --server sql-primary \
  --resource-group rg-eastus \
  --partner-server sql-secondary \
  --partner-resource-group rg-westus \
  --failover-policy Automatic \
  --grace-period 1   # hours before automatic failover triggers
```

**NoSQL data — Azure Cosmos DB multi-region replication:**

```bash
# Add secondary region to Cosmos DB account (automatic replication)
az cosmosdb update \
  --name cosmos-myapp \
  --resource-group rg-global \
  --locations regionName=eastus failoverPriority=0 isZoneRedundant=true \
                regionName=westus failoverPriority=1 isZoneRedundant=true

# Enable automatic failover (Cosmos DB promotes secondary on primary outage)
az cosmosdb update \
  --name cosmos-myapp \
  --resource-group rg-global \
  --enable-automatic-failover true

# Enable multi-region writes (active-active instead of active-passive)
az cosmosdb update \
  --name cosmos-myapp \
  --resource-group rg-global \
  --enable-multiple-write-locations true
```

> Cosmos DB replicates data continuously and asynchronously. With **automatic failover enabled**, if the primary region becomes unavailable, Cosmos DB promotes the next region in the failover priority list — typically within minutes.

**Summary of failover automation:**

| Layer | Service | Failover mechanism | RTO |
|-------|---------|-------------------|-----|
| Traffic | Azure Front Door | Health probes (30 s interval) | ~1–2 min |
| Relational DB | Azure SQL Auto-failover group | Automatic after grace period | ~1 h (configurable) |
| NoSQL DB | Cosmos DB automatic failover | Automatic on region outage | ~15–30 min |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

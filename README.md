# Microsoft Azure Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Table of Contents

* [Azure Fundamentals](#-1-azure-fundamentals)
* [Core Services](#-2-core-services)
* [Security](#-3-security)
* [Azure Storage](#-4-azure-storage)
* [Networking](#-5-networking)
* [Monitoring](#-6-monitoring)
* [Databases](#-7-databases)
* [Architecture](#-8-architecture)
* [Migration](#-9-migration)
* [Access Management](#-10-access-management)
* [Performance Management](#-11-performance-management)
* [Troubleshooting](#-12-troubleshooting)
* [Application Development](#-13-application-development)
* [DevOps](#-14-devops)
* [Cost Management](#-15-cost-management)
* [AI Services](#-16-ai-services)
* [Miscellaneous](#-17-miscellaneous)
* [General Questions](#-18-general-questions)
* [Azure Multiple Choice Questions](azure-mcq.md)

<br/>

## # 1. AZURE FUNDAMENTALS

<br/>

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
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Describe the shared responsibility model in Azure.

The shared responsibility model defines which security tasks are handled by Microsoft (cloud provider) and which are the customer\'s responsibility. The boundary shifts depending on the service model.

```
                    Customer Responsibility →
|----------------------------------------------------------------------|
|                | On-Prem | IaaS    | PaaS    | SaaS    |
|----------------|---------|---------|---------|---------|
| Data           | ✅ You  | ✅ You  | ✅ You  | ✅ You  |
| Identity/Access | ✅ You  | ✅ You  | ✅ You  | ✅ You  |
| Application    | ✅ You  | ✅ You  | ✅ You  | 🔵 MS   |
| OS & Runtime   | ✅ You  | ✅ You  | 🔵 MS   | 🔵 MS   |
| Networking     | ✅ You  | Shared  | 🔵 MS   | 🔵 MS   |
| Physical/DC    | ✅ You  | 🔵 MS   | 🔵 MS   | 🔵 MS   |
```

**Key principle:** Microsoft always owns physical security, hardware, and the hypervisor. The customer always owns their data and identity configuration regardless of service type. For IaaS (VMs), the customer patches the OS. For PaaS (App Service), Microsoft patches the OS and runtime.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 2. CORE SERVICES

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is Azure Service Bus and when would you use it over Azure Storage Queues?

**Azure Service Bus** is an enterprise messaging broker supporting queues (point-to-point) and topics/subscriptions (publish-subscribe), with features like message sessions, dead-letter queues, duplicate detection, and transactions.

| Feature | Service Bus | Storage Queues |
|---------|-------------|----------------|
| Max message size | 100 MB (Premium) | 64 KB |
| Message ordering | Sessions (FIFO) | Best-effort |
| Dead-letter queue | ✅ Built-in | ❌ |
| Publish-Subscribe | ✅ Topics | ❌ |
| Transactions | ✅ | ❌ |
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 3. SECURITY

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 4. AZURE STORAGE

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 5. NETWORKING

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Load Balancer and Azure Application Gateway?

| Aspect | Azure Load Balancer | Azure Application Gateway |
|--------|--------------------|-----------------------------|
| OSI Layer | Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| Routing | IP + Port hash | URL path, host header, cookies |
| SSL Termination | ❌ | ✅ |
| WAF | ❌ | ✅ (WAF SKU) |
| Health probes | TCP/HTTP | HTTP/HTTPS |
| Session affinity | IP-based | Cookie-based |
| Autoscaling | ❌ (Standard) | ✅ (v2) |
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## # 6. MONITORING

<br/>

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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 7. DATABASES

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
| SQL Agent | ✅ (Managed Instance only for full Agent) | ✅ |
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 8. ARCHITECTURE

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the difference between Azure Service Bus queues, Event Grid, and Event Hubs?

| Aspect | Service Bus Queues/Topics | Event Grid | Event Hubs |
|--------|--------------------------|------------|------------|
| Pattern | Message broker (push-pull) | Event routing (push-push) | Event streaming |
| Ordering | Sessions (FIFO) | No ordering | Within partition |
| Retention | Up to 14 days | 24 hours retry | 1–90 days |
| Throughput | Millions/sec (Premium) | 10M events/sec | Millions events/sec |
| Dead-letter | ✅ | ❌ | ❌ |
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 9. MIGRATION

<br/>

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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## # 10. ACCESS MANAGEMENT

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 11. PERFORMANCE MANAGEMENT

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 12. TROUBLESHOOTING

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 13. APPLICATION DEVELOPMENT

<br/>

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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 14. DEVOPS

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 15. COST MANAGEMENT

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 16. AI SERVICES

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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

<div align="right">
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 17. MISCELLANEOUS

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

## # 18. GENERAL QUESTIONS

<br/>

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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
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
    <b><a href="#">↥ back to top</a></b>
</div>

# Azure Scenario-Based MCQ

> Scenario-based multiple choice questions covering core Microsoft Azure topics.

<br>

## Table of Contents

1. [Azure Fundamentals](#1-azure-fundamentals)
2. [Compute Services](#2-compute-services)
3. [Azure Storage](#3-azure-storage)
4. [Networking](#4-networking)
5. [Security and Identity](#5-security-and-identity)
6. [Databases](#6-databases)
7. [Monitoring and Diagnostics](#7-monitoring-and-diagnostics)
8. [Azure App Service and Functions](#8-azure-app-service-and-functions)
9. [Containers and Kubernetes](#9-containers-and-kubernetes)
10. [DevOps and CI/CD](#10-devops-and-cicd)
11. [Cost Management](#11-cost-management)
12. [High Availability and Disaster Recovery](#12-high-availability-and-disaster-recovery)
13. [Migration](#13-migration)
14. [AI and Cognitive Services](#14-ai-and-cognitive-services)
15. [Governance and Compliance](#15-governance-and-compliance)
16. [AZ-900: Azure Fundamentals Certification](#16-az-900-azure-fundamentals-certification)
17. [AZ-104: Azure Administrator Certification](#17-az-104-azure-administrator-certification)
18. [AZ-204: Azure Developer Certification](#18-az-204-azure-developer-certification)
19. [AZ-305: Azure Solutions Architect Certification](#19-az-305-azure-solutions-architect-certification)
20. [AZ-400: Azure DevOps Engineer Certification](#20-az-400-azure-devops-engineer-certification)

<br>

## 1. Azure Fundamentals

**Q.** A startup wants to host a web application on Azure. The development team is new to Azure and wants to avoid managing virtual machines, OS patching, or server scaling. Which service model best fits their requirement?

- A) Infrastructure as a Service (IaaS)
- B) Platform as a Service (PaaS)
- C) Software as a Service (SaaS)
- D) Function as a Service (FaaS)

> **Answer: B**  
> PaaS abstracts the underlying infrastructure (VMs, OS, networking) so developers focus only on application code and data. Azure App Service is a prime example. IaaS (Azure VMs) requires OS management; SaaS is a ready-made application (like Microsoft 365); FaaS (Azure Functions) is event-driven serverless, a subset of PaaS.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company deploys resources across three Azure subscriptions for Dev, Test, and Production. The security team needs to apply the same Azure Policy across all three subscriptions from a single place. What is the most efficient approach?

- A) Assign the policy to each subscription individually.
- B) Create a Management Group that contains all three subscriptions, then assign the policy to the Management Group.
- C) Use Azure Blueprints on each subscription separately.
- D) Tag all resources and create a policy based on tags.

> **Answer: B**  
> Management Groups are a governance scope above subscriptions. Policies assigned to a Management Group automatically apply to all subscriptions and resource groups beneath it, eliminating per-subscription repetition.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer is choosing between an Azure Region and an Azure Availability Zone for deploying a mission-critical application. What is the primary difference?

- A) Regions are free; Availability Zones cost extra.
- B) A Region is a geographic area with multiple datacenters; Availability Zones are physically separate datacenters within the same region, each with independent power, cooling, and networking.
- C) Availability Zones are located in different countries; Regions are in the same country.
- D) Regions provide disaster recovery; Availability Zones provide load balancing only.

> **Answer: B**  
> An Azure Region is a set of datacenters deployed within a latency-defined perimeter. Within each region, Availability Zones are physically isolated datacenters connected by high-speed private fiber. Deploying across AZs protects against single-datacenter failures within the same region.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Your team uses Azure Resource Manager (ARM) templates to deploy infrastructure. A colleague says the same template is deployed to three environments (dev, staging, prod) but with different parameter files. After a production deployment, you discover the wrong parameter file was used. Which ARM feature helps prevent this mistake in the future?

- A) Use linked templates stored in different storage containers.
- B) Use Azure Blueprints to lock templates per environment.
- C) Use deployment stacks with deny assignments to prevent manual overrides, and integrate the parameter file selection into a CI/CD pipeline with environment-specific steps.
- D) Use resource tags to separate environments.

> **Answer: C**  
> Deployment stacks manage a set of resources as a unit and can apply deny assignments to prevent out-of-band changes. Integrating parameter file selection into a CI/CD pipeline (e.g., Azure DevOps environment stages) eliminates manual file selection errors.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An organization has two Azure subscriptions managed under the same Microsoft Entra tenant. Resources in Subscription A need to communicate with resources in Subscription B. Which statement about cross-subscription resource access is correct?

- A) Resources in different subscriptions cannot communicate because subscriptions are fully isolated networks.
- B) Cross-subscription communication requires a separate Azure ExpressRoute circuit per subscription.
- C) Virtual Network Peering and Role-Based Access Control can be configured across subscriptions within the same tenant, allowing secure resource sharing.
- D) You must merge both subscriptions into one to allow cross-subscription resource access.

> **Answer: C**  
> VNet Peering supports cross-subscription connectivity within the same Entra tenant. RBAC can grant principals from one subscription access to resources in another. Subscriptions share the same identity plane within a tenant.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 2. Compute Services

**Q.** A company runs a batch processing workload that is highly CPU-intensive and runs once a day for about 2 hours. The workload is stateless and can tolerate interruptions. Which Azure compute option gives the lowest cost?

- A) Azure Virtual Machines (pay-as-you-go)
- B) Azure Spot Virtual Machines
- C) Azure Reserved Virtual Machine Instances (1-year)
- D) Azure Dedicated Hosts

> **Answer: B**  
> Azure Spot VMs use unused Azure capacity at up to 90% discount. Since the workload is stateless and can tolerate eviction (it can be restarted), Spot VMs are the cheapest option. Reserved Instances save money for always-on workloads, not sporadic 2-hour jobs. Dedicated Hosts are the most expensive option.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A web application is deployed across 10 Azure Virtual Machines behind an Azure Load Balancer. During a planned maintenance event, Azure reboots two VMs simultaneously. The application becomes unavailable. What configuration would have prevented this?

- A) Use a larger VM size so fewer VMs are needed.
- B) Place the VMs in an Availability Set, which groups VMs into fault domains and update domains.
- C) Use Azure Traffic Manager with geographic routing.
- D) Enable VM auto-shutdown schedules.

> **Answer: B**  
> Availability Sets place VMs across multiple update domains (UDs) and fault domains (FDs). During planned maintenance, Azure respects UD boundaries — only one UD is rebooted at a time — ensuring at least some VMs remain online.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An e-commerce platform experiences a 5x traffic spike every Black Friday. The team wants the application to automatically scale out additional web server VMs when CPU exceeds 70% and scale in when it drops below 30%, without manual intervention. Which Azure feature enables this?

- A) Azure Autoscale on a Virtual Machine Scale Set (VMSS)
- B) Azure Traffic Manager with performance routing
- C) Azure Load Balancer health probes
- D) Azure Advisor right-sizing recommendations

> **Answer: A**  
> Virtual Machine Scale Sets with Azure Autoscale rules allow automatic horizontal scaling based on metrics (CPU, memory, queue depth, custom metrics). Traffic Manager and Load Balancer distribute traffic but do not add or remove VMs. Azure Advisor provides recommendations but does not auto-scale.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs to run a one-off data transformation script that takes 45 minutes. The script is containerized. The team does not want to manage VM infrastructure, and the job should be billed only for the duration it runs. Which Azure service is most appropriate?

- A) Azure Kubernetes Service (AKS)
- B) Azure Container Instances (ACI)
- C) Azure App Service (container deployment)
- D) Azure Virtual Machines with Docker installed

> **Answer: B**  
> Azure Container Instances runs containers on-demand with per-second billing, no cluster management, and no idle costs. AKS requires a node pool that incurs costs whether or not workloads are running. App Service is for long-running web apps. VMs require manual OS management.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Your company\'s on-premises application writes files to a local network share and reads them back sequentially. You are lifting-and-shifting this application to Azure VMs. Which Azure storage service should replace the on-premises network share?

- A) Azure Blob Storage
- B) Azure Files (SMB share)
- C) Azure Table Storage
- D) Azure Queue Storage

> **Answer: B**  
> Azure Files provides fully managed SMB and NFS file shares accessible from Azure VMs and on-premises machines. Applications that rely on the file system path and SMB protocol work without code changes. Blob Storage is object storage accessed via HTTP/SDK, not via file system paths.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 3. Azure Storage

**Q.** A media company stores large video files that are accessed frequently for the first 30 days after upload and then rarely accessed afterward. They want to minimize storage costs automatically without deleting the files. What Azure Storage feature should they configure?

- A) Azure Blob Storage with Lifecycle Management policies to transition blobs from Hot to Cool (or Archive) tier after 30 days.
- B) Azure Queue Storage with time-to-live (TTL) settings.
- C) Azure Table Storage with automatic data expiry.
- D) Azure File Sync with cloud tiering enabled.

> **Answer: A**  
> Blob Storage Lifecycle Management policies can automatically transition blobs between Hot, Cool, Cold, and Archive tiers (or delete them) based on the number of days since last modification or creation. This minimizes cost without manual intervention.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An auditing requirement states that files stored in Azure Blob Storage must not be modified or deleted for 7 years after creation. Which feature enforces this at the storage level?

- A) Azure Storage firewall with IP restrictions
- B) Soft delete for blobs
- C) Immutable Blob Storage with a time-based retention policy (WORM)
- D) Azure Backup for storage accounts

> **Answer: C**  
> Immutable Blob Storage with a locked time-based retention policy implements WORM (Write Once, Read Many) compliance. Once locked, even subscription admins cannot delete or overwrite blobs until the retention period expires. Soft delete only protects against accidental deletion with a short window.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs to grant temporary read access to a private blob in Azure Storage to an external partner for exactly 24 hours, without making the container public or sharing storage account keys. What is the correct approach?

- A) Change the container access level to "Blob (anonymous read)" and revert it after 24 hours.
- B) Generate a Shared Access Signature (SAS) token with read permission and an expiry of 24 hours.
- C) Share the storage account access key and ask the partner to delete it after use.
- D) Use Azure AD guest user access and assign Storage Blob Data Reader role.

> **Answer: B**  
> A SAS token grants scoped, time-limited access to specific resources without exposing the account key. It can be restricted to read-only, specific containers/blobs, and a 24-hour window. Sharing the account key grants full storage account access and cannot be scoped.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A storage account in East US replicates data to West US. During a regional disaster in East US, the secondary region in West US becomes read-only. The operations team needs to restore full read/write access as quickly as possible. Which replication option supports a customer-initiated failover to make the secondary region the new primary?

- A) Locally Redundant Storage (LRS)
- B) Zone-Redundant Storage (ZRS)
- C) Geo-Redundant Storage (GRS) with customer-managed failover
- D) Read-Access Geo-Redundant Storage (RA-GRS) — failover is not possible in this tier.

> **Answer: C**  
> GRS (and RA-GRS) replicate data to a secondary region. Microsoft now supports customer-initiated account failover, which promotes the secondary to primary for read-write access. LRS and ZRS do not replicate to a secondary region.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company wants to store semi-structured session data for millions of users with fast key-value lookups, minimal operational overhead, and automatic scaling. Which Azure storage service is most appropriate?

- A) Azure SQL Database
- B) Azure Table Storage
- C) Azure Blob Storage
- D) Azure Files

> **Answer: B**  
> Azure Table Storage is a NoSQL key-value store designed for fast, cost-effective storage of large amounts of semi-structured data. It scales automatically and is accessed via a simple REST/SDK API. For even more advanced NoSQL features, Azure Cosmos DB would be considered, but Table Storage meets the stated requirements with minimal cost.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 4. Networking

**Q.** Two Azure Virtual Networks in different regions need to communicate privately without traffic going over the public internet. Each VNet has non-overlapping address spaces. What should you configure?

- A) Azure VPN Gateway with Site-to-Site connection
- B) Azure ExpressRoute between the two VNets
- C) Global VNet Peering
- D) Azure Traffic Manager with private endpoints

> **Answer: C**  
> Global VNet Peering connects VNets in different Azure regions over Microsoft\'s backbone network — not the public internet — with low latency and no bandwidth limits beyond subscription quotas. VPN Gateway introduces encryption overhead and uses public IPs. ExpressRoute connects on-premises to Azure, not VNet-to-VNet.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An application tier (VMs) must only accept inbound traffic from the web tier subnet (10.0.1.0/24) on port 443, and deny all other inbound traffic. Which Azure resource enforces this rule?

- A) Azure Firewall with DNAT rules
- B) A Network Security Group (NSG) attached to the application subnet with an inbound rule allowing TCP 443 from 10.0.1.0/24 and a lower-priority deny-all rule.
- C) Azure DDoS Protection Standard
- D) Route Table with a blackhole route for all other traffic

> **Answer: B**  
> NSGs are stateful packet filters applied at the subnet or NIC level. An inbound rule with source `10.0.1.0/24`, destination port `443`, action `Allow`, and a lower-priority `Deny` catch-all rule achieves the required segmentation. Azure Firewall operates at a higher level and is used for hub-spoke architectures. DDoS Protection defends against volumetric attacks, not access control.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company is migrating from on-premises to Azure and requires a dedicated, private connection with consistent bandwidth (not over the public internet) between their datacenter and Azure, with 99.95% SLA. Which service should they use?

- A) Azure VPN Gateway (Site-to-Site IPsec)
- B) Azure ExpressRoute
- C) Azure Bastion
- D) Azure Virtual WAN

> **Answer: B**  
> Azure ExpressRoute provides a private, dedicated connection to Azure through a connectivity provider, bypassing the public internet. It offers up to 100 Gbps bandwidth, consistent latency, and a 99.95% SLA. VPN Gateway uses encrypted tunnels over the public internet and has lower guaranteed bandwidth.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A web application hosted in Azure needs to distribute HTTPS traffic across multiple backend VMs based on URL path (`/api/*` → API servers, `/static/*` → CDN servers). Layer 7 SSL termination is also required. Which Azure load-balancing service should be used?

- A) Azure Load Balancer (Standard)
- B) Azure Traffic Manager
- C) Azure Application Gateway
- D) Azure Front Door (basic tier)

> **Answer: C**  
> Azure Application Gateway is a Layer 7 load balancer that supports URL path-based routing, SSL termination, Web Application Firewall (WAF), session affinity, and header rewrites. Azure Load Balancer operates at Layer 4 (TCP/UDP) and cannot inspect URLs. Traffic Manager uses DNS-based routing and does not terminate SSL.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company deploys Azure VMs without public IP addresses in a locked-down VNet. Operations staff needs to RDP and SSH into these VMs securely without opening inbound ports 3389/22 to the internet. Which Azure service enables this?

- A) Azure VPN Gateway with Point-to-Site VPN
- B) Azure Bastion
- C) Just-In-Time (JIT) VM access via Microsoft Defender for Cloud
- D) Azure Private Link

> **Answer: B**  
> Azure Bastion provides secure, browser-based RDP and SSH access to VMs directly in the Azure portal over TLS port 443, without requiring a public IP or opening RDP/SSH ports to the internet. JIT VM access temporarily opens ports on-demand but does expose them to specific IPs, while Bastion never exposes those ports.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 5. Security and Identity

**Q.** A developer\'s application running on an Azure VM needs to access secrets stored in Azure Key Vault. The team wants to avoid storing any credentials (client secrets or certificates) in the application code or configuration. What is the recommended approach?

- A) Store the Key Vault access key in an environment variable on the VM.
- B) Create a service principal and embed its client secret in the application\'s `appsettings.json`.
- C) Assign a System-Assigned Managed Identity to the VM and grant it the Key Vault Secrets User role.
- D) Use a shared access signature (SAS) token to authenticate to Key Vault.

> **Answer: C**  
> Managed Identities eliminate the need to manage credentials. A system-assigned managed identity creates an Azure AD identity for the VM; the application acquires a token from the Instance Metadata Service (IMDS) endpoint automatically. RBAC then grants that identity access to Key Vault. No secrets are stored anywhere in the code or config.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A security audit finds that a junior developer was accidentally assigned the Owner role on the production subscription 3 months ago. The role was never used. Your task is to prevent such excessive privilege assignments in the future using the principle of least privilege. Which combination of features best addresses this?

- A) Use Azure Policy to deny Owner role assignments at the subscription scope; use Azure AD Privileged Identity Management (PIM) for time-bound, approval-required role activations.
- B) Create a custom role with all permissions and assign it to everyone.
- C) Enable Multi-Factor Authentication (MFA) for all users.
- D) Move all resources to a separate subscription so junior developers cannot access them.

> **Answer: A**  
> Azure Policy can deny broad role assignments (like Owner) at sensitive scopes. PIM enforces just-in-time access: users must request activation of privileged roles, require approval, and the access auto-expires. Together these enforce least privilege and provide a full audit trail. MFA alone does not restrict what users can do once authenticated.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A financial services company requires that any user accessing their Azure-hosted banking application from outside corporate networks must complete MFA, while users on the corporate network are trusted. Which Azure feature implements this location-based conditional access?

- A) Azure AD Password Protection
- B) Microsoft Entra ID Conditional Access with Named Locations
- C) Azure Firewall application rules
- D) Microsoft Defender for Identity

> **Answer: B**  
> Conditional Access policies in Microsoft Entra ID can evaluate sign-in conditions including network location (Named Locations defined by IP ranges). A policy granting access only after MFA when the sign-in is not from the corporate IP range implements the requirement. Firewall rules operate at the network layer and do not integrate with identity-aware policies.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Your application uses a connection string to connect to Azure SQL Database. This connection string is currently stored in `appsettings.json` committed to Git. The security team flags this as a critical vulnerability. What is the correct remediation?

- A) Base64-encode the connection string before committing to Git.
- B) Move the connection string to an environment variable on the build server.
- C) Store the connection string in Azure Key Vault and have the application retrieve it at startup using a Managed Identity.
- D) Encrypt `appsettings.json` with a symmetric key stored in the same repository.

> **Answer: C**  
> Azure Key Vault is the dedicated secret store for connection strings, API keys, and certificates. Combined with a Managed Identity, the application fetches the secret at runtime with no credentials embedded in code or Git history. Base64 encoding is not encryption. Storing secrets in environment variables on build servers is an improvement but not the recommended Azure pattern.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team wants to audit all operations performed on an Azure Key Vault — specifically who accessed which secret and when — for compliance purposes. Which service provides this audit trail?

- A) Azure Monitor Metrics
- B) Azure Key Vault Diagnostic Logs sent to a Log Analytics workspace
- C) Microsoft Defender for Key Vault alerts only
- D) Azure Activity Log at the subscription level

> **Answer: B**  
> Key Vault Diagnostic Logs (specifically the `AuditEvent` category) record every data-plane operation: who accessed which secret, key, or certificate, and at what time. These logs are sent to a Log Analytics workspace, Event Hub, or Storage Account. The Activity Log records control-plane operations (create/delete vault) but not secret access.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 6. Databases

**Q.** An application needs a globally distributed database with single-digit millisecond reads, multiple write regions, and five consistency level options from strong to eventual. Which Azure database service should you choose?

- A) Azure SQL Database Hyperscale
- B) Azure Database for PostgreSQL — Flexible Server
- C) Azure Cosmos DB
- D) Azure Cache for Redis

> **Answer: C**  
> Azure Cosmos DB is a globally distributed, multi-model NoSQL database with turnkey multi-region writes, five consistency levels (Strong, Bounded Staleness, Session, Consistent Prefix, Eventual), and guaranteed single-digit millisecond latency at the 99th percentile. Azure SQL Database is relational and does not offer multi-region writes natively.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company\'s Azure SQL Database experiences performance degradation during month-end reporting. The reports involve complex analytical queries over 3 years of transactional data. The OLTP workload must not be impacted. What is the recommended architectural solution?

- A) Upgrade the Azure SQL Database to a Business Critical tier.
- B) Add an Azure Cache for Redis layer in front of the SQL Database.
- C) Use Azure Synapse Analytics (dedicated SQL pool) for the analytical queries, and replicate data from Azure SQL via Synapse Link or ADF pipelines.
- D) Use Read Replicas on Azure SQL Database and run reports on the replica.

> **Answer: C**  
> Separating OLTP (Azure SQL) from OLAP (Azure Synapse Analytics) is the recommended pattern. Synapse is optimized for massive parallel analytical queries. Azure Synapse Link for SQL provides near-real-time data replication from Azure SQL to Synapse without impacting the OLTP workload. Read replicas reduce read load on the primary but don\'t provide the analytical query performance of a columnar MPP engine.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A SaaS platform serves 500 tenants, each with their own Azure SQL Database. Managing 500 individual databases is operationally expensive. The databases have variable and unpredictable usage patterns — some are busy while others are mostly idle. What Azure SQL feature reduces cost and management overhead in this scenario?

- A) Azure SQL Managed Instance
- B) Azure SQL Elastic Pools
- C) SQL Server on Azure VM
- D) Azure SQL Database Hyperscale

> **Answer: B**  
> Elastic Pools allow multiple databases to share a pool of eDTUs or vCores. Databases with different peak times share resources cost-effectively — idle databases consume minimal resources while busy ones burst. This is the canonical multi-tenant SaaS architecture for Azure SQL.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An application queries an Azure SQL Database for the same product catalog data millions of times per day. The catalog changes once per hour. Database CPU and DTU consumption is high despite the data being mostly static. What is the most effective optimization?

- A) Scale up the Azure SQL Database to a higher service tier permanently.
- B) Add a read replica and route all reads to it.
- C) Place Azure Cache for Redis in front of the database; cache product catalog results with a 1-hour TTL.
- D) Enable Query Store on Azure SQL Database to identify slow queries.

> **Answer: C**  
> Caching static or slow-changing data in Redis dramatically reduces database load. With a 1-hour TTL matching the catalog update frequency, the application serves millions of reads from in-memory cache at sub-millisecond latency, reducing SQL DTU consumption significantly. Scaling up permanently is expensive for a caching problem.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Your team is migrating an on-premises SQL Server 2019 instance with SQL Agent jobs, linked servers, and CLR objects to Azure. The requirement is minimal code changes and full SQL Server compatibility. Which Azure SQL option should you choose?

- A) Azure SQL Database (single database)
- B) Azure SQL Database Hyperscale
- C) Azure SQL Managed Instance
- D) SQL Server on Azure Virtual Machines

> **Answer: C**  
> Azure SQL Managed Instance provides near-100% compatibility with SQL Server on-premises, including SQL Agent, CLR, linked servers, cross-database queries, Service Broker, and Database Mail — features not available in Azure SQL Database. SQL Server on Azure VM provides full SQL Server control but requires OS management.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 7. Monitoring and Diagnostics

**Q.** An operations team receives alerts about an Azure App Service application being slow, but by the time they investigate, the performance data is already gone. What should they configure to persist performance data for retrospective analysis?

- A) Enable Application Insights with a 90-day retention period and configure Continuous Export to Azure Storage.
- B) Increase the App Service plan to a Premium tier.
- C) Enable Azure Service Health alerts.
- D) Configure Azure Monitor Metrics with a 5-minute granularity.

> **Answer: A**  
> Application Insights collects detailed telemetry (requests, dependencies, exceptions, performance counters). With a retention period up to 90 days (730 days on a workspace) and Continuous Export (or Diagnostic Settings to Log Analytics), all data persists for retrospective investigation. Azure Monitor Metrics retains 93 days but without the rich application-level detail of Application Insights.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer writes the following KQL query in Log Analytics:

```kql
AzureActivity
| where OperationNameValue contains "delete"
| where ActivityStatusValue == "Success"
| project TimeGenerated, Caller, ResourceGroup, OperationNameValue
| order by TimeGenerated desc
```

What does this query return?

- A) All failed delete operations across all Azure resources.
- B) A list of successful delete operations sorted by most recent first, showing who performed them and on which resource group.
- C) All operations performed by a specific caller in the last 24 hours.
- D) All Azure activity log entries containing errors.

> **Answer: B**  
> The query filters the `AzureActivity` table for rows where the operation name includes "delete" and the status is "Success". It projects four columns and orders results newest-first, giving an audit trail of successful delete operations.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An SRE team wants to be notified by SMS and email within 5 minutes if the average CPU of their Azure VM exceeds 90% for more than 3 minutes. What Azure Monitor components should they configure?

- A) A Service Health alert with a webhook to send an email.
- B) A Metric Alert on the VM\'s `Percentage CPU` metric with a 3-minute evaluation window, threshold of 90%, and an Action Group configured with email and SMS contacts.
- C) A Log Analytics scheduled query that runs every 5 minutes and sends an email.
- D) Azure Advisor cost recommendations with email notifications enabled.

> **Answer: B**  
> Azure Monitor Metric Alerts evaluate metric conditions at near-real-time. A metric alert on `Percentage CPU > 90%` with a 3-minute aggregation window triggers an Action Group, which sends notifications via email, SMS, voice call, webhook, or Logic Apps. Log Analytics alerts have higher latency due to ingestion delays.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A microservices application spans Azure Functions, Azure Service Bus, Azure SQL, and Azure App Service. The support team cannot trace a slow customer request end-to-end across all services. Which Application Insights feature provides distributed tracing across all these components?

- A) Application Insights Availability Tests
- B) Application Insights Application Map and distributed tracing with correlation IDs
- C) Azure Monitor Workbooks
- D) Azure Network Watcher Connection Monitor

> **Answer: B**  
> Application Insights propagates correlation IDs (W3C TraceContext standard) across service calls. The Application Map visualizes the entire call chain and highlights slow or failing dependencies. Each service (Functions, App Service) is instrumented with the Application Insights SDK or auto-instrumentation, enabling end-to-end distributed tracing.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company wants to verify that their Azure-hosted e-commerce website is available and responding correctly from locations across Europe, Asia, and the Americas every 5 minutes, and receive an alert if availability drops below 99%. Which Azure Monitor feature enables this?

- A) Azure Service Health
- B) Application Insights Availability Tests (Standard or Multi-step tests)
- C) Azure Monitor Metrics — HTTP response time metric
- D) Azure Front Door health probes

> **Answer: B**  
> Application Insights Availability Tests (URL ping, Standard, or Multi-step) run synthetic HTTP requests from Azure test nodes worldwide at configurable intervals. Alerts trigger when availability drops below the configured threshold. Service Health reports Azure platform issues, not application-level availability from multiple geographic vantage points.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 8. Azure App Service and Functions

**Q.** A team wants to deploy a new version of their App Service web application for testing by 10% of production traffic, while the remaining 90% continues to use the stable version. No infrastructure changes should be required. Which feature achieves this?

- A) Azure Traffic Manager weighted routing
- B) App Service Deployment Slots with Traffic Routing percentage
- C) Azure Application Gateway with path-based routing
- D) Azure Front Door with origin groups

> **Answer: B**  
> App Service Deployment Slots allow traffic splitting between named slots (e.g., production and staging). The Traffic Routing percentage setting directs a configurable fraction of requests to the staging slot, enabling canary releases and A/B testing with a single App Service plan — no extra infrastructure.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An Azure Function is triggered by messages on an Azure Service Bus queue. During peak load, 10,000 messages arrive per minute. The function takes 2 seconds to process each message. The team is concerned about throughput and scaling. Which hosting plan supports unlimited automatic scale-out at no fixed cost for compute?

- A) App Service Plan (Standard S2)
- B) Azure Functions Consumption Plan
- C) Azure Functions Premium Plan
- D) Azure Functions Dedicated (App Service) Plan

> **Answer: B**  
> The Consumption Plan scales out automatically — adding new instances based on queue depth — with no fixed monthly charge for compute. You pay only per execution and GB-seconds of memory used. The Premium Plan also auto-scales but includes a minimum warm instance with a fixed cost. App Service Plan requires manual or autoscale configuration within fixed plan limits.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An Azure Function using the Consumption plan starts processing a large file upload but fails intermittently after 5 minutes with a timeout error. What is the most likely cause, and what is the solution?

- A) The function ran out of memory. Increase the memory allocation in `host.json`.
- B) The Consumption plan has a maximum execution timeout of 5 minutes (default); increase it to up to 10 minutes in `host.json`, or migrate to the Premium or Dedicated plan for longer executions.
- C) The Azure Storage trigger has a polling delay. Switch to an Event Grid trigger.
- D) The function is not retrying on failure. Enable retry policies in `host.json`.

> **Answer: B**  
> The default timeout for Azure Functions on the Consumption plan is 5 minutes, configurable up to 10 minutes via `functionTimeout` in `host.json`. For operations exceeding 10 minutes, the Premium or Dedicated plan (with no timeout limit, or up to 60 minutes configured) is required. The Durable Functions pattern (async orchestration) is the recommended approach for very long-running workflows.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An App Service web application needs to read a secret from Azure Key Vault without storing credentials. The application is deployed in a Standard tier App Service. What is the simplest way to achieve this?

- A) Add the Key Vault access key to the App Service Application Settings.
- B) Enable the System-Assigned Managed Identity on the App Service, grant it Key Vault Secrets User RBAC, and reference the secret via a Key Vault reference in Application Settings: `@Microsoft.KeyVault(SecretUri=...)`.
- C) Download the secret from Key Vault during the CI/CD pipeline and inject it as a build artifact.
- D) Use Azure API Management to proxy Key Vault requests from the App Service.

> **Answer: B**  
> App Service supports Key Vault references natively. When a Managed Identity is enabled and has the Key Vault Secrets User role, you set an App Setting value to `@Microsoft.KeyVault(SecretUri=https://vault.vault.azure.net/secrets/mySecret/)`. The App Service runtime resolves the secret at startup automatically — no SDK code required.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team uses Azure Durable Functions to orchestrate a multi-step workflow: Step 1 calls an external payment API, Step 2 sends a confirmation email, and Step 3 updates a database. The external payment API is occasionally slow (up to 3 minutes). How does Durable Functions handle this without blocking a thread?

- A) Durable Functions runs each step on a separate thread pool with a 5-minute timeout per thread.
- B) The orchestrator function suspends at each `await` call and persists its state to Azure Storage. The thread is freed. When the activity completes, the orchestrator replays to resume from the saved checkpoint.
- C) Durable Functions allocates a dedicated VM for each orchestration instance.
- D) The orchestrator polls the activity function every 30 seconds until it completes.

> **Answer: B**  
> Durable Functions uses an event sourcing / checkpointing pattern. When the orchestrator awaits an activity, it serializes its state to Azure Storage and releases the thread. When the activity completes, the runtime replays the orchestrator function from the beginning, fast-forwarding through completed steps using the event history, until it reaches the next pending await. This enables long-running workflows without holding threads.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 9. Containers and Kubernetes

**Q.** A DevOps team has just deployed a new container image to their AKS cluster. After the deployment, some pods are crashing with status `CrashLoopBackOff`. What is the first command they should run to diagnose the issue?

- A) `kubectl delete pod <pod-name>`
- B) `kubectl logs <pod-name> --previous`
- C) `kubectl scale deployment <name> --replicas=0`
- D) `az aks upgrade --kubernetes-version`

> **Answer: B**  
> `kubectl logs <pod-name> --previous` retrieves the logs from the previous (crashed) container instance, which usually contains the error that caused the crash. `kubectl describe pod <pod-name>` also reveals events and resource constraints. Deleting the pod restarts it but loses current diagnostic data.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An AKS-hosted microservice needs to autoscale the number of pods based on the number of messages in an Azure Service Bus queue — not based on CPU or memory. Which AKS feature supports this?

- A) Horizontal Pod Autoscaler (HPA) with a custom CPU metric
- B) Vertical Pod Autoscaler (VPA)
- C) KEDA (Kubernetes Event-Driven Autoscaling) with an Azure Service Bus scaler
- D) AKS Cluster Autoscaler

> **Answer: C**  
> KEDA is a CNCF project integrated into AKS that scales pods based on external event sources including Azure Service Bus queue/topic depth, Azure Storage queues, Event Hubs, and 50+ other scalers. HPA can only scale on CPU/memory or custom Kubernetes metrics, not external Azure queue depth natively.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company stores container images in Azure Container Registry (ACR). Only the AKS cluster should be able to pull images from this private registry, with no long-lived credentials stored in the cluster. What is the recommended configuration?

- A) Create a Kubernetes `imagePullSecret` with the ACR admin username and password.
- B) Make the ACR registry public so no credentials are needed.
- C) Attach the ACR to the AKS cluster using `az aks update --attach-acr`, which grants the cluster\'s managed identity the AcrPull role on the ACR.
- D) Store the ACR access token in an Azure Key Vault secret and reference it in pod specs.

> **Answer: C**  
> `az aks update --attach-acr <acr-name>` grants the AKS cluster\'s managed identity (kubelet identity) the `AcrPull` role on the ACR. Pods can then pull images without any imagePullSecret. No passwords or tokens are stored in the cluster. Admin credentials in imagePullSecrets are a security anti-pattern.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Your AKS workload processes sensitive financial data. A security requirement states that no pod should run as root, all file systems should be read-only unless explicitly required, and privilege escalation must be blocked. Which Kubernetes mechanism enforces these constraints cluster-wide?

- A) Kubernetes Network Policies
- B) Azure Policy for AKS with built-in Pod Security Standards (Restricted profile)
- C) AKS node pool taints
- D) Resource Quotas and LimitRanges

> **Answer: B**  
> Azure Policy for AKS integrates with the OPA Gatekeeper admission controller to enforce Pod Security Standards at the cluster level. The Restricted profile enforces: no root users, read-only root filesystems, no privilege escalation, no host namespaces. Network Policies control network traffic, not container security context. Taints control pod scheduling placement, not security posture.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team wants to minimize AKS node pool costs for a batch workload that runs for 4 hours each night. The nodes should not exist (and not incur cost) outside of the batch window. Which AKS feature supports this?

- A) AKS Cluster Autoscaler with a minimum node count of 0
- B) AKS node pool with `--node-count 0` and start/stop the cluster via `az aks start/stop`
- C) Use Azure Container Instances for the batch workload instead of AKS.
- D) AKS Spot node pools with automatic eviction

> **Answer: B**  
> AKS supports stopping and starting the entire cluster or individual user node pools. When stopped, you pay only for the persistent storage (OS disks, etcd) but not for VM compute. Combined with a scheduled automation (Logic Apps or Azure Automation), the cluster starts before the batch window and stops after, eliminating idle VM costs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 10. DevOps and CI/CD

**Q.** A development team uses Azure DevOps pipelines to build and deploy a .NET application. The pipeline currently deploys directly to production on every merge to `main`. The team wants to add a manual approval gate before the production deployment. Which Azure DevOps feature implements this?

- A) Pipeline branch filters
- B) Azure DevOps Environment with a required approval check
- C) Repository branch protection rules
- D) YAML template parameters

> **Answer: B**  
> Azure DevOps Environments support pre-deployment checks including manual approvals and required reviewers. When a deployment stage targets an environment with an approval check, the pipeline pauses and sends a notification to approvers before proceeding. Branch filters control when the pipeline triggers, not deployment approval.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team\'s Azure DevOps pipeline needs to deploy to Azure using a Service Principal. Currently the Client Secret is stored as a pipeline variable in plain text. The security team requires secrets to never be stored in Azure DevOps. What is the recommended approach?

- A) Store the secret in a pipeline variable group marked as secret.
- B) Use an Azure Resource Manager Service Connection (with Workload Identity Federation / OIDC), which does not require storing a client secret in Azure DevOps at all.
- C) Rotate the client secret every 30 days automatically.
- D) Encode the client secret in Base64 before storing it in the pipeline variable.

> **Answer: B**  
> Workload Identity Federation (OIDC) for Azure DevOps ARM Service Connections uses short-lived tokens issued by Azure DevOps, validated by Microsoft Entra ID — no client secret is stored anywhere. The pipeline authenticates to Azure using federated identity without any long-lived credential. This is the current Microsoft-recommended approach.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company wants to enforce that all Azure resources deployed through their CI/CD pipeline are tagged with `CostCenter`, `Environment`, and `Owner` tags. Resources deployed without these tags should be automatically rejected. Which approach enforces this at the Azure control plane level?

- A) Add a pipeline script step that checks tags before deployment.
- B) Assign an Azure Policy with the `deny` effect that requires the specified tags on all resource creation/update operations.
- C) Use Azure Blueprints to tag resources after deployment.
- D) Enable Microsoft Defender for Cloud resource compliance.

> **Answer: B**  
> Azure Policy with `deny` effect blocks any resource deployment that does not include the required tags, regardless of how the deployment is triggered (portal, CLI, pipeline, Terraform). Pipeline-level checks can be bypassed by out-of-band deployments. Policy enforcement at the ARM control plane is authoritative.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team manages Azure infrastructure with Bicep templates stored in a Git repository. They want to automatically validate and deploy changes to infrastructure when a pull request is merged to `main`. Which pipeline structure is most appropriate?

- A) A single stage pipeline that runs `az deployment group create` on every commit to every branch.
- B) A multi-stage YAML pipeline: Stage 1 (PR) runs `az deployment group validate` and `az deployment group what-if`; Stage 2 (merge to main) runs `az deployment group create` with environment approval.
- C) Use Azure Blueprints to auto-deploy on repository changes.
- D) A release pipeline triggered by a build artifact containing the Bicep files.

> **Answer: B**  
> The recommended IaC pipeline pattern: validate (syntax check) and what-if (preview changes) on PRs to catch errors before merge; deploy to production after merge with an environment approval gate. This provides safety, visibility, and auditability without deploying untested changes automatically.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A GitHub Actions workflow deploys to Azure App Service on every push to `main`. The team wants the deployment to roll back automatically if the App Service health check fails after deployment. Which GitHub Actions feature supports this?

- A) GitHub Actions `continue-on-error: true` flag
- B) The `azure/webapps-deploy` action with slot swap and App Service Health Check configured on the staging slot; if the health check fails, the swap does not proceed.
- C) GitHub Dependabot auto-merge
- D) GitHub Environment protection rules with a required status check

> **Answer: B**  
> Deploying to a staging slot and using slot swap (with Auto swap or a manual swap step in the pipeline) combined with App Service Health Check means Azure validates the new version on the staging slot before swapping to production. If the health check fails, the slot remains at staging and production is unaffected — providing automatic rollback without additional tooling.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 11. Cost Management

**Q.** A company\'s Azure bill is unexpectedly 40% higher than the previous month. The finance team asks you to investigate. Which is the most direct first step?

- A) Delete all resources in non-production resource groups immediately.
- B) Open Azure Cost Management + Billing → Cost Analysis, filter by time period, group by Service and Resource Group to identify which service and resource group drove the increase.
- C) Scale down all VM sizes to the smallest available SKU.
- D) Enable Azure Advisor and wait for recommendations to appear.

> **Answer: B**  
> Cost Analysis in Azure Cost Management provides a visual breakdown of costs by service, resource group, resource, tag, and location. Grouping by service and resource group quickly identifies the source of unexpected spend. Taking destructive actions before diagnosing the root cause risks business disruption.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A production workload runs 24/7 on 10 Azure D8s_v5 VMs in East US. The workload has been stable for 18 months with no plans to change VM size or region. What is the most cost-effective purchasing option?

- A) Pay-as-you-go pricing
- B) 3-year Azure Reserved VM Instance with no upfront payment
- C) 3-year Azure Reserved VM Instance with all-upfront payment
- D) Azure Spot VMs

> **Answer: C**  
> For stable, long-running 24/7 workloads, 3-year Reserved Instances with all-upfront payment provide the maximum discount (up to 72% vs pay-as-you-go). The all-upfront option has a larger discount than the monthly payment option. Spot VMs can be evicted and are inappropriate for production workloads. Pay-as-you-go is the most expensive for constant usage.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A development team leaves 15 development VMs running overnight and on weekends when no one is working. How can you automatically shut down these VMs during off-hours to reduce cost without deleting them?

- A) Use Azure Policy to deny VM creation outside business hours.
- B) Enable Auto-shutdown on each VM (or via Azure Automation runbooks with schedules) to stop them at 7 PM on weekdays and restart at 8 AM.
- C) Move the VMs to Azure Spot to reduce cost when idle.
- D) Enable Azure Advisor and configure it to shut down idle VMs.

> **Answer: B**  
> Azure VMs support a built-in Auto-shutdown feature configurable per VM. Azure Automation with PowerShell runbooks and schedules can manage start/stop at scale across all dev VMs. Stopped (deallocated) VMs do not incur compute charges. Azure Advisor recommends but does not automatically shut down resources.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company has Windows Server 2022 VMs running SQL Server Enterprise on-premises with active Software Assurance licenses. They are migrating to Azure VMs. Which Azure benefit allows them to reuse existing licenses and avoid paying for new Azure licensing costs?

- A) Azure Hybrid Benefit
- B) Azure Reserved Instances
- C) Azure Dev/Test pricing
- D) Azure Spot pricing

> **Answer: A**  
> Azure Hybrid Benefit allows customers with active Software Assurance on Windows Server and SQL Server licenses to run those workloads on Azure VMs at a reduced rate (paying only compute, not the OS or SQL Server license again). This can save up to 85% compared to pay-as-you-go pricing with included licenses.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A FinOps team wants to alert the Finance team automatically when monthly Azure spending in the Production subscription exceeds $50,000, so corrective action can be taken before the end of the month. Which feature enables this?

- A) Azure Monitor metric alert on billing metrics
- B) Azure Cost Management Budget with an alert action group configured to email the Finance team when 80% and 100% of the $50,000 budget is reached.
- C) Azure Policy with a cost threshold deny effect
- D) Microsoft Defender for Cloud cost recommendations

> **Answer: B**  
> Azure Cost Management Budgets allow you to define a spending threshold (e.g., $50,000/month) and trigger alerts at configurable percentages (e.g., 80%, 100%) via email or action groups. Budgets also support forecast-based alerts (when Azure predicts you'll exceed the budget). Azure Policy cannot block spending; it controls resource configurations.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 12. High Availability and Disaster Recovery

**Q.** A company\'s Azure SQL Database in East US experiences an outage. They have a failover group configured with a secondary in West US. The application connection string uses the failover group listener endpoint. What happens to the application during a database failover?

- A) The application must be redeployed with the secondary database connection string.
- B) The failover group listener endpoint automatically resolves to the new primary (West US) after failover; the application reconnects without a connection string change.
- C) The application must manually trigger failover via the Azure portal.
- D) The connection string becomes invalid and must be updated with the secondary endpoint.

> **Answer: B**  
> Failover group listener endpoints (read-write and read-only) are DNS names that remain constant. After failover, the DNS record is updated to point to the new primary. Applications using the listener endpoint reconnect transparently after a brief reconnection retry — no connection string change required.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company needs their Azure-hosted application to have an RTO (Recovery Time Objective) of less than 15 minutes and RPO (Recovery Point Objective) of less than 5 minutes in the event of a full region failure. Which design best meets both objectives?

- A) Daily backup of VMs to Azure Backup in the same region.
- B) Active-passive multi-region deployment: primary region runs the workload; Azure Site Recovery continuously replicates VMs to a secondary region (RPO ~1 min); Azure Traffic Manager fails over DNS to the secondary region (RTO ~5–15 min).
- C) Weekly geo-redundant backup with manual restore in the secondary region.
- D) Azure Backup with cross-region restore (CRR) enabled.

> **Answer: B**  
> Azure Site Recovery with continuous replication achieves RPO of approximately 1 minute. Azure Traffic Manager with health probes detects the primary region failure and reroutes traffic to the pre-warmed secondary region in minutes, meeting the 15-minute RTO. Daily or weekly backups cannot meet a 5-minute RPO. Cross-region restore with Azure Backup takes hours.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer asks: "What is the difference between RTO and RPO?" Which answer is correct?

- A) RTO is the maximum data loss tolerated; RPO is the maximum downtime tolerated.
- B) RTO (Recovery Time Objective) is the maximum acceptable downtime — how long it takes to restore service; RPO (Recovery Point Objective) is the maximum acceptable data loss — how old the most recent backup/replica can be.
- C) RTO applies to databases only; RPO applies to VMs only.
- D) RTO and RPO are the same thing measured in different units.

> **Answer: B**  
> RTO answers "how long can the system be down?" — a 15-minute RTO means service must be restored within 15 minutes. RPO answers "how much data can we afford to lose?" — a 5-minute RPO means the data can be at most 5 minutes old at the time of recovery. Both are business requirements that drive the choice of DR solution.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An e-commerce application needs 99.99% monthly uptime SLA. It is currently deployed on a single Azure App Service in one region. What architectural change is required to achieve the 99.99% SLA?

- A) Upgrade to the App Service Premium v3 tier in the same region.
- B) Deploy the App Service to two Azure regions, place Azure Front Door in front with health probes, and use a geo-replicated database backend. The combined SLA of redundant components exceeds 99.99%.
- C) Enable App Service Always-On setting and increase the instance count to 3.
- D) Configure App Service with ZRS storage for the content share.

> **Answer: B**  
> A single-region App Service has a maximum SLA of 99.95%. Achieving 99.99% requires multi-region active-active or active-passive deployment with a global load balancer (Azure Front Door, 99.99% SLA) routing traffic. The composite SLA of independent redundant components (A AND B fail = 0.05% × 0.05% = 0.0025%) far exceeds 99.99%.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company runs a 3-tier application (web, app, database) on Azure VMs. They want to protect against a full datacentre failure within the same Azure region (not a full region failure). Which redundancy configuration achieves this?

- A) Deploy all VMs in the same Availability Set.
- B) Deploy each tier\'s VMs across multiple Availability Zones within the same region (zone-redundant deployment).
- C) Deploy VMs in a single Availability Set in one fault domain.
- D) Use Azure Backup with cross-region restore.

> **Answer: B**  
> Availability Zones are physically separate datacentres within an Azure region with independent power, cooling, and networking. Deploying each tier across 2–3 AZs ensures that a single datacentre failure does not bring down the application. Availability Sets protect against rack-level failures within a single datacentre but not datacentre-level failures.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 13. Migration

**Q.** A company wants to migrate 200 on-premises VMs to Azure. Before committing to migration, they need an assessment of which VMs are ready for Azure, the recommended Azure VM sizes, and the estimated monthly cost. Which tool provides this?

- A) Azure Cost Management pricing calculator
- B) Azure Migrate: Discovery and Assessment
- C) Azure Site Recovery
- D) Azure Database Migration Service

> **Answer: B**  
> Azure Migrate: Discovery and Assessment deploys a lightweight appliance on-premises that discovers VMs (VMware, Hyper-V, or physical servers), collects performance data, and produces an Azure readiness report with recommended VM SKUs and estimated costs. Azure Site Recovery performs the actual replication/migration, not the assessment.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company is migrating a 10 TB on-premises SQL Server 2019 database to Azure SQL Managed Instance with minimal downtime (less than 30 minutes). Which migration approach achieves this?

- A) Take a full backup, restore it to Azure SQL MI (offline migration — downtime equals restore time, likely hours).
- B) Use Azure Database Migration Service (DMS) in online mode with Log Shipping / Change Data Capture to sync changes continuously, then perform a cutover during a short maintenance window.
- C) Export to BACPAC, import into Azure SQL MI (BACPAC is unsupported for MI at scale).
- D) Use Azure Data Factory to copy all tables row-by-row to Azure SQL MI.

> **Answer: B**  
> DMS online mode uses log shipping (for SQL Server) to continuously sync transaction log changes to the target MI after the initial full backup restore. When the lag is near-zero, the team schedules a short cutover window (typically <30 minutes) to stop writes on the source, let the final logs drain, and redirect the application to the new endpoint. Offline migration requires the source database to be taken offline for the entire restore duration.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company needs to transfer 500 TB of on-premises archive data to Azure Blob Storage. Their internet connection is 1 Gbps and is shared with production traffic. Over the network, this transfer would take over 6 weeks. Which Azure service provides the fastest path?

- A) AzCopy with parallel threads over ExpressRoute
- B) Azure Data Box Heavy (1 PB capacity physical device shipped to the datacenter)
- C) Azure Import/Export service with standard HDDs shipped by the customer
- D) Azure Data Factory with self-hosted Integration Runtime

> **Answer: B**  
> Azure Data Box Heavy is a ruggedized 1 PB storage device shipped by Microsoft. The team copies data locally (up to 1 Gbps USB), ships the device back, and Microsoft uploads data directly into Azure Storage at datacenter speeds. For 500 TB, this is orders of magnitude faster than a 1 Gbps internet connection shared with production traffic.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A legacy .NET Framework 4.7 web application runs on IIS on Windows Server 2012. The team wants to lift-and-shift to Azure with minimal changes. Which Azure service is most appropriate?

- A) Azure App Service (Windows) — .NET Framework 4.7 is supported
- B) Azure Kubernetes Service with a Windows node pool
- C) Azure Virtual Machines running Windows Server — migrate the VM as-is using Azure Migrate
- D) Azure Functions with .NET Framework isolated worker

> **Answer: C**  
> For a true lift-and-shift with minimal changes, Azure Migrate replicates the on-premises IIS/Windows Server VM to an Azure VM. No code changes are required. Azure App Service supports .NET Framework but may require configuration changes (no IIS configuration file support for all features). AKS requires containerization. Azure Functions does not support full IIS-hosted apps.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** During a cloud migration, a company discovers that their application connects to a third-party on-premises API that cannot be moved to Azure due to a vendor contract. The Azure-hosted application must securely call this on-premises API. What is the recommended connectivity approach?

- A) Expose the on-premises API on the public internet and restrict by IP.
- B) Establish an Azure VPN Gateway Site-to-Site or ExpressRoute connection to the on-premises network, allowing the Azure application to call the private on-premises API endpoint securely.
- C) Use Azure API Management with a self-hosted gateway deployed on-premises.
- D) Both B and C are valid approaches — B for network connectivity, C for API management features like throttling, auth, and caching at the edge.

> **Answer: D**  
> B (VPN/ExpressRoute) establishes the private network path from Azure to on-premises. C (API Management self-hosted gateway) deploys the gateway component on-premises close to the backend API, adding management features (rate limiting, auth, caching, logging) without requiring the API to be on the internet. Both can be combined for a complete solution.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 14. AI and Cognitive Services

**Q.** A development team wants to add natural language search capabilities to their Azure-hosted product catalog so users can query in plain English (e.g., "red running shoes under $100"). Which Azure service combination implements this?

- A) Azure Cognitive Search with semantic search + Azure OpenAI Service for query understanding
- B) Azure SQL Database full-text search
- C) Azure Table Storage with wildcard queries
- D) Azure Bot Service with LUIS (Language Understanding)

> **Answer: A**  
> Azure AI Search (formerly Cognitive Search) with semantic ranking understands query intent beyond keyword matching. Combined with Azure OpenAI Service (e.g., GPT-4o for query enrichment or vector embeddings for semantic similarity search), it enables natural language product discovery. Azure SQL full-text search is keyword-based and does not understand query semantics.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company wants to build a chatbot grounded in their own internal documentation (PDFs, Word files, SharePoint content) using Azure OpenAI Service. The chatbot must only answer from their documents and cite sources. Which Azure pattern implements this?

- A) Fine-tune a GPT model with the company\'s documents.
- B) Retrieval Augmented Generation (RAG): index documents in Azure AI Search (with vector embeddings), retrieve relevant chunks for each user query, and pass them as context to Azure OpenAI\'s chat completion API.
- C) Store all documents in Azure Blob Storage and pass the entire storage account URL to the OpenAI API.
- D) Use Azure Cognitive Services Text Analytics to extract key phrases and feed them to a rule-based chatbot.

> **Answer: B**  
> RAG is the recommended pattern for grounding LLM responses in enterprise documents. Documents are chunked and vectorized (using OpenAI Ada embeddings or other models) and indexed in Azure AI Search. At query time, the most relevant chunks are retrieved and injected into the OpenAI prompt as context. This is more cost-effective and up-to-date than fine-tuning, and avoids hallucinations by grounding responses in retrieved text.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An application calls Azure OpenAI Service for completions and occasionally receives HTTP 429 (Too Many Requests) errors during peak hours. The team has a provisioned throughput deployment (PTU). What is the recommended way to handle this without losing requests?

- A) Catch the 429 error and immediately retry with a fixed 1-second delay.
- B) Implement exponential backoff with jitter on retry, and consider overflow to a pay-as-you-go deployment when PTU capacity is exhausted.
- C) Switch from PTU to pay-as-you-go to avoid rate limits entirely.
- D) Increase the `max_tokens` parameter to reduce the number of API calls.

> **Answer: B**  
> Azure OpenAI recommends exponential backoff with jitter for 429 errors to avoid retry storms. For PTU deployments, overflow routing to a PAYG endpoint handles burst traffic beyond provisioned capacity. Fixed-interval retries worsen the problem under sustained load. Increasing `max_tokens` increases per-call cost/latency but does not reduce the rate of calls.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A healthcare company wants to extract structured information (patient name, diagnosis, medication, dosage) from unstructured clinical notes stored in Azure Blob Storage. Which Azure AI service is purpose-built for healthcare NLP?

- A) Azure OpenAI Service with a custom prompt
- B) Azure AI Language — Text Analytics for Health
- C) Azure Form Recognizer (Document Intelligence)
- D) Azure Cognitive Search with custom skills

> **Answer: B**  
> Azure AI Language\'s **Text Analytics for Health** is a pre-trained NLP model specifically designed to extract and structure healthcare entities (conditions, medications, dosages, anatomy, test results) from clinical text in compliance with healthcare standards. Form Recognizer extracts structured fields from forms/documents by layout, not by healthcare-specific NLP.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company trains a custom image classification model using Azure Machine Learning. After training, they want to expose the model as a REST API endpoint that auto-scales based on request volume and has no idle compute cost. Which Azure ML deployment target achieves this?

- A) Azure Machine Learning Managed Online Endpoint with Kubernetes compute
- B) Azure Machine Learning Serverless Online Endpoint (pay-per-call)
- C) Azure Container Instances with the model Docker image
- D) Deploy the model to Azure Functions as a Python function

> **Answer: B**  
> Azure Machine Learning Serverless Online Endpoints (in preview/GA 2025) provide fully managed, auto-scaling inference with per-call billing — no idle compute charges. Managed Online Endpoints with dedicated compute incur costs even when idle. ACI is non-managed and does not auto-scale. Azure Functions imposes memory and timeout limits unsuitable for large ML models.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 15. Governance and Compliance

**Q.** An organization wants to ensure that all new Azure resources are created only in approved regions (East US and West Europe) and that resource groups without a required `Owner` tag cannot be created. Which Azure service enforces these constraints declaratively at the control plane?

- A) Azure Role-Based Access Control (RBAC)
- B) Azure Policy with `deny` effect definitions
- C) Azure Blueprints
- D) Azure Advisor recommendations

> **Answer: B**  
> Azure Policy with `deny` effect prevents non-compliant resource creation or updates at the ARM control plane. Built-in policies exist for allowed locations and required tags. RBAC controls who can perform operations but not what resource configurations are allowed. Blueprints orchestrate policy assignment but the enforcement mechanism is still Policy.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A security team discovers that a storage account is publicly accessible (blob public access enabled). They remediate it manually, but the same misconfiguration keeps reappearing. Which Azure Policy capability automatically remediates non-compliant resources without manual intervention?

- A) Azure Policy `audit` effect with email notifications
- B) Azure Policy `deployIfNotExists` or `modify` effect with a remediation task and a managed identity.
- C) Microsoft Defender for Cloud security recommendations
- D) Azure Advisor security score improvements

> **Answer: B**  
> Azure Policy with `modify` effect can automatically correct resource properties (like setting `allowBlobPublicAccess: false`) on existing non-compliant resources via a remediation task. `deployIfNotExists` deploys a companion resource if missing. The policy is assigned a managed identity with the appropriate RBAC to make the change. `audit` only flags the issue without fixing it.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An enterprise has 5 business units, each with multiple subscriptions. The security team needs to apply consistent policies and RBAC across all 50+ subscriptions from a single location, with different policies for Production vs Non-Production subscriptions. What management structure achieves this?

- A) Assign policies to each subscription individually.
- B) Create a Management Group hierarchy: Root → Production MG → [prod subscriptions]; Root → Non-Production MG → [non-prod subscriptions]. Assign policies at each MG level; they inherit down.
- C) Use Azure Blueprints on each subscription separately.
- D) Create a single subscription with resource group-based separation.

> **Answer: B**  
> Management Group hierarchies enable governance at scale. Policies and RBAC assigned to a Management Group automatically inherit to all child Management Groups and subscriptions. The Production and Non-Production MGs can have different policy sets (e.g., stricter network policies in Production), with shared baseline policies assigned at the Root MG.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An organization must demonstrate SOC 2 Type II compliance for their Azure-hosted SaaS application. They need a centralized dashboard showing which Azure controls are compliant and which have gaps. Which tool provides this?

- A) Azure Cost Management compliance reports
- B) Microsoft Defender for Cloud with Regulatory Compliance dashboard (SOC 2 standard)
- C) Azure Monitor Workbooks with compliance queries
- D) Azure Service Health compliance certificates

> **Answer: B**  
> Microsoft Defender for Cloud includes a Regulatory Compliance dashboard that maps Azure security controls to compliance standards including SOC 2, ISO 27001, PCI DSS, HIPAA, CIS, and NIST. It shows compliant vs non-compliant controls in real-time and provides remediation guidance, supporting audit evidence collection.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team accidentally deleted a critical Azure resource group containing production resources. Which Azure feature would have prevented this, and which feature could help with recovery if prevention was not in place?

- A) Prevention: Azure Backup; Recovery: Azure Site Recovery
- B) Prevention: Azure Resource Locks (`CanNotDelete` lock on the resource group); Recovery: Azure Resource Graph query to find deleted resources + restore from Azure Backup snapshots.
- C) Prevention: Azure Policy `deny` effect; Recovery: redeploy from ARM templates.
- D) Prevention: RBAC removing delete permissions; Recovery: Azure Activity Log replay.

> **Answer: B**  
> A `CanNotDelete` resource lock prevents deletion of a resource or resource group even by subscription Owners, until the lock is explicitly removed. For recovery, Azure Backup point-in-time restore (for VMs, SQL, blobs) restores data. Azure Resource Graph can identify what was in the deleted resource group from historical data. The Activity Log records the delete event for audit purposes.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 16. AZ-900: Azure Fundamentals Certification

> *AZ-900 validates foundational cloud concepts and core Azure services. No technical prerequisites required.*

**Q.** The AZ-900 exam tests understanding of the shared responsibility model. In an IaaS deployment, which of the following is the **customer\'s** responsibility?

- A) Physical datacenter security
- B) Hypervisor and host OS patching
- C) Guest OS patching and application security
- D) Network fabric and cooling

> **Answer: C**  
> In IaaS, Microsoft manages the physical infrastructure (datacenter, hardware, hypervisor, host OS). The customer is responsible for the guest OS (patching, configuration), middleware, runtime, applications, and data. In PaaS, the customer responsibility shrinks further to application and data only.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A small business owner asks: "What is the primary financial benefit of moving from on-premises to the Azure cloud?" Which AZ-900 concept best describes the benefit?

- A) Replacing Capital Expenditure (CapEx) with Operational Expenditure (OpEx) — pay only for what you use.
- B) Replacing Operational Expenditure (OpEx) with Capital Expenditure (CapEx) — buy infrastructure upfront.
- C) Eliminating all IT costs permanently.
- D) Guaranteeing 100% uptime for all workloads.

> **Answer: A**  
> Cloud computing converts the large upfront capital investment in hardware (CapEx) into a predictable, usage-based operational expense (OpEx). This improves cash flow and reduces the risk of over-provisioning or under-provisioning infrastructure.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Which Azure service provides a **guarantee** of minimum uptime (SLA) for a virtual machine deployed across **two or more Availability Zones**?

- A) 99.9% SLA — same as a single VM with Premium SSD
- B) 99.99% SLA — the highest VM SLA, requiring zone-redundant deployment
- C) 99.95% SLA — same as an Availability Set
- D) 100% SLA — Azure guarantees no downtime with Availability Zones

> **Answer: B**  
> Microsoft\'s SLA for a single VM with Premium SSD is 99.9%. For VMs in an Availability Set it is 99.95%. For VMs deployed across two or more Availability Zones it is 99.99%. No cloud provider guarantees 100% uptime.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A company wants to estimate the monthly cost of running an Azure Virtual Machine and Azure SQL Database before provisioning them. Which free Azure tool provides this estimate?

- A) Azure Cost Management + Billing (for existing resources)
- B) Azure Pricing Calculator (pricing.azure.com)
- C) Azure Advisor cost recommendations
- D) Azure Total Cost of Ownership (TCO) Calculator

> **Answer: B**  
> The **Azure Pricing Calculator** lets you configure hypothetical Azure services (VM size, region, OS, hours per month) and get an estimated monthly cost before provisioning anything. The **TCO Calculator** compares on-premises costs to Azure — different use case. Azure Cost Management tracks actual spend on existing resources.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** According to AZ-900 concepts, which statement about Azure Availability Zones and Azure Region Pairs is **correct**?

- A) Availability Zones protect against full region failures; Region Pairs protect against rack-level failures.
- B) Availability Zones are physically separate datacenters within one region, protecting against datacenter failures; Region Pairs are two regions hundreds of miles apart, protecting against region-wide disasters.
- C) Region Pairs and Availability Zones provide identical levels of redundancy.
- D) Availability Zones are only available in the US; Region Pairs are a global feature.

> **Answer: B**  
> Availability Zones = intra-region redundancy (datacenter-level protection). Region Pairs = inter-region redundancy (geographic disaster protection, e.g., East US ↔ West US). Azure prioritizes recovering one region in a pair first during a platform-wide outage, and ensures updates are not rolled out to both regions simultaneously.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 17. AZ-104: Azure Administrator Certification

> *AZ-104 (which replaced the retired AZ-102) validates skills in implementing, managing, and monitoring Azure environments for enterprise organizations.*

**Q.** An Azure Administrator needs to move a virtual machine from resource group **rg-dev** to resource group **rg-prod** within the same subscription. What happens to the VM\'s resource ID after the move?

- A) The resource ID remains unchanged.
- B) The resource ID changes to reflect the new resource group name.
- C) The move is not supported — VMs cannot be moved between resource groups.
- D) The VM is deleted and must be recreated in the new resource group.

> **Answer: B**  
> When you move a resource between resource groups (or subscriptions), its resource ID changes because the resource group name is part of the ID path (`/subscriptions/.../resourceGroups/rg-prod/...`). The VM\'s public/private IPs, disks, and configuration are preserved, but any hardcoded references to the old resource ID (in scripts, RBAC assignments, policy scopes) must be updated.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An Administrator needs to allow a specific external vendor\'s application to access an Azure Storage account using its own identity — not a user account. No passwords should be stored. Which identity type in Microsoft Entra ID is appropriate?

- A) User account with a guest invitation (Azure AD B2B)
- B) Service Principal with certificate-based authentication or Workload Identity Federation
- C) Managed Identity assigned to the vendor\'s application
- D) Shared Access Signature stored in the vendor\'s app config

> **Answer: B**  
> A **Service Principal** is an application identity in Entra ID. For external vendor apps running outside Azure, a service principal with **certificate authentication** (avoiding client secrets) or **Workload Identity Federation** (OIDC) is the secure pattern. Managed Identities are only for Azure-hosted workloads. SAS tokens grant storage access but are not identity-based.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An Azure Administrator runs the following command and receives an error:

```bash
az vm create --resource-group rg-prod --name vm-web01 \
  --image Ubuntu2204 --size Standard_D4s_v5 \
  --location eastus
```

Error: `The subscription does not have enough quota for cores in region eastus.`

What is the correct resolution?

- A) Choose a different VM size that uses fewer cores.
- B) Request a quota increase via the Azure portal (Subscriptions → Usage + Quotas → Request Increase) or deploy in a different region with available quota.
- C) Delete other VMs to free quota — Azure does not allow quota increases.
- D) Upgrade the subscription from Pay-as-you-go to Enterprise Agreement.

> **Answer: B**  
> Azure subscriptions have per-region vCPU quotas. When the quota is exhausted, new VMs cannot be deployed in that region for that VM family. Administrators request quota increases through the portal or support ticket. Alternatively, deploying in a different region (with available quota) is an immediate workaround.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An Administrator needs to ensure that all Azure VMs in the production subscription have the Azure Monitor Agent installed and are sending logs to a specific Log Analytics workspace. New VMs provisioned in the future must also comply automatically. Which approach achieves this at scale?

- A) Manually install the agent on each VM after provisioning.
- B) Assign an Azure Policy Initiative (e.g., "Enable Azure Monitor for VMs") with `deployIfNotExists` effect, scoped to the production subscription. Create a remediation task for existing non-compliant VMs.
- C) Create an Azure Automation runbook that checks for the agent weekly.
- D) Use an ARM template to deploy the agent alongside each VM.

> **Answer: B**  
> Azure Policy with `deployIfNotExists` automatically installs the Azure Monitor Agent on VMs that don\'t have it, triggered at deployment time (for new VMs) and via remediation tasks (for existing VMs). This ensures continuous compliance without manual intervention. The built-in "Enable Azure Monitor for VMs" initiative includes the relevant policy definitions.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An Azure Administrator configures a Network Security Group with the following inbound rules:

| Priority | Source | Port | Action |
|----------|--------|------|--------|
| 100 | 10.0.1.0/24 | 443 | Allow |
| 200 | * | 443 | Deny |
| 300 | * | * | Deny |

A VM in subnet 10.0.2.0/24 tries to connect to the protected VM on port 443. What is the result?

- A) The connection is allowed because port 443 is in both rules 100 and 200.
- B) The connection is denied — the source 10.0.2.0/24 does not match rule 100\'s source (10.0.1.0/24), so rule 200 (deny all on 443) applies.
- C) The connection is allowed because Azure allows HTTPS traffic by default.
- D) The connection hits rule 300 and is denied regardless.

> **Answer: B**  
> NSG rules are evaluated in priority order (lowest number first). The source 10.0.2.0/24 does not match rule 100 (which only allows 10.0.1.0/24). Rule 200 matches (any source, port 443, Deny) and blocks the connection. Rule 300 is never reached because rule 200 already matched. NSGs do not have any built-in "allow HTTPS" default — all unlisted traffic follows the default deny-all rule.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 18. AZ-204: Azure Developer Certification

> *AZ-204 validates skills in designing, building, testing, and maintaining cloud applications and services on Azure.*

**Q.** A developer is building an Azure Function that must process each message from an Azure Service Bus queue **exactly once**, even if the function fails mid-processing and the message is retried. Which Service Bus feature ensures exactly-once processing?

- A) Set `maxDeliveryCount` to 1 so messages are never retried.
- B) Use Service Bus sessions with a session-aware receiver to enforce FIFO ordering per session.
- C) Enable duplicate detection on the queue with a detection window that covers the maximum processing time, and make the function idempotent.
- D) Use PeekLock mode combined with message deduplication and an idempotent processing logic that checks a processed-message store (e.g., Azure Table Storage) before acting.

> **Answer: D**  
> Service Bus PeekLock holds the message invisible to other consumers until explicitly completed or abandoned. However, if the function crashes after processing but before completing the lock, the message reappears. True exactly-once semantics require idempotent processing: check a durable store (Table Storage, Redis, SQL) whether the message ID was already processed before acting, then complete the lock. Duplicate detection prevents the *same message from being enqueued twice*, not from being processed twice after delivery.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer implements the following code pattern in a .NET Azure Function:

```csharp
var credential = new DefaultAzureCredential();
var client = new SecretClient(new Uri(keyVaultUrl), credential);
var secret = await client.GetSecretAsync("ConnectionString");
```

What authentication method does `DefaultAzureCredential` use when running in Azure App Service with a system-assigned Managed Identity enabled?

- A) It prompts the developer for username/password.
- B) It reads credentials from a `.env` file in the project root.
- C) It automatically uses the Managed Identity token from the Azure Instance Metadata Service (IMDS), requiring no stored credentials.
- D) It falls back to the Azure CLI logged-in account.

> **Answer: C**  
> `DefaultAzureCredential` tries multiple credential sources in order. In App Service/Functions with a Managed Identity, it uses `ManagedIdentityCredential`, which fetches a token from the IMDS endpoint (`169.254.169.254`) using the assigned managed identity — no client secrets, certificates, or stored credentials required. The Azure CLI fallback is used only in local development environments.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer registers an application in Microsoft Entra ID to call the Microsoft Graph API and read users' profiles. The app runs as a background service (no user interaction). Which OAuth 2.0 flow should the developer use?

- A) Authorization Code Flow with PKCE
- B) Client Credentials Flow
- C) Device Authorization Flow
- D) On-Behalf-Of (OBO) Flow

> **Answer: B**  
> The **Client Credentials Flow** is for daemon/service applications that run without a signed-in user. The app authenticates with its own identity (client ID + secret or certificate) and receives an access token scoped to application permissions. Authorization Code Flow requires user interaction. OBO is used when a service calls another service on behalf of a signed-in user.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs to cache the results of expensive database queries in Azure Cache for Redis. The cache should automatically expire entries after 10 minutes and handle cache misses gracefully. Which pattern should be implemented?

- A) Write-through cache: write to Redis and the database simultaneously on every request.
- B) Cache-Aside (Lazy Loading): on a cache miss, read from the database, write the result to Redis with a 10-minute TTL, and return the result.
- C) Read-through cache: configure Redis to query the database automatically on a miss.
- D) Write-behind cache: write to Redis first and asynchronously sync to the database.

> **Answer: B**  
> **Cache-Aside** (also called Lazy Loading) is the recommended pattern with Azure Cache for Redis for read-heavy workloads. The application checks the cache first; on a miss, it fetches from the database, stores the result in Redis with a TTL (`SETEX key 600 value`), and returns the data. Azure Cache for Redis does not natively support read-through or write-behind without additional infrastructure.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer is publishing messages to Azure Event Hubs from an IoT application. The consumer application must process messages from the **same device** (same partition key) in strict order. What should the developer configure?

- A) Use multiple consumer groups, one per device.
- B) Set the `PartitionKey` on each event to the device ID when publishing; consumers reading from the same partition receive events in order.
- C) Use Event Hub Capture to write to Azure Storage and process files in order.
- D) Enable geo-replication on the Event Hub namespace.

> **Answer: B**  
> Azure Event Hubs guarantees message ordering **within a partition**. By setting `PartitionKey = deviceId`, all messages from the same device are routed to the same partition and delivered in arrival order to consumers. Different devices can be on different partitions, enabling parallel processing. Consumer groups provide independent read positions for different consumer applications, not ordering guarantees.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 19. AZ-305: Azure Solutions Architect Certification

> *AZ-305 validates skills in designing cloud and hybrid solutions including compute, network, storage, monitoring, identity, and governance at enterprise scale.*

**Q.** An architect is designing a multi-region active-active web application. Each region has its own Azure SQL Database. Users in Asia write data to the Asia region, but reads must reflect data written by any region within 5 seconds. Which Azure SQL Database feature supports this?

- A) Azure SQL Database geo-replication (asynchronous, no multi-region writes)
- B) Azure SQL Database Hyperscale with named replicas
- C) Azure Cosmos DB with Session consistency (SQL API) — not Azure SQL
- D) Azure SQL Database Hyperscale does not support multi-region writes; use Cosmos DB multi-master with Bounded Staleness consistency (max 5-second lag) for this scenario.

> **Answer: D**  
> Azure SQL Database does not support multi-region active-active writes natively. For true multi-region write with bounded replication lag (e.g., 5 seconds), **Azure Cosmos DB** with **Bounded Staleness** consistency is the correct choice. Bounded Staleness guarantees reads lag behind writes by no more than K operations or T seconds (configurable). This is a common AZ-305 architecture decision point — choosing the right database for multi-region write requirements.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An architect must design a solution where 50 branch offices each connect to Azure with a private connection, and all inter-branch traffic is routed through Azure. The solution must minimize the number of VPN tunnels and provide a hub-and-spoke topology that is easy to manage. Which Azure networking service is purpose-built for this?

- A) 50 separate VNet Peerings between branch VNets and a hub VNet
- B) Azure Virtual WAN (vWAN) with branch office VPN connections
- C) 50 Site-to-Site VPN Gateways, one per branch
- D) Azure ExpressRoute with 50 circuits

> **Answer: B**  
> **Azure Virtual WAN** is a managed, global transit network service that provides hub-spoke connectivity for branches, remote users, VNets, and ExpressRoute circuits from a single control plane. It automatically manages routing between all connected sites (any-to-any via the vWAN hub), supports thousands of branches, and eliminates the operational burden of managing individual VPN tunnels or peerings.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An architect is designing a solution where an Azure Function needs to call an Azure SQL Database. The database is in a private VNet with no public endpoint. The Function is on the Consumption plan. How should the architect design network connectivity?

- A) Enable the SQL Database public endpoint and restrict by Azure Function outbound IPs.
- B) Upgrade the Function to a Premium or Dedicated plan, enable VNet Integration on the Function App, and configure a private endpoint for the SQL Database in the same VNet.
- C) Use Azure API Management as a proxy between the Function and SQL Database.
- D) Consumption plan Functions cannot access private VNet resources under any circumstance.

> **Answer: B**  
> Azure Functions **VNet Integration** (outbound) is supported on Premium and Dedicated plans (not Consumption). It allows Functions to make outbound calls into a VNet where a private endpoint for Azure SQL is configured. The SQL Database\'s private endpoint exposes a private IP in the VNet, and the Function routes to it via the integrated VNet. This is a frequently tested AZ-305 network design pattern.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An architect needs to design a solution where legacy on-premises applications authenticate using Kerberos/NTLM (Windows Integrated Authentication) but users are managed in Microsoft Entra ID (no on-premises AD). Which Azure service bridges this gap?

- A) Microsoft Entra ID Connect (Azure AD Connect) — requires on-premises AD
- B) Azure AD Domain Services (Microsoft Entra Domain Services) — provides managed Kerberos/NTLM authentication from Entra ID without on-premises AD
- C) Microsoft Entra External ID (B2B)
- D) Azure Active Directory Federation Services (ADFS)

> **Answer: B**  
> **Microsoft Entra Domain Services** (formerly Azure AD Domain Services) provides managed domain services — LDAP, Kerberos, NTLM, Group Policy — backed by Microsoft Entra ID without deploying or managing domain controllers. Legacy apps that require Kerberos/NTLM can join the managed domain and authenticate using Entra ID users. This eliminates the need for on-premises AD while supporting legacy authentication protocols.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An architect is designing a data pipeline that ingests 1 million events per second from IoT devices, processes them in real-time for anomaly detection, and stores raw events for 90-day historical analysis. Which Azure architecture best fits this?

- A) Azure Queue Storage → Azure Functions → Azure SQL Database
- B) Azure Event Hubs (ingestion, 90-day retention) → Azure Stream Analytics (real-time processing, anomaly detection) → Azure Data Lake Storage Gen2 (historical storage) + Azure Synapse Analytics (batch analysis)
- C) Azure Service Bus Premium → Azure Logic Apps → Azure Blob Storage
- D) Azure API Management → Azure Cache for Redis → Azure Cosmos DB

> **Answer: B**  
> This is the classic **Lambda/Kappa architecture** on Azure: Event Hubs handles massive throughput ingestion with configurable retention (up to 90 days with Event Hubs Premium). Stream Analytics provides real-time processing with built-in anomaly detection ML functions. Data Lake Gen2 stores raw events cheaply at scale, and Synapse Analytics enables historical batch queries. Service Bus is for reliable message delivery, not high-throughput event streaming.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 20. AZ-400: Azure DevOps Engineer Certification

> *AZ-400 validates skills in designing and implementing DevOps practices including source control, CI/CD, infrastructure as code, monitoring, and security integration.*

**Q.** A DevOps engineer needs to implement a branching strategy that supports multiple versions of a product in production simultaneously (e.g., v1.x still receiving security patches while v2.x is in active development). Which Git branching strategy is most appropriate?

- A) Trunk-Based Development with feature flags
- B) Git Flow with `main`, `develop`, `release/v1.x`, `release/v2.x`, `hotfix/*`, and `feature/*` branches
- C) GitHub Flow with a single `main` branch and short-lived feature branches
- D) Feature Branch Workflow with one branch per developer

> **Answer: B**  
> **Git Flow** explicitly supports parallel version maintenance through long-lived release branches (`release/v1.x`, `release/v2.x`). Hotfixes can be cherry-picked to both. Trunk-Based Development and GitHub Flow work best for teams deploying a single version continuously — they do not naturally support simultaneous multi-version maintenance without complexity.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A DevOps engineer wants to prevent secrets from being committed to the Azure Repos Git repository. Which mechanism detects secrets **before** they are pushed to the remote repository?

- A) Azure DevOps branch policies with a required build validation pipeline
- B) A pre-commit hook (e.g., `git-secrets`, `detect-secrets`, or `gitleaks`) installed on developers' machines, combined with a pipeline secret-scanning step as a second line of defense
- C) Azure Key Vault with access policies for the repository
- D) Microsoft Defender for DevOps repository scanning (post-push only)

> **Answer: B**  
> Pre-commit hooks run locally before a commit is created, catching secrets before they ever enter Git history. Tools like `gitleaks`, `detect-secrets`, or `git-secrets` scan staged files for patterns. A pipeline scanning step (Defender for DevOps, GitHub Advanced Security for ADO) acts as a second gate after the push. Defender for DevOps scans the remote repo — it does not prevent the push from occurring.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A DevOps engineer is designing a release pipeline for a microservices application on AKS. The team requires that a new version is deployed to 10% of users first, monitored for error rate increases, and automatically promoted to 100% if healthy or rolled back if error rate exceeds 1%. Which deployment strategy implements this?

- A) Blue-Green deployment
- B) Canary release with automated progressive delivery (e.g., using Argo Rollouts or Azure DevOps + Application Insights release gates)
- C) Rolling update with `maxSurge: 1`
- D) Recreate deployment strategy (scale old to 0, deploy new)

> **Answer: B**  
> **Canary release with automated progressive delivery** routes a small percentage of traffic to the new version, monitors metrics (error rate, latency) against defined thresholds, and automatically promotes or rolls back. Azure DevOps release gates integrate with Application Insights to query metrics before promoting between stages. Argo Rollouts on AKS provides native Kubernetes canary with metric-based analysis. Blue-Green switches 100% at once; rolling updates don\'t support traffic percentage control natively.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** An AZ-400 engineer is setting up a dependency management strategy. The team uses internal NuGet packages shared across 15 microservices. They want a central repository for internal packages with access control, versioning, and upstream proxy to nuget.org. Which Azure service provides this?

- A) Azure Blob Storage with a custom NuGet feed URL
- B) Azure Artifacts with a NuGet feed (supports upstreaming, access control, versioning, and retention policies)
- C) GitHub Packages with a public registry
- D) Azure Container Registry (for NuGet packages)

> **Answer: B**  
> **Azure Artifacts** is the package management service in Azure DevOps that supports NuGet, npm, Maven, Python (PyPI), and Cargo feeds. It supports upstream sources (proxying nuget.org so all package traffic routes through Artifacts), RBAC for feed access, semantic versioning, and retention policies to control storage. ACR is for container images, not NuGet packages.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A DevOps engineer implements a Continuous Integration pipeline. A code review policy requires that all builds pass before a PR can be merged. However, the full test suite takes 45 minutes. What technique reduces the feedback loop without removing test coverage?

- A) Remove integration tests from the CI pipeline entirely.
- B) Implement test parallelization (split tests across multiple agents) and test impact analysis (run only tests affected by changed files), targeting a sub-10-minute PR validation pipeline.
- C) Increase the pipeline agent\'s VM size to speed up the build.
- D) Merge without tests and run the full suite nightly.

> **Answer: B**  
> **Test parallelization** distributes test execution across multiple agents simultaneously (Azure DevOps supports parallel jobs with `strategy: parallel`). **Test Impact Analysis** (TIA) — supported in Visual Studio Test task — identifies which tests are affected by code changes and runs only those, skipping unrelated tests. Together these techniques reduce PR feedback time from 45 minutes to under 10 minutes while maintaining coverage. Nightly-only testing is a risk management failure.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

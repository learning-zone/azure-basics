# Azure Scenario-Based MCQ

> Scenario-based multiple choice questions covering core Microsoft Azure topics.

<br>

## Table of Contents

## L1: Cloud Foundations (Entry-Level / Cloud Practitioner)
Focus: Core Azure concepts, cloud models, resource management, and fundamental services.

* [Azure Fundamentals](#-1-azure-fundamentals): Core Azure concepts, shared responsibility, cloud models, and resource management.
* [Compute Services](#-2-compute-services): VMs, App Service, AKS, Azure Functions, Service Bus, and Batch.

## L2: Infrastructure (Associate Level)
Focus: Storage, networking, and security essentials for building and securing Azure infrastructure.

* [Azure Storage](#-3-azure-storage): Blob, access tiers, lifecycle management, replication options, and AzCopy.
* [Networking](#-4-networking): VNet, NSG, Load Balancer, Application Gateway, ExpressRoute, and VNet Peering.
* [Security and Identity](#-5-security-and-identity): Entra ID, RBAC, Key Vault, Managed Identities, Defender for Cloud, and DDoS.

## L3: Operations (Cloud Administrator / Developer)
Focus: Monitoring, observability, databases, access governance, and operational management.

* [Monitoring and Diagnostics](#-6-monitoring-and-diagnostics): Azure Monitor, Application Insights, alerts, and distributed tracing.
* [Databases](#-7-databases): Azure SQL, Cosmos DB, Elastic Pools, PostgreSQL, and Redis.
* [Governance and Compliance](#-8-governance-and-compliance): Azure Policy, RBAC, and compliance standards.

## L4: Architecture & Migration (Solutions Architect)
Focus: Cloud architecture patterns, disaster recovery, and migration strategies.

* [High Availability and Disaster Recovery](#-9-high-availability-and-disaster-recovery): Multi-region HA, backup, site recovery, and DR patterns.
* [Infrastructure Design: Cost, Multi-region, and HA/DR](#-10-infrastructure-design-cost-multi-region-and-hadr): Reserved Instances, right-sizing, Traffic Manager, Cosmos DB multi-master, and SQL geo-replication.
* [Migration](#-11-migration): Azure Migrate, 6 R\'s, Database Migration Service, Data Box, and Data Factory.

## L5: Development & DevOps (Cloud Developer / DevOps Engineer)
Focus: Application development, CI/CD pipelines, and container orchestration.

* [Azure App Service and Functions](#-12-azure-app-service-and-functions): Deployment slots, Azure Functions, Logic Apps, Event Grid, and Service Bus.
* [Serverless and Performance Management](#-13-serverless-and-performance-management): Durable Functions patterns, KEDA/Event Hub scaling, cold starts, and CPU offloading.
* [Error Handling, Deadlock Prevention, and Encryption](#-14-error-handling-deadlock-prevention-and-encryption): Retry policies, SQL deadlocks, TDE with CMK, and Service Bus DLQ.
* [Containers and Kubernetes](#-15-containers-and-kubernetes): AKS, ACR, container lifecycle, and Kubernetes networking.
* [DevOps and CI/CD](#-16-devops-and-cicd): Azure DevOps YAML pipelines, GitHub Actions, Terraform, and container registries.
* [ARM Templates and Bicep Syntax](#-17-arm-templates-and-bicep-syntax): ARM JSON authoring, Bicep modules, parameter files, and deployment modes.

## L6: Expert (Cloud Architect / Lead)
Focus: Cost governance, AI integration, and advanced Azure scenarios.

* [Cost Management](#-18-cost-management): Pricing models, Azure Advisor, Reserved Instances, Budgets, and Cost Analysis.
* [AI and Cognitive Services](#-19-ai-and-cognitive-services): Azure OpenAI, Cognitive Services, AI Search, and Machine Learning.


## Certifications

* [AZ-900: Azure Fundamentals Certification](#-20-az-900-azure-fundamentals-certification)
* [AZ-104: Azure Administrator Certification](#-21-az-104-azure-administrator-certification)
* [AZ-204: Azure Developer Certification](#-22-az-204-azure-developer-certification)
* [AZ-305: Azure Solutions Architect Certification](#-23-az-305-azure-solutions-architect-certification)
* [AZ-400: Azure DevOps Engineer Certification](#-24-az-400-azure-devops-engineer-certification)

<br>

## # 1. Azure Fundamentals

<br>

## Q. A startup wants to host a web application on Azure. The development team is new to Azure and wants to avoid managing virtual machines, OS patching, or server scaling. Which service model best fits their requirement?

- A) Infrastructure as a Service (IaaS)
- B) Platform as a Service (PaaS)
- C) Software as a Service (SaaS)
- D) Function as a Service (FaaS)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

PaaS abstracts the underlying infrastructure (VMs, OS, networking) so developers focus only on application code and data. Azure App Service is a prime example. IaaS (Azure VMs) requires OS management; SaaS is a ready-made application (like Microsoft 365); FaaS (Azure Functions) is event-driven serverless, a subset of PaaS.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company deploys resources across three Azure subscriptions for Dev, Test, and Production. The security team needs to apply the same Azure Policy across all three subscriptions from a single place. What is the most efficient approach?

- A) Assign the policy to each subscription individually.
- B) Create a Management Group that contains all three subscriptions, then assign the policy to the Management Group.
- C) Use Azure Blueprints on each subscription separately.
- D) Tag all resources and create a policy based on tags.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Management Groups are a governance scope above subscriptions. Policies assigned to a Management Group automatically apply to all subscriptions and resource groups beneath it, eliminating per-subscription repetition.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer is choosing between an Azure Region and an Azure Availability Zone for deploying a mission-critical application. What is the primary difference?

- A) Regions are free; Availability Zones cost extra.
- B) A Region is a geographic area with multiple datacenters; Availability Zones are physically separate datacenters within the same region, each with independent power, cooling, and networking.
- C) Availability Zones are located in different countries; Regions are in the same country.
- D) Regions provide disaster recovery; Availability Zones provide load balancing only.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

An Azure Region is a set of datacenters deployed within a latency-defined perimeter. Within each region, Availability Zones are physically isolated datacenters connected by high-speed private fiber. Deploying across AZs protects against single-datacenter failures within the same region.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Your team uses Azure Resource Manager (ARM) templates to deploy infrastructure. A colleague says the same template is deployed to three environments (dev, staging, prod) but with different parameter files. After a production deployment, you discover the wrong parameter file was used. Which ARM feature helps prevent this mistake in the future?

- A) Use linked templates stored in different storage containers.
- B) Use Azure Blueprints to lock templates per environment.
- C) Use deployment stacks with deny assignments to prevent manual overrides, and integrate the parameter file selection into a CI/CD pipeline with environment-specific steps.
- D) Use resource tags to separate environments.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Deployment stacks manage a set of resources as a unit and can apply deny assignments to prevent out-of-band changes. Integrating parameter file selection into a CI/CD pipeline (e.g., Azure DevOps environment stages) eliminates manual file selection errors.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An organization has two Azure subscriptions managed under the same Microsoft Entra tenant. Resources in Subscription A need to communicate with resources in Subscription B. Which statement about cross-subscription resource access is correct?

- A) Resources in different subscriptions cannot communicate because subscriptions are fully isolated networks.
- B) Cross-subscription communication requires a separate Azure ExpressRoute circuit per subscription.
- C) Virtual Network Peering and Role-Based Access Control can be configured across subscriptions within the same tenant, allowing secure resource sharing.
- D) You must merge both subscriptions into one to allow cross-subscription resource access.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

VNet Peering supports cross-subscription connectivity within the same Entra tenant. RBAC can grant principals from one subscription access to resources in another. Subscriptions share the same identity plane within a tenant.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. Compute Services

<br>

## Q. A company runs a batch processing workload that is highly CPU-intensive and runs once a day for about 2 hours. The workload is stateless and can tolerate interruptions. Which Azure compute option gives the lowest cost?

- A) Azure Virtual Machines (pay-as-you-go)
- B) Azure Spot Virtual Machines
- C) Azure Reserved Virtual Machine Instances (1-year)
- D) Azure Dedicated Hosts

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Spot VMs use unused Azure capacity at up to 90% discount. Since the workload is stateless and can tolerate eviction (it can be restarted), Spot VMs are the cheapest option. Reserved Instances save money for always-on workloads, not sporadic 2-hour jobs. Dedicated Hosts are the most expensive option.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A web application is deployed across 10 Azure Virtual Machines behind an Azure Load Balancer. During a planned maintenance event, Azure reboots two VMs simultaneously. The application becomes unavailable. What configuration would have prevented this?

- A) Use a larger VM size so fewer VMs are needed.
- B) Place the VMs in an Availability Set, which groups VMs into fault domains and update domains.
- C) Use Azure Traffic Manager with geographic routing.
- D) Enable VM auto-shutdown schedules.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Availability Sets place VMs across multiple update domains (UDs) and fault domains (FDs). During planned maintenance, Azure respects UD boundaries — only one UD is rebooted at a time — ensuring at least some VMs remain online.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An e-commerce platform experiences a 5x traffic spike every Black Friday. The team wants the application to automatically scale out additional web server VMs when CPU exceeds 70% and scale in when it drops below 30%, without manual intervention. Which Azure feature enables this?

- A) Azure Autoscale on a Virtual Machine Scale Set (VMSS)
- B) Azure Traffic Manager with performance routing
- C) Azure Load Balancer health probes
- D) Azure Advisor right-sizing recommendations

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Virtual Machine Scale Sets with Azure Autoscale rules allow automatic horizontal scaling based on metrics (CPU, memory, queue depth, custom metrics). Traffic Manager and Load Balancer distribute traffic but do not add or remove VMs. Azure Advisor provides recommendations but does not auto-scale.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to run a one-off data transformation script that takes 45 minutes. The script is containerized. The team does not want to manage VM infrastructure, and the job should be billed only for the duration it runs. Which Azure service is most appropriate?

- A) Azure Kubernetes Service (AKS)
- B) Azure Container Instances (ACI)
- C) Azure App Service (container deployment)
- D) Azure Virtual Machines with Docker installed

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Container Instances runs containers on-demand with per-second billing, no cluster management, and no idle costs. AKS requires a node pool that incurs costs whether or not workloads are running. App Service is for long-running web apps. VMs require manual OS management.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Your company\'s on-premises application writes files to a local network share and reads them back sequentially. You are lifting-and-shifting this application to Azure VMs. Which Azure storage service should replace the on-premises network share?

- A) Azure Blob Storage
- B) Azure Files (SMB share)
- C) Azure Table Storage
- D) Azure Queue Storage

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Files provides fully managed SMB and NFS file shares accessible from Azure VMs and on-premises machines. Applications that rely on the file system path and SMB protocol work without code changes. Blob Storage is object storage accessed via HTTP/SDK, not via file system paths.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. Azure Storage

<br>

## Q. A media company stores large video files that are accessed frequently for the first 30 days after upload and then rarely accessed afterward. They want to minimize storage costs automatically without deleting the files. What Azure Storage feature should they configure?

- A) Azure Blob Storage with Lifecycle Management policies to transition blobs from Hot to Cool (or Archive) tier after 30 days.
- B) Azure Queue Storage with time-to-live (TTL) settings.
- C) Azure Table Storage with automatic data expiry.
- D) Azure File Sync with cloud tiering enabled.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Blob Storage Lifecycle Management policies can automatically transition blobs between Hot, Cool, Cold, and Archive tiers (or delete them) based on the number of days since last modification or creation. This minimizes cost without manual intervention.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An auditing requirement states that files stored in Azure Blob Storage must not be modified or deleted for 7 years after creation. Which feature enforces this at the storage level?

- A) Azure Storage firewall with IP restrictions
- B) Soft delete for blobs
- C) Immutable Blob Storage with a time-based retention policy (WORM)
- D) Azure Backup for storage accounts

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Immutable Blob Storage with a locked time-based retention policy implements WORM (Write Once, Read Many) compliance. Once locked, even subscription admins cannot delete or overwrite blobs until the retention period expires. Soft delete only protects against accidental deletion with a short window.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to grant temporary read access to a private blob in Azure Storage to an external partner for exactly 24 hours, without making the container public or sharing storage account keys. What is the correct approach?

- A) Change the container access level to "Blob (anonymous read)" and revert it after 24 hours.
- B) Generate a Shared Access Signature (SAS) token with read permission and an expiry of 24 hours.
- C) Share the storage account access key and ask the partner to delete it after use.
- D) Use Azure AD guest user access and assign Storage Blob Data Reader role.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A SAS token grants scoped, time-limited access to specific resources without exposing the account key. It can be restricted to read-only, specific containers/blobs, and a 24-hour window. Sharing the account key grants full storage account access and cannot be scoped.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A storage account in East US replicates data to West US. During a regional disaster in East US, the secondary region in West US becomes read-only. The operations team needs to restore full read/write access as quickly as possible. Which replication option supports a customer-initiated failover to make the secondary region the new primary?

- A) Locally Redundant Storage (LRS)
- B) Zone-Redundant Storage (ZRS)
- C) Geo-Redundant Storage (GRS) with customer-managed failover
- D) Read-Access Geo-Redundant Storage (RA-GRS) — failover is not possible in this tier.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

GRS (and RA-GRS) replicate data to a secondary region. Microsoft now supports customer-initiated account failover, which promotes the secondary to primary for read-write access. LRS and ZRS do not replicate to a secondary region.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to store semi-structured session data for millions of users with fast key-value lookups, minimal operational overhead, and automatic scaling. Which Azure storage service is most appropriate?

- A) Azure SQL Database
- B) Azure Table Storage
- C) Azure Blob Storage
- D) Azure Files

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Table Storage is a NoSQL key-value store designed for fast, cost-effective storage of large amounts of semi-structured data. It scales automatically and is accessed via a simple REST/SDK API. For even more advanced NoSQL features, Azure Cosmos DB would be considered, but Table Storage meets the stated requirements with minimal cost.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A media company stores video files in Azure Blob Storage. Files uploaded in the last 30 days are accessed frequently; files between 30–365 days old are rarely accessed; files older than 365 days must be retained for compliance and almost never accessed. Which strategy minimizes storage cost while meeting retention requirements?

- A) Store all videos in Hot tier and use Azure CDN to reduce storage transaction costs.
- B) Use a **Lifecycle Management policy** to transition blobs: Hot → Cool after 30 days (infrequent access), Cool → Cold after 90 days, Cold → Archive after 365 days; apply an immutability policy or legal hold for compliance retention.
- C) Use a single Lifecycle Management rule that moves all blobs directly to Archive after 30 days.
- D) Store all videos in Cold tier from upload to minimize cost uniformly.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Blob Storage Lifecycle Management automates tier transitions. **Hot** (frequent access) → **Cool** (infrequent, 30-day minimum) → **Cold** (rare access, 90-day minimum) → **Archive** (near-zero access, 180-day minimum, rehydration required) progressively reduces storage cost as files age. **Immutability policies** or **legal holds** enforce compliance retention. Moving everything to Archive at 30 days (Option C) ignores the Cool/Cold tiers and requires rehydration for files still occasionally accessed. Cold tier from upload (Option D) incurs early deletion fees and penalizes frequently accessed new uploads.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer accidentally commits a Shared Access Signature (SAS) token to a public GitHub repository. The SAS grants read access to a production blob container. What is the correct immediate remediation?

- A) Set the blob container\'s access level to Private to block all SAS-authenticated requests.
- B) If the SAS was signed using a **Stored Access Policy**, delete or modify that policy to immediately invalidate all associated SAS tokens. If the SAS was an ad-hoc SAS signed with a storage account key, **rotate that storage account key** immediately to invalidate all SAS tokens signed with it.
- C) Add IP firewall rules on the storage account to block all public internet access.
- D) Delete the blob container and recreate it with a new name.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A SAS token\'s validity depends on how it was created. A **Stored Access Policy**-based SAS can be revoked by deleting or modifying the policy. An **ad-hoc SAS** (signed directly with a storage account key) is invalidated only by rotating the account key used to sign it. IP firewall rules (Option C) restrict access by network origin but do not revoke the already-issued SAS token. Container access level (Option A) controls only anonymous access, not SAS-authenticated requests. Deleting the container (Option D) causes data loss.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An enterprise wants to replace its on-premises file server (Windows SMB shares) with a cloud-based solution. Windows clients must mount a network drive using the standard UNC path format without a VPN. Which Azure service supports this natively?

- A) Azure Blob Storage with NFS 3.0 protocol enabled.
- B) **Azure Files** with SMB protocol — clients mount shares using `\\<storageaccount>.file.core.windows.net\<sharename>` over port 445, with optional Microsoft Entra Kerberos authentication for identity-based access control.
- C) Azure NetApp Files (ANF) with SMB, as the only enterprise-grade SMB solution on Azure.
- D) Azure Data Lake Storage Gen2 with hierarchical namespace enabled.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Files** supports SMB 2.1 and SMB 3.0 protocols, allowing Windows, Linux, and macOS clients to mount shares as native network drives. **Microsoft Entra Kerberos** and on-premises AD DS authentication enable identity-based NTFS permissions. **Azure NetApp Files** (Option C) also supports SMB but is a high-cost, high-performance service designed for enterprise/HPC workloads, not general file server replacement. Blob Storage with NFS 3.0 targets Linux workloads and does not support Windows SMB. Data Lake Storage is for analytics, not file sharing.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A backend service reads messages from Azure Queue Storage. Each message takes up to 90 seconds to process. After 30 seconds, messages become visible again and are processed a second time by a competing consumer, causing duplicate work. What is the correct fix?

- A) Increase the queue\'s `MessageTimeToLive` (TTL) value to 90 seconds.
- B) When reading the message, set the **visibility timeout** to a value greater than the maximum expected processing time (e.g., 120 seconds); only delete the message from the queue after successful processing.
- C) Enable FIFO ordering on the Azure Queue Storage queue to prevent duplicate delivery.
- D) Reduce the backend\'s concurrency to a single consumer thread.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Queue Storage\'s **visibility timeout** hides a dequeued message from other consumers for a specified duration (default 30 seconds). If processing exceeds this timeout, the message becomes visible again and is redelivered — causing duplicates. Setting the visibility timeout to the maximum expected processing duration (e.g., 120 seconds), or calling `UpdateMessage` to reset it during long operations, prevents premature redelivery. Messages are permanently removed only when explicitly deleted after successful processing. Azure Queue Storage does not support guaranteed FIFO ordering.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. Networking

<br>

## Q. Two Azure Virtual Networks in different regions need to communicate privately without traffic going over the public internet. Each VNet has non-overlapping address spaces. What should you configure?

- A) Azure VPN Gateway with Site-to-Site connection
- B) Azure ExpressRoute between the two VNets
- C) Global VNet Peering
- D) Azure Traffic Manager with private endpoints

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Global VNet Peering connects VNets in different Azure regions over Microsoft\'s backbone network — not the public internet — with low latency and no bandwidth limits beyond subscription quotas. VPN Gateway introduces encryption overhead and uses public IPs. ExpressRoute connects on-premises to Azure, not VNet-to-VNet.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An application tier (VMs) must only accept inbound traffic from the web tier subnet (10.0.1.0/24) on port 443, and deny all other inbound traffic. Which Azure resource enforces this rule?

- A) Azure Firewall with DNAT rules
- B) A Network Security Group (NSG) attached to the application subnet with an inbound rule allowing TCP 443 from 10.0.1.0/24 and a lower-priority deny-all rule.
- C) Azure DDoS Protection Standard
- D) Route Table with a blackhole route for all other traffic

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

NSGs are stateful packet filters applied at the subnet or NIC level. An inbound rule with source `10.0.1.0/24`, destination port `443`, action `Allow`, and a lower-priority `Deny` catch-all rule achieves the required segmentation. Azure Firewall operates at a higher level and is used for hub-spoke architectures. DDoS Protection defends against volumetric attacks, not access control.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company is migrating from on-premises to Azure and requires a dedicated, private connection with consistent bandwidth (not over the public internet) between their datacenter and Azure, with 99.95% SLA. Which service should they use?

- A) Azure VPN Gateway (Site-to-Site IPsec)
- B) Azure ExpressRoute
- C) Azure Bastion
- D) Azure Virtual WAN

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure ExpressRoute provides a private, dedicated connection to Azure through a connectivity provider, bypassing the public internet. It offers up to 100 Gbps bandwidth, consistent latency, and a 99.95% SLA. VPN Gateway uses encrypted tunnels over the public internet and has lower guaranteed bandwidth.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A web application hosted in Azure needs to distribute HTTPS traffic across multiple backend VMs based on URL path (`/api/*` → API servers, `/static/*` → CDN servers). Layer 7 SSL termination is also required. Which Azure load-balancing service should be used?

- A) Azure Load Balancer (Standard)
- B) Azure Traffic Manager
- C) Azure Application Gateway
- D) Azure Front Door (basic tier)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Azure Application Gateway is a Layer 7 load balancer that supports URL path-based routing, SSL termination, Web Application Firewall (WAF), session affinity, and header rewrites. Azure Load Balancer operates at Layer 4 (TCP/UDP) and cannot inspect URLs. Traffic Manager uses DNS-based routing and does not terminate SSL.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company deploys Azure VMs without public IP addresses in a locked-down VNet. Operations staff needs to RDP and SSH into these VMs securely without opening inbound ports 3389/22 to the internet. Which Azure service enables this?

- A) Azure VPN Gateway with Point-to-Site VPN
- B) Azure Bastion
- C) Just-In-Time (JIT) VM access via Microsoft Defender for Cloud
- D) Azure Private Link

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Bastion provides secure, browser-based RDP and SSH access to VMs directly in the Azure portal over TLS port 443, without requiring a public IP or opening RDP/SSH ports to the internet. JIT VM access temporarily opens ports on-demand but does expose them to specific IPs, while Bastion never exposes those ports.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company has three business units, each requiring isolated Azure environments with separate billing, independent resource quotas, and distinct security boundaries. Which Azure construct best achieves this?

- A) Three separate Azure Resource Groups within a single subscription
- B) Three separate Azure Subscriptions grouped under a Management Group
- C) Three separate VNets with VNet Peering within a single subscription
- D) Three separate Microsoft Entra tenants

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Subscriptions provide billing isolation, independent resource limits/quotas, and a security boundary. Resource Groups are logical containers within one subscription — they share the same quota and billing. VNets provide network isolation but not billing or quota separation. Multiple tenants would break unified identity management across the organization.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A network engineer creates an NSG allowing inbound HTTP (port 80) and HTTPS (port 443) to a web server subnet. The admin team now reports that SSH connections (port 22) from their corporate IP range are being blocked. What is the root cause?

- A) NSGs cannot allow multiple ports in a single rule.
- B) The NSG default `DenyAllInBound` rule (priority 65500) blocks SSH because no explicit allow rule for port 22 exists.
- C) NSGs applied to a subnet cannot block traffic originating from within the same VNet.
- D) SSH traffic must use a different protocol type setting in the NSG rule.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

NSGs include a default inbound rule `DenyAllInBound` at priority 65500 that blocks all traffic not explicitly permitted by a lower-priority rule. Since only ports 80 and 443 are allowed, port 22 is blocked. The fix is to add an inbound NSG rule permitting TCP port 22 from the corporate IP range at a priority lower than 65500 (e.g., 300). NSGs fully support multiple ports per rule and function correctly on subnets.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A spoke VNet is peered with a hub VNet in a hub-and-spoke topology. The team wants all internet-bound traffic from the spoke to flow through an Azure Firewall in the hub, but after enabling peering, spoke VMs still route directly to the internet. What configuration is missing?

- A) Enable "Allow forwarded traffic" on the hub side of the peering.
- B) Enable "Allow gateway transit" on the hub peering and "Use remote gateways" on the spoke peering, then add a User-Defined Route (UDR) on the spoke subnet pointing `0.0.0.0/0` to the Azure Firewall\'s private IP in the hub.
- C) Replace VNet Peering with a VNet-to-VNet VPN between hub and spoke.
- D) Enable "Allow virtual network access" on the peering — this is disabled by default.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

VNet Peering alone does not redirect traffic to the hub\'s firewall. A **User-Defined Route (UDR)** on the spoke subnet with `0.0.0.0/0` and next hop type `Virtual Appliance` pointing to the Azure Firewall\'s private IP is required to force-tunnel internet traffic through the hub. "Allow gateway transit" and "Use remote gateways" are needed only when sharing a VPN or ExpressRoute gateway — not strictly for firewall routing. Option A affects forwarded traffic pass-through but does not redirect internet egress.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A security audit finds that an NSG allows inbound RDP (port 3389) from source `0.0.0.0/0` to a production subnet. What is the correct remediation that maintains admin access while eliminating public exposure?

- A) Delete the NSG entirely and rely on Azure\'s built-in firewall.
- B) Restrict the NSG rule\'s source to the specific admin IP range, or deploy Azure Bastion to provide browser-based RDP/SSH over TLS — completely eliminating the need to expose port 3389 publicly.
- C) Change the NSG rule priority to 65000 so it is evaluated last and rarely matched.
- D) Enable Just-In-Time (JIT) VM Access without modifying the NSG source prefix.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Exposing RDP to `0.0.0.0/0` is a critical security misconfiguration (OWASP Top 10 — A05:2021). The optimal remediation is **Azure Bastion**, which provides RDP and SSH over TLS through the Azure portal without any public port exposure. Alternatively, restricting the source to a known admin IP range significantly reduces attack surface. JIT VM Access (Option D) can time-limit port exposure, but the underlying rule still allows the world unless source is also restricted. Lowering priority (Option C) has no meaningful security effect.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. Security and Identity

<br>

## Q. A developer\'s application running on an Azure VM needs to access secrets stored in Azure Key Vault. The team wants to avoid storing any credentials (client secrets or certificates) in the application code or configuration. What is the recommended approach?

- A) Store the Key Vault access key in an environment variable on the VM.
- B) Create a service principal and embed its client secret in the application\'s `appsettings.json`.
- C) Assign a System-Assigned Managed Identity to the VM and grant it the Key Vault Secrets User role.
- D) Use a shared access signature (SAS) token to authenticate to Key Vault.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Managed Identities eliminate the need to manage credentials. A system-assigned managed identity creates an Azure AD identity for the VM; the application acquires a token from the Instance Metadata Service (IMDS) endpoint automatically. RBAC then grants that identity access to Key Vault. No secrets are stored anywhere in the code or config.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A security audit finds that a junior developer was accidentally assigned the Owner role on the production subscription 3 months ago. The role was never used. Your task is to prevent such excessive privilege assignments in the future using the principle of least privilege. Which combination of features best addresses this?

- A) Use Azure Policy to deny Owner role assignments at the subscription scope; use Azure AD Privileged Identity Management (PIM) for time-bound, approval-required role activations.
- B) Create a custom role with all permissions and assign it to everyone.
- C) Enable Multi-Factor Authentication (MFA) for all users.
- D) Move all resources to a separate subscription so junior developers cannot access them.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Azure Policy can deny broad role assignments (like Owner) at sensitive scopes. PIM enforces just-in-time access: users must request activation of privileged roles, require approval, and the access auto-expires. Together these enforce least privilege and provide a full audit trail. MFA alone does not restrict what users can do once authenticated.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A financial services company requires that any user accessing their Azure-hosted banking application from outside corporate networks must complete MFA, while users on the corporate network are trusted. Which Azure feature implements this location-based conditional access?

- A) Azure AD Password Protection
- B) Microsoft Entra ID Conditional Access with Named Locations
- C) Azure Firewall application rules
- D) Microsoft Defender for Identity

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Conditional Access policies in Microsoft Entra ID can evaluate sign-in conditions including network location (Named Locations defined by IP ranges). A policy granting access only after MFA when the sign-in is not from the corporate IP range implements the requirement. Firewall rules operate at the network layer and do not integrate with identity-aware policies.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Your application uses a connection string to connect to Azure SQL Database. This connection string is currently stored in `appsettings.json` committed to Git. The security team flags this as a critical vulnerability. What is the correct remediation?

- A) Base64-encode the connection string before committing to Git.
- B) Move the connection string to an environment variable on the build server.
- C) Store the connection string in Azure Key Vault and have the application retrieve it at startup using a Managed Identity.
- D) Encrypt `appsettings.json` with a symmetric key stored in the same repository.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Azure Key Vault is the dedicated secret store for connection strings, API keys, and certificates. Combined with a Managed Identity, the application fetches the secret at runtime with no credentials embedded in code or Git history. Base64 encoding is not encryption. Storing secrets in environment variables on build servers is an improvement but not the recommended Azure pattern.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team wants to audit all operations performed on an Azure Key Vault — specifically who accessed which secret and when — for compliance purposes. Which service provides this audit trail?

- A) Azure Monitor Metrics
- B) Azure Key Vault Diagnostic Logs sent to a Log Analytics workspace
- C) Microsoft Defender for Key Vault alerts only
- D) Azure Activity Log at the subscription level

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Key Vault Diagnostic Logs (specifically the `AuditEvent` category) record every data-plane operation: who accessed which secret, key, or certificate, and at what time. These logs are sent to a Log Analytics workspace, Event Hub, or Storage Account. The Activity Log records control-plane operations (create/delete vault) but not secret access.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A financial services company needs a private, dedicated connection from their on-premises datacenter to Azure with guaranteed bandwidth, predictable low latency, and SLA-backed availability — without traversing the public internet. Which Azure networking service fulfills this?

- A) Azure VPN Gateway with a Site-to-Site IPsec VPN tunnel.
- B) **Azure ExpressRoute** — a private connection provisioned through a network connectivity provider (e.g., Equinix, AT&T) that bypasses the internet entirely, providing up to 100 Gbps bandwidth, consistent latency, and a 99.95% SLA.
- C) Azure Virtual WAN with SD-WAN partner integration over broadband.
- D) Azure Point-to-Site (P2S) VPN for per-user device connectivity.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure ExpressRoute** establishes a dedicated private circuit through a connectivity provider. Traffic never crosses the public internet, ensuring **predictable latency**, **guaranteed bandwidth** (50 Mbps to 100 Gbps), and a 99.95% availability SLA. VPN Gateway (Option A) uses IPsec tunnels over the public internet — bandwidth and latency are shared and variable. P2S VPN (Option D) is designed for individual device connections. Virtual WAN (Option C) uses internet-based underlay unless combined with ExpressRoute.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company uses Azure ExpressRoute for hybrid connectivity. They want a cost-effective automatic failover to ensure on-premises workloads can still reach Azure if the ExpressRoute circuit fails. What is the recommended design?

- A) Provision two ExpressRoute circuits from different providers in active-passive mode.
- B) Configure a **Site-to-Site VPN Gateway as a failover path** alongside the ExpressRoute circuit. Use **BGP route preferences** (AS path, local preference) to prefer ExpressRoute routes when available; when ExpressRoute fails, BGP converges and traffic automatically routes over the VPN tunnel.
- C) Configure VNet Peering between two regions to create a redundant path.
- D) Enable ExpressRoute Global Reach to connect on-premises sites through Azure as backup.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **ExpressRoute + VPN failover** pattern is the Microsoft-recommended cost-effective resilience design. BGP is configured to prefer ExpressRoute paths; when the circuit goes down, BGP reconverges and routes traffic over the VPN (reduced bandwidth, slightly higher latency but functional). Two ExpressRoute circuits (Option A) provide higher availability but at significantly greater cost. Global Reach (Option D) connects on-premises sites via Azure\'s backbone — it is not a failover mechanism. VNet Peering (Option C) connects VNets, not on-premises to Azure.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Firewall in the hub VNet is blocking outbound HTTPS (port 443) requests from a spoke application to `api.example.com`. A network rule allowing TCP from the spoke to `0.0.0.0/0:443` was added, but the team prefers to restrict outbound access to specific FQDNs rather than all internet IPs. Which rule type should be used?

- A) A DNAT rule to translate the spoke\'s private IP to a public IP for outbound traffic.
- B) An **Application rule (FQDN rule)** targeting `api.example.com` on HTTPS port 443 — Azure Firewall resolves the FQDN to IPs and enforces outbound access by hostname rather than IP address, supporting wildcards and TLS inspection.
- C) A NAT rule on the hub to allow outbound SNAT for the spoke subnet.
- D) A custom route (UDR) bypassing the firewall for the specific destination IP.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Firewall **Application rules** filter outbound HTTP/HTTPS/MSSQL traffic by **FQDN** (fully qualified domain name). The firewall dynamically resolves the FQDN to IPs and enforces the allow/deny. This is preferred over Network rules targeting `0.0.0.0/0` because it restricts to specific named endpoints. Wildcards (e.g., `*.example.com`) are supported. DNAT rules (Option A) are for inbound traffic mapping. NAT/SNAT rules handle address translation, not outbound access control. Bypassing the firewall (Option D) defeats the security purpose.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A Site-to-Site VPN Gateway connects an on-premises network to Azure. The on-premises team can reach existing Azure VMs, but cannot reach VMs in a newly added subnet `10.2.5.0/24`. The VPN uses static routing (no BGP). What needs to be updated?

- A) Upgrade the VPN Gateway to a higher SKU to support more address prefixes.
- B) Update the **Local Network Gateway** resource (which defines the on-premises address space from Azure\'s perspective) to include `10.2.5.0/24`, and update the on-premises VPN device\'s routing table to include the new Azure subnet.
- C) Add a UDR on the new subnet with `0.0.0.0/0` pointing to the VPN Gateway.
- D) Create a new VPN connection specifically for the new subnet.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

For **static routing VPNs**, route information is not automatically exchanged. The **Local Network Gateway** in Azure defines address prefixes that represent the on-premises network — and conversely, the on-premises VPN device must have routes for the new Azure subnet. Adding `10.2.5.0/24` to the Azure VNet address space and updating both the Local Network Gateway address prefixes and the on-premises routing table resolves the connectivity gap. With **BGP** (Option A\'s implication), new subnets are advertised automatically — no manual update needed. A new VPN connection (Option D) is not required; a single connection can route multiple address spaces.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. Databases

<br>

## Q. An application needs a globally distributed database with single-digit millisecond reads, multiple write regions, and five consistency level options from strong to eventual. Which Azure database service should you choose?

- A) Azure SQL Database Hyperscale
- B) Azure Database for PostgreSQL — Flexible Server
- C) Azure Cosmos DB
- D) Azure Cache for Redis

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Azure Cosmos DB is a globally distributed, multi-model NoSQL database with turnkey multi-region writes, five consistency levels (Strong, Bounded Staleness, Session, Consistent Prefix, Eventual), and guaranteed single-digit millisecond latency at the 99th percentile. Azure SQL Database is relational and does not offer multi-region writes natively.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company\'s Azure SQL Database experiences performance degradation during month-end reporting. The reports involve complex analytical queries over 3 years of transactional data. The OLTP workload must not be impacted. What is the recommended architectural solution?

- A) Upgrade the Azure SQL Database to a Business Critical tier.
- B) Add an Azure Cache for Redis layer in front of the SQL Database.
- C) Use Azure Synapse Analytics (dedicated SQL pool) for the analytical queries, and replicate data from Azure SQL via Synapse Link or ADF pipelines.
- D) Use Read Replicas on Azure SQL Database and run reports on the replica.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Separating OLTP (Azure SQL) from OLAP (Azure Synapse Analytics) is the recommended pattern. Synapse is optimized for massive parallel analytical queries. Azure Synapse Link for SQL provides near-real-time data replication from Azure SQL to Synapse without impacting the OLTP workload. Read replicas reduce read load on the primary but don\'t provide the analytical query performance of a columnar MPP engine.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A SaaS platform serves 500 tenants, each with their own Azure SQL Database. Managing 500 individual databases is operationally expensive. The databases have variable and unpredictable usage patterns — some are busy while others are mostly idle. What Azure SQL feature reduces cost and management overhead in this scenario?

- A) Azure SQL Managed Instance
- B) Azure SQL Elastic Pools
- C) SQL Server on Azure VM
- D) Azure SQL Database Hyperscale

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Elastic Pools allow multiple databases to share a pool of eDTUs or vCores. Databases with different peak times share resources cost-effectively — idle databases consume minimal resources while busy ones burst. This is the canonical multi-tenant SaaS architecture for Azure SQL.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An application queries an Azure SQL Database for the same product catalog data millions of times per day. The catalog changes once per hour. Database CPU and DTU consumption is high despite the data being mostly static. What is the most effective optimization?

- A) Scale up the Azure SQL Database to a higher service tier permanently.
- B) Add a read replica and route all reads to it.
- C) Place Azure Cache for Redis in front of the database; cache product catalog results with a 1-hour TTL.
- D) Enable Query Store on Azure SQL Database to identify slow queries.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Caching static or slow-changing data in Redis dramatically reduces database load. With a 1-hour TTL matching the catalog update frequency, the application serves millions of reads from in-memory cache at sub-millisecond latency, reducing SQL DTU consumption significantly. Scaling up permanently is expensive for a caching problem.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Your team is migrating an on-premises SQL Server 2019 instance with SQL Agent jobs, linked servers, and CLR objects to Azure. The requirement is minimal code changes and full SQL Server compatibility. Which Azure SQL option should you choose?

- A) Azure SQL Database (single database)
- B) Azure SQL Database Hyperscale
- C) Azure SQL Managed Instance
- D) SQL Server on Azure Virtual Machines

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Azure SQL Managed Instance provides near-100% compatibility with SQL Server on-premises, including SQL Agent, CLR, linked servers, cross-database queries, Service Broker, and Database Mail — features not available in Azure SQL Database. SQL Server on Azure VM provides full SQL Server control but requires OS management.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A globally distributed product catalog on Azure Cosmos DB is read-heavy and replicated across 5 regions. The team accepts slightly stale reads but requires that reads never see data in a different order than it was written (i.e., writes that happened in sequence must always appear in sequence to readers). Which consistency level fits this requirement with the lowest read latency?

- A) Strong
- B) Bounded Staleness
- C) Consistent Prefix
- D) Eventual

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Consistent Prefix** guarantees that reads never observe out-of-order writes. If items are written as versions 1, 2, 3, any reader sees 1, 1-2, or 1-2-3 — never 1-3 or 3 alone. It allows reads from local replicas (low latency) while preserving write ordering. **Eventual** consistency (Option D) provides the lowest latency but may return out-of-order values. **Strong** (Option A) ensures linearizability but routes reads to the write region, increasing latency. **Bounded Staleness** (Option B) also preserves ordering but adds a staleness window guarantee — more than what the scenario requires.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A banking application requires that every read always returns the most recently committed write, regardless of which region serves the request. No stale reads are acceptable under any circumstances. Which consistency level must be used, and what is its primary trade-off?

- A) Session consistency — per-session read-your-writes guarantee with low latency.
- B) **Strong consistency** — provides linearizability (every read reflects the latest committed write), but reads must be served from the write region or a quorum, increasing read latency for geographically distributed clients and reducing throughput.
- C) Bounded Staleness — ensures reads are current within a configured time window.
- D) Eventual consistency — lowest latency reads, suitable for financial data.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Strong consistency** is the only Cosmos DB level that guarantees every read returns the most recently committed write (linearizability). The trade-off is that reads cannot be served from local replicas in remote regions — they must wait for quorum acknowledgment, increasing latency proportionally to inter-region distance. Strong consistency is also incompatible with multi-region write configurations. For financial transactions requiring strict correctness, this trade-off is acceptable. Eventual consistency (Option D) may return stale data — never appropriate for banking balances or financial records.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A social media application uses Cosmos DB. When a user posts a comment (write) and immediately queries their own feed (read), they must always see their own post. Reads by other users may serve slightly stale data. Which consistency level is optimal?

- A) Strong
- B) Bounded Staleness
- C) Session
- D) Eventual

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Session consistency** is Cosmos DB\'s default and most commonly used level. Within a single client session (tracked by a session token), it guarantees **read-your-own-writes**, **monotonic reads** (reads never go backward), and **monotonic writes** (writes are ordered). Other sessions may see slightly stale data. This is ideal for user-centric scenarios like social media feeds where a user must immediately see their own actions but other users' reads can be eventually consistent. Strong (Option A) is overkill with unnecessary latency overhead for this use case.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A Cosmos DB account uses Bounded Staleness configured with `MaxStalenessPrefix = 100 operations` and `MaxStalenessIntervalInSeconds = 5`. What does this configuration guarantee for reads from non-primary regions?

- A) Reads will never be more than 5 milliseconds behind the latest write.
- B) Reads from any region are guaranteed to lag the write region by no more than **100 operations OR 5 seconds** — whichever threshold is reached first. Once a replica exceeds either bound, it waits until caught up before serving read requests.
- C) The account will reject new writes if replication lag exceeds 5 seconds.
- D) Reads are always served from the closest local replica regardless of how far behind it is.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Bounded Staleness** provides strong ordering guarantees (identical to Consistent Prefix) while bounding the maximum lag between the write region and read replicas — by both **operation count (K)** and **time interval (T)**. If a replica falls behind by more than K operations or T seconds, it pauses reads until it catches up to within the bound. This is the only consistency level (besides Strong) that provides globally ordered reads while offering a defined freshness window. It does not throttle writes (Option C) and does not ignore lag when reading (Option D).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. Monitoring and Diagnostics

<br>

## Q. An operations team receives alerts about an Azure App Service application being slow, but by the time they investigate, the performance data is already gone. What should they configure to persist performance data for retrospective analysis?

- A) Enable Application Insights with a 90-day retention period and configure Continuous Export to Azure Storage.
- B) Increase the App Service plan to a Premium tier.
- C) Enable Azure Service Health alerts.
- D) Configure Azure Monitor Metrics with a 5-minute granularity.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Application Insights collects detailed telemetry (requests, dependencies, exceptions, performance counters). With a retention period up to 90 days (730 days on a workspace) and Continuous Export (or Diagnostic Settings to Log Analytics), all data persists for retrospective investigation. Azure Monitor Metrics retains 93 days but without the rich application-level detail of Application Insights.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer writes the following KQL query in Log Analytics:

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

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The query filters the `AzureActivity` table for rows where the operation name includes "delete" and the status is "Success". It projects four columns and orders results newest-first, giving an audit trail of successful delete operations.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An SRE team wants to be notified by SMS and email within 5 minutes if the average CPU of their Azure VM exceeds 90% for more than 3 minutes. What Azure Monitor components should they configure?

- A) A Service Health alert with a webhook to send an email.
- B) A Metric Alert on the VM\'s `Percentage CPU` metric with a 3-minute evaluation window, threshold of 90%, and an Action Group configured with email and SMS contacts.
- C) A Log Analytics scheduled query that runs every 5 minutes and sends an email.
- D) Azure Advisor cost recommendations with email notifications enabled.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Monitor Metric Alerts evaluate metric conditions at near-real-time. A metric alert on `Percentage CPU > 90%` with a 3-minute aggregation window triggers an Action Group, which sends notifications via email, SMS, voice call, webhook, or Logic Apps. Log Analytics alerts have higher latency due to ingestion delays.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A microservices application spans Azure Functions, Azure Service Bus, Azure SQL, and Azure App Service. The support team cannot trace a slow customer request end-to-end across all services. Which Application Insights feature provides distributed tracing across all these components?

- A) Application Insights Availability Tests
- B) Application Insights Application Map and distributed tracing with correlation IDs
- C) Azure Monitor Workbooks
- D) Azure Network Watcher Connection Monitor

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Application Insights propagates correlation IDs (W3C TraceContext standard) across service calls. The Application Map visualizes the entire call chain and highlights slow or failing dependencies. Each service (Functions, App Service) is instrumented with the Application Insights SDK or auto-instrumentation, enabling end-to-end distributed tracing.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to verify that their Azure-hosted e-commerce website is available and responding correctly from locations across Europe, Asia, and the Americas every 5 minutes, and receive an alert if availability drops below 99%. Which Azure Monitor feature enables this?

- A) Azure Service Health
- B) Application Insights Availability Tests (Standard or Multi-step tests)
- C) Azure Monitor Metrics — HTTP response time metric
- D) Azure Front Door health probes

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Application Insights Availability Tests (URL ping, Standard, or Multi-step) run synthetic HTTP requests from Azure test nodes worldwide at configurable intervals. Alerts trigger when availability drops below the configured threshold. Service Health reports Azure platform issues, not application-level availability from multiple geographic vantage points.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. Azure App Service and Functions

<br>

## Q. A team wants to deploy a new version of their App Service web application for testing by 10% of production traffic, while the remaining 90% continues to use the stable version. No infrastructure changes should be required. Which feature achieves this?

- A) Azure Traffic Manager weighted routing
- B) App Service Deployment Slots with Traffic Routing percentage
- C) Azure Application Gateway with path-based routing
- D) Azure Front Door with origin groups

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

App Service Deployment Slots allow traffic splitting between named slots (e.g., production and staging). The Traffic Routing percentage setting directs a configurable fraction of requests to the staging slot, enabling canary releases and A/B testing with a single App Service plan — no extra infrastructure.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Function is triggered by messages on an Azure Service Bus queue. During peak load, 10,000 messages arrive per minute. The function takes 2 seconds to process each message. The team is concerned about throughput and scaling. Which hosting plan supports unlimited automatic scale-out at no fixed cost for compute?

- A) App Service Plan (Standard S2)
- B) Azure Functions Consumption Plan
- C) Azure Functions Premium Plan
- D) Azure Functions Dedicated (App Service) Plan

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The Consumption Plan scales out automatically — adding new instances based on queue depth — with no fixed monthly charge for compute. You pay only per execution and GB-seconds of memory used. The Premium Plan also auto-scales but includes a minimum warm instance with a fixed cost. App Service Plan requires manual or autoscale configuration within fixed plan limits.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Function using the Consumption plan starts processing a large file upload but fails intermittently after 5 minutes with a timeout error. What is the most likely cause, and what is the solution?

- A) The function ran out of memory. Increase the memory allocation in `host.json`.
- B) The Consumption plan has a maximum execution timeout of 5 minutes (default); increase it to up to 10 minutes in `host.json`, or migrate to the Premium or Dedicated plan for longer executions.
- C) The Azure Storage trigger has a polling delay. Switch to an Event Grid trigger.
- D) The function is not retrying on failure. Enable retry policies in `host.json`.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The default timeout for Azure Functions on the Consumption plan is 5 minutes, configurable up to 10 minutes via `functionTimeout` in `host.json`. For operations exceeding 10 minutes, the Premium or Dedicated plan (with no timeout limit, or up to 60 minutes configured) is required. The Durable Functions pattern (async orchestration) is the recommended approach for very long-running workflows.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An App Service web application needs to read a secret from Azure Key Vault without storing credentials. The application is deployed in a Standard tier App Service. What is the simplest way to achieve this?

- A) Add the Key Vault access key to the App Service Application Settings.
- B) Enable the System-Assigned Managed Identity on the App Service, grant it Key Vault Secrets User RBAC, and reference the secret via a Key Vault reference in Application Settings: `@Microsoft.KeyVault(SecretUri=...)`.
- C) Download the secret from Key Vault during the CI/CD pipeline and inject it as a build artifact.
- D) Use Azure API Management to proxy Key Vault requests from the App Service.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

App Service supports Key Vault references natively. When a Managed Identity is enabled and has the Key Vault Secrets User role, you set an App Setting value to `@Microsoft.KeyVault(SecretUri=https://vault.vault.azure.net/secrets/mySecret/)`. The App Service runtime resolves the secret at startup automatically — no SDK code required.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team uses Azure Durable Functions to orchestrate a multi-step workflow: Step 1 calls an external payment API, Step 2 sends a confirmation email, and Step 3 updates a database. The external payment API is occasionally slow (up to 3 minutes). How does Durable Functions handle this without blocking a thread?

- A) Durable Functions runs each step on a separate thread pool with a 5-minute timeout per thread.
- B) The orchestrator function suspends at each `await` call and persists its state to Azure Storage. The thread is freed. When the activity completes, the orchestrator replays to resume from the saved checkpoint.
- C) Durable Functions allocates a dedicated VM for each orchestration instance.
- D) The orchestrator polls the activity function every 30 seconds until it completes.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Durable Functions uses an event sourcing / checkpointing pattern. When the orchestrator awaits an activity, it serializes its state to Azure Storage and releases the thread. When the activity completes, the runtime replays the orchestrator function from the beginning, fast-forwarding through completed steps using the event history, until it reaches the next pending await. This enables long-running workflows without holding threads.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer deploys an HTTP-triggered Azure Function on a Consumption plan. Under moderate load, the first few requests each morning experience 4–8 second response delays, while subsequent requests are fast. What is the root cause?

- A) The Consumption plan enforces a 100 concurrent request limit during off-peak hours.
- B) Cold starts — when the function has been idle and scaled to zero, the first request must initialize the host, load the runtime and dependencies, and compile startup code before processing begins.
- C) HTTP trigger functions are single-threaded and cannot handle concurrent requests.
- D) The Consumption plan does not support HTTP triggers for external traffic.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The Consumption plan scales to zero after a period of inactivity. The first request triggers a **cold start** where the Azure Functions host boots, the language runtime (e.g., .NET, Node.js) initializes, and startup code executes — adding seconds of latency. Mitigation options include: switching to the **Premium plan** (pre-warmed instances), enabling **Always On** on a Dedicated App Service plan, or minimizing startup code. Consumption plans fully support HTTP triggers and concurrent requests.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team needs deployment slots (staging → production swap), auto-scaling, and custom SSL on their Azure App Service web app. They are currently on a Standard S2 plan. Are all three features available?

- A) No — deployment slots and auto-scaling require the Premium plan.
- B) Yes — Standard tier supports up to 5 deployment slots, custom SSL bindings, and metric-based auto-scaling rules (CPU, memory, HTTP queue length).
- C) Yes, but SSL certificates require a separate Premium add-on; Standard only supports HTTP.
- D) No — deployment slots are only available on the Isolated (ASE) tier.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The Standard App Service tier supports: up to **5 deployment slots**, **custom domain SSL/TLS bindings** (including App Service Managed Certificates), and **auto-scale rules** based on metrics such as CPU percentage, memory, and HTTP queue length. Premium (P1–P3) increases slot count to 20, adds VNet integration without restrictions, and provides faster hardware. Isolated (ASE) adds full network isolation in a dedicated environment.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Function uses a Service Bus trigger. Messages occasionally fail due to a downstream service being unavailable. After 10 failed delivery attempts, messages are being silently lost. What configuration prevents data loss?

- A) Increase the function timeout in `host.json` to allow more processing time per attempt.
- B) Configure the Service Bus queue\'s `maxDeliveryCount` and ensure a **Dead-Letter Queue (DLQ)** is enabled; after exceeding delivery attempts, messages are automatically moved to the DLQ. Set up a monitoring alert on DLQ message count for manual review and reprocessing.
- C) Replace the Service Bus trigger with an Azure Blob Storage trigger for better reliability.
- D) Wrap function code in try/catch and swallow exceptions to prevent the delivery count from incrementing.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Service Bus automatically moves messages to the **Dead-Letter Queue** (`$DeadLetterQueue`) after the `maxDeliveryCount` is exceeded — messages are **not** lost. The DLQ preserves the message with the dead-letter reason and description. Setting an **Azure Monitor alert** on DLQ message count ensures the team is notified for investigation and reprocessing. Swallowing exceptions (Option D) hides failures and risks data loss. Increasing timeout doesn\'t prevent delivery count exhaustion.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. After swapping a staging slot to production in Azure App Service, the production app starts using the staging database connection string instead of the production one. What configuration prevents this?

- A) App settings always swap with the deployment — this is expected behavior that must be corrected manually after each swap.
- B) Mark the database connection string as a **Deployment slot setting** (slot-sticky). Slot-sticky settings remain bound to their slot and do not move during swaps; non-sticky settings swap with the application code.
- C) Store connection strings in the app binary so they are fixed and never swapped.
- D) Disable slot swaps and use CI/CD to deploy directly to production.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure App Service distinguishes between **slot-sticky** settings (marked as "Deployment slot setting") and non-sticky settings. Sticky settings are permanently bound to a specific slot — they remain in place during a swap. This is the intended mechanism for environment-specific values (e.g., staging DB string stays in staging, production DB string stays in production) while application code travels through the swap. Non-sticky settings (feature flags, config values) swap along with the code.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 15. Containers and Kubernetes

<br>

## Q. A DevOps team has just deployed a new container image to their AKS cluster. After the deployment, some pods are crashing with status `CrashLoopBackOff`. What is the first command they should run to diagnose the issue?

- A) `kubectl delete pod <pod-name>`
- B) `kubectl logs <pod-name> --previous`
- C) `kubectl scale deployment <name> --replicas=0`
- D) `az aks upgrade --kubernetes-version`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

`kubectl logs <pod-name> --previous` retrieves the logs from the previous (crashed) container instance, which usually contains the error that caused the crash. `kubectl describe pod <pod-name>` also reveals events and resource constraints. Deleting the pod restarts it but loses current diagnostic data.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An AKS-hosted microservice needs to autoscale the number of pods based on the number of messages in an Azure Service Bus queue — not based on CPU or memory. Which AKS feature supports this?

- A) Horizontal Pod Autoscaler (HPA) with a custom CPU metric
- B) Vertical Pod Autoscaler (VPA)
- C) KEDA (Kubernetes Event-Driven Autoscaling) with an Azure Service Bus scaler
- D) AKS Cluster Autoscaler

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

KEDA is a CNCF project integrated into AKS that scales pods based on external event sources including Azure Service Bus queue/topic depth, Azure Storage queues, Event Hubs, and 50+ other scalers. HPA can only scale on CPU/memory or custom Kubernetes metrics, not external Azure queue depth natively.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company stores container images in Azure Container Registry (ACR). Only the AKS cluster should be able to pull images from this private registry, with no long-lived credentials stored in the cluster. What is the recommended configuration?

- A) Create a Kubernetes `imagePullSecret` with the ACR admin username and password.
- B) Make the ACR registry public so no credentials are needed.
- C) Attach the ACR to the AKS cluster using `az aks update --attach-acr`, which grants the cluster\'s managed identity the AcrPull role on the ACR.
- D) Store the ACR access token in an Azure Key Vault secret and reference it in pod specs.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

`az aks update --attach-acr <acr-name>` grants the AKS cluster\'s managed identity (kubelet identity) the `AcrPull` role on the ACR. Pods can then pull images without any imagePullSecret. No passwords or tokens are stored in the cluster. Admin credentials in imagePullSecrets are a security anti-pattern.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Your AKS workload processes sensitive financial data. A security requirement states that no pod should run as root, all file systems should be read-only unless explicitly required, and privilege escalation must be blocked. Which Kubernetes mechanism enforces these constraints cluster-wide?

- A) Kubernetes Network Policies
- B) Azure Policy for AKS with built-in Pod Security Standards (Restricted profile)
- C) AKS node pool taints
- D) Resource Quotas and LimitRanges

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Policy for AKS integrates with the OPA Gatekeeper admission controller to enforce Pod Security Standards at the cluster level. The Restricted profile enforces: no root users, read-only root filesystems, no privilege escalation, no host namespaces. Network Policies control network traffic, not container security context. Taints control pod scheduling placement, not security posture.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team wants to minimize AKS node pool costs for a batch workload that runs for 4 hours each night. The nodes should not exist (and not incur cost) outside of the batch window. Which AKS feature supports this?

- A) AKS Cluster Autoscaler with a minimum node count of 0
- B) AKS node pool with `--node-count 0` and start/stop the cluster via `az aks start/stop`
- C) Use Azure Container Instances for the batch workload instead of AKS.
- D) AKS Spot node pools with automatic eviction

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

AKS supports stopping and starting the entire cluster or individual user node pools. When stopped, you pay only for the persistent storage (OS disks, etcd) but not for VM compute. Combined with a scheduled automation (Logic Apps or Azure Automation), the cluster starts before the batch window and stops after, eliminating idle VM costs.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 16. DevOps and CI/CD

<br>

## Q. A development team uses Azure DevOps pipelines to build and deploy a .NET application. The pipeline currently deploys directly to production on every merge to `main`. The team wants to add a manual approval gate before the production deployment. Which Azure DevOps feature implements this?

- A) Pipeline branch filters
- B) Azure DevOps Environment with a required approval check
- C) Repository branch protection rules
- D) YAML template parameters

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure DevOps Environments support pre-deployment checks including manual approvals and required reviewers. When a deployment stage targets an environment with an approval check, the pipeline pauses and sends a notification to approvers before proceeding. Branch filters control when the pipeline triggers, not deployment approval.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team\'s Azure DevOps pipeline needs to deploy to Azure using a Service Principal. Currently the Client Secret is stored as a pipeline variable in plain text. The security team requires secrets to never be stored in Azure DevOps. What is the recommended approach?

- A) Store the secret in a pipeline variable group marked as secret.
- B) Use an Azure Resource Manager Service Connection (with Workload Identity Federation / OIDC), which does not require storing a client secret in Azure DevOps at all.
- C) Rotate the client secret every 30 days automatically.
- D) Encode the client secret in Base64 before storing it in the pipeline variable.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Workload Identity Federation (OIDC) for Azure DevOps ARM Service Connections uses short-lived tokens issued by Azure DevOps, validated by Microsoft Entra ID — no client secret is stored anywhere. The pipeline authenticates to Azure using federated identity without any long-lived credential. This is the current Microsoft-recommended approach.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to enforce that all Azure resources deployed through their CI/CD pipeline are tagged with `CostCenter`, `Environment`, and `Owner` tags. Resources deployed without these tags should be automatically rejected. Which approach enforces this at the Azure control plane level?

- A) Add a pipeline script step that checks tags before deployment.
- B) Assign an Azure Policy with the `deny` effect that requires the specified tags on all resource creation/update operations.
- C) Use Azure Blueprints to tag resources after deployment.
- D) Enable Microsoft Defender for Cloud resource compliance.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Policy with `deny` effect blocks any resource deployment that does not include the required tags, regardless of how the deployment is triggered (portal, CLI, pipeline, Terraform). Pipeline-level checks can be bypassed by out-of-band deployments. Policy enforcement at the ARM control plane is authoritative.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team manages Azure infrastructure with Bicep templates stored in a Git repository. They want to automatically validate and deploy changes to infrastructure when a pull request is merged to `main`. Which pipeline structure is most appropriate?

- A) A single stage pipeline that runs `az deployment group create` on every commit to every branch.
- B) A multi-stage YAML pipeline: Stage 1 (PR) runs `az deployment group validate` and `az deployment group what-if`; Stage 2 (merge to main) runs `az deployment group create` with environment approval.
- C) Use Azure Blueprints to auto-deploy on repository changes.
- D) A release pipeline triggered by a build artifact containing the Bicep files.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The recommended IaC pipeline pattern: validate (syntax check) and what-if (preview changes) on PRs to catch errors before merge; deploy to production after merge with an environment approval gate. This provides safety, visibility, and auditability without deploying untested changes automatically.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A GitHub Actions workflow deploys to Azure App Service on every push to `main`. The team wants the deployment to roll back automatically if the App Service health check fails after deployment. Which GitHub Actions feature supports this?

- A) GitHub Actions `continue-on-error: true` flag
- B) The `azure/webapps-deploy` action with slot swap and App Service Health Check configured on the staging slot; if the health check fails, the swap does not proceed.
- C) GitHub Dependabot auto-merge
- D) GitHub Environment protection rules with a required status check

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Deploying to a staging slot and using slot swap (with Auto swap or a manual swap step in the pipeline) combined with App Service Health Check means Azure validates the new version on the staging slot before swapping to production. If the health check fails, the slot remains at staging and production is unaffected — providing automatic rollback without additional tooling.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 18. Cost Management

<br>

## Q. A company\'s Azure bill is unexpectedly 40% higher than the previous month. The finance team asks you to investigate. Which is the most direct first step?

- A) Delete all resources in non-production resource groups immediately.
- B) Open Azure Cost Management + Billing → Cost Analysis, filter by time period, group by Service and Resource Group to identify which service and resource group drove the increase.
- C) Scale down all VM sizes to the smallest available SKU.
- D) Enable Azure Advisor and wait for recommendations to appear.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Cost Analysis in Azure Cost Management provides a visual breakdown of costs by service, resource group, resource, tag, and location. Grouping by service and resource group quickly identifies the source of unexpected spend. Taking destructive actions before diagnosing the root cause risks business disruption.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A production workload runs 24/7 on 10 Azure D8s_v5 VMs in East US. The workload has been stable for 18 months with no plans to change VM size or region. What is the most cost-effective purchasing option?

- A) Pay-as-you-go pricing
- B) 3-year Azure Reserved VM Instance with no upfront payment
- C) 3-year Azure Reserved VM Instance with all-upfront payment
- D) Azure Spot VMs

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

For stable, long-running 24/7 workloads, 3-year Reserved Instances with all-upfront payment provide the maximum discount (up to 72% vs pay-as-you-go). The all-upfront option has a larger discount than the monthly payment option. Spot VMs can be evicted and are inappropriate for production workloads. Pay-as-you-go is the most expensive for constant usage.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A development team leaves 15 development VMs running overnight and on weekends when no one is working. How can you automatically shut down these VMs during off-hours to reduce cost without deleting them?

- A) Use Azure Policy to deny VM creation outside business hours.
- B) Enable Auto-shutdown on each VM (or via Azure Automation runbooks with schedules) to stop them at 7 PM on weekdays and restart at 8 AM.
- C) Move the VMs to Azure Spot to reduce cost when idle.
- D) Enable Azure Advisor and configure it to shut down idle VMs.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure VMs support a built-in Auto-shutdown feature configurable per VM. Azure Automation with PowerShell runbooks and schedules can manage start/stop at scale across all dev VMs. Stopped (deallocated) VMs do not incur compute charges. Azure Advisor recommends but does not automatically shut down resources.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company has Windows Server 2022 VMs running SQL Server Enterprise on-premises with active Software Assurance licenses. They are migrating to Azure VMs. Which Azure benefit allows them to reuse existing licenses and avoid paying for new Azure licensing costs?

- A) Azure Hybrid Benefit
- B) Azure Reserved Instances
- C) Azure Dev/Test pricing
- D) Azure Spot pricing

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Azure Hybrid Benefit allows customers with active Software Assurance on Windows Server and SQL Server licenses to run those workloads on Azure VMs at a reduced rate (paying only compute, not the OS or SQL Server license again). This can save up to 85% compared to pay-as-you-go pricing with included licenses.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A FinOps team wants to alert the Finance team automatically when monthly Azure spending in the Production subscription exceeds $50,000, so corrective action can be taken before the end of the month. Which feature enables this?

- A) Azure Monitor metric alert on billing metrics
- B) Azure Cost Management Budget with an alert action group configured to email the Finance team when 80% and 100% of the $50,000 budget is reached.
- C) Azure Policy with a cost threshold deny effect
- D) Microsoft Defender for Cloud cost recommendations

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Cost Management Budgets allow you to define a spending threshold (e.g., $50,000/month) and trigger alerts at configurable percentages (e.g., 80%, 100%) via email or action groups. Budgets also support forecast-based alerts (when Azure predicts you'll exceed the budget). Azure Policy cannot block spending; it controls resource configurations.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. High Availability and Disaster Recovery

<br>

## Q. A company\'s Azure SQL Database in East US experiences an outage. They have a failover group configured with a secondary in West US. The application connection string uses the failover group listener endpoint. What happens to the application during a database failover?

- A) The application must be redeployed with the secondary database connection string.
- B) The failover group listener endpoint automatically resolves to the new primary (West US) after failover; the application reconnects without a connection string change.
- C) The application must manually trigger failover via the Azure portal.
- D) The connection string becomes invalid and must be updated with the secondary endpoint.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Failover group listener endpoints (read-write and read-only) are DNS names that remain constant. After failover, the DNS record is updated to point to the new primary. Applications using the listener endpoint reconnect transparently after a brief reconnection retry — no connection string change required.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company needs their Azure-hosted application to have an RTO (Recovery Time Objective) of less than 15 minutes and RPO (Recovery Point Objective) of less than 5 minutes in the event of a full region failure. Which design best meets both objectives?

- A) Daily backup of VMs to Azure Backup in the same region.
- B) Active-passive multi-region deployment: primary region runs the workload; Azure Site Recovery continuously replicates VMs to a secondary region (RPO ~1 min); Azure Traffic Manager fails over DNS to the secondary region (RTO ~5–15 min).
- C) Weekly geo-redundant backup with manual restore in the secondary region.
- D) Azure Backup with cross-region restore (CRR) enabled.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Site Recovery with continuous replication achieves RPO of approximately 1 minute. Azure Traffic Manager with health probes detects the primary region failure and reroutes traffic to the pre-warmed secondary region in minutes, meeting the 15-minute RTO. Daily or weekly backups cannot meet a 5-minute RPO. Cross-region restore with Azure Backup takes hours.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer asks: "What is the difference between RTO and RPO?" Which answer is correct?

- A) RTO is the maximum data loss tolerated; RPO is the maximum downtime tolerated.
- B) RTO (Recovery Time Objective) is the maximum acceptable downtime — how long it takes to restore service; RPO (Recovery Point Objective) is the maximum acceptable data loss — how old the most recent backup/replica can be.
- C) RTO applies to databases only; RPO applies to VMs only.
- D) RTO and RPO are the same thing measured in different units.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

RTO answers "how long can the system be down?" — a 15-minute RTO means service must be restored within 15 minutes. RPO answers "how much data can we afford to lose?" — a 5-minute RPO means the data can be at most 5 minutes old at the time of recovery. Both are business requirements that drive the choice of DR solution.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An e-commerce application needs 99.99% monthly uptime SLA. It is currently deployed on a single Azure App Service in one region. What architectural change is required to achieve the 99.99% SLA?

- A) Upgrade to the App Service Premium v3 tier in the same region.
- B) Deploy the App Service to two Azure regions, place Azure Front Door in front with health probes, and use a geo-replicated database backend. The combined SLA of redundant components exceeds 99.99%.
- C) Enable App Service Always-On setting and increase the instance count to 3.
- D) Configure App Service with ZRS storage for the content share.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A single-region App Service has a maximum SLA of 99.95%. Achieving 99.99% requires multi-region active-active or active-passive deployment with a global load balancer (Azure Front Door, 99.99% SLA) routing traffic. The composite SLA of independent redundant components (A AND B fail = 0.05% × 0.05% = 0.0025%) far exceeds 99.99%.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company runs a 3-tier application (web, app, database) on Azure VMs. They want to protect against a full datacentre failure within the same Azure region (not a full region failure). Which redundancy configuration achieves this?

- A) Deploy all VMs in the same Availability Set.
- B) Deploy each tier\'s VMs across multiple Availability Zones within the same region (zone-redundant deployment).
- C) Deploy VMs in a single Availability Set in one fault domain.
- D) Use Azure Backup with cross-region restore.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Availability Zones are physically separate datacentres within an Azure region with independent power, cooling, and networking. Deploying each tier across 2–3 AZs ensures that a single datacentre failure does not bring down the application. Availability Sets protect against rack-level failures within a single datacentre but not datacentre-level failures.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. Migration

<br>

## Q. A company wants to migrate 200 on-premises VMs to Azure. Before committing to migration, they need an assessment of which VMs are ready for Azure, the recommended Azure VM sizes, and the estimated monthly cost. Which tool provides this?

- A) Azure Cost Management pricing calculator
- B) Azure Migrate: Discovery and Assessment
- C) Azure Site Recovery
- D) Azure Database Migration Service

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Migrate: Discovery and Assessment deploys a lightweight appliance on-premises that discovers VMs (VMware, Hyper-V, or physical servers), collects performance data, and produces an Azure readiness report with recommended VM SKUs and estimated costs. Azure Site Recovery performs the actual replication/migration, not the assessment.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company is migrating a 10 TB on-premises SQL Server 2019 database to Azure SQL Managed Instance with minimal downtime (less than 30 minutes). Which migration approach achieves this?

- A) Take a full backup, restore it to Azure SQL MI (offline migration — downtime equals restore time, likely hours).
- B) Use Azure Database Migration Service (DMS) in online mode with Log Shipping / Change Data Capture to sync changes continuously, then perform a cutover during a short maintenance window.
- C) Export to BACPAC, import into Azure SQL MI (BACPAC is unsupported for MI at scale).
- D) Use Azure Data Factory to copy all tables row-by-row to Azure SQL MI.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

DMS online mode uses log shipping (for SQL Server) to continuously sync transaction log changes to the target MI after the initial full backup restore. When the lag is near-zero, the team schedules a short cutover window (typically <30 minutes) to stop writes on the source, let the final logs drain, and redirect the application to the new endpoint. Offline migration requires the source database to be taken offline for the entire restore duration.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company needs to transfer 500 TB of on-premises archive data to Azure Blob Storage. Their internet connection is 1 Gbps and is shared with production traffic. Over the network, this transfer would take over 6 weeks. Which Azure service provides the fastest path?

- A) AzCopy with parallel threads over ExpressRoute
- B) Azure Data Box Heavy (1 PB capacity physical device shipped to the datacenter)
- C) Azure Import/Export service with standard HDDs shipped by the customer
- D) Azure Data Factory with self-hosted Integration Runtime

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Data Box Heavy is a ruggedized 1 PB storage device shipped by Microsoft. The team copies data locally (up to 1 Gbps USB), ships the device back, and Microsoft uploads data directly into Azure Storage at datacenter speeds. For 500 TB, this is orders of magnitude faster than a 1 Gbps internet connection shared with production traffic.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A legacy .NET Framework 4.7 web application runs on IIS on Windows Server 2012. The team wants to lift-and-shift to Azure with minimal changes. Which Azure service is most appropriate?

- A) Azure App Service (Windows) — .NET Framework 4.7 is supported
- B) Azure Kubernetes Service with a Windows node pool
- C) Azure Virtual Machines running Windows Server — migrate the VM as-is using Azure Migrate
- D) Azure Functions with .NET Framework isolated worker

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

For a true lift-and-shift with minimal changes, Azure Migrate replicates the on-premises IIS/Windows Server VM to an Azure VM. No code changes are required. Azure App Service supports .NET Framework but may require configuration changes (no IIS configuration file support for all features). AKS requires containerization. Azure Functions does not support full IIS-hosted apps.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. During a cloud migration, a company discovers that their application connects to a third-party on-premises API that cannot be moved to Azure due to a vendor contract. The Azure-hosted application must securely call this on-premises API. What is the recommended connectivity approach?

- A) Expose the on-premises API on the public internet and restrict by IP.
- B) Establish an Azure VPN Gateway Site-to-Site or ExpressRoute connection to the on-premises network, allowing the Azure application to call the private on-premises API endpoint securely.
- C) Use Azure API Management with a self-hosted gateway deployed on-premises.
- D) Both B and C are valid approaches — B for network connectivity, C for API management features like throttling, auth, and caching at the edge.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: D**

B (VPN/ExpressRoute) establishes the private network path from Azure to on-premises. C (API Management self-hosted gateway) deploys the gateway component on-premises close to the backend API, adding management features (rate limiting, auth, caching, logging) without requiring the API to be on the internet. Both can be combined for a complete solution.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 19. AI and Cognitive Services

<br>

## Q. A development team wants to add natural language search capabilities to their Azure-hosted product catalog so users can query in plain English (e.g., "red running shoes under $100"). Which Azure service combination implements this?

- A) Azure Cognitive Search with semantic search + Azure OpenAI Service for query understanding
- B) Azure SQL Database full-text search
- C) Azure Table Storage with wildcard queries
- D) Azure Bot Service with LUIS (Language Understanding)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Azure AI Search (formerly Cognitive Search) with semantic ranking understands query intent beyond keyword matching. Combined with Azure OpenAI Service (e.g., GPT-4o for query enrichment or vector embeddings for semantic similarity search), it enables natural language product discovery. Azure SQL full-text search is keyword-based and does not understand query semantics.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to build a chatbot grounded in their own internal documentation (PDFs, Word files, SharePoint content) using Azure OpenAI Service. The chatbot must only answer from their documents and cite sources. Which Azure pattern implements this?

- A) Fine-tune a GPT model with the company\'s documents.
- B) Retrieval Augmented Generation (RAG): index documents in Azure AI Search (with vector embeddings), retrieve relevant chunks for each user query, and pass them as context to Azure OpenAI\'s chat completion API.
- C) Store all documents in Azure Blob Storage and pass the entire storage account URL to the OpenAI API.
- D) Use Azure Cognitive Services Text Analytics to extract key phrases and feed them to a rule-based chatbot.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

RAG is the recommended pattern for grounding LLM responses in enterprise documents. Documents are chunked and vectorized (using OpenAI Ada embeddings or other models) and indexed in Azure AI Search. At query time, the most relevant chunks are retrieved and injected into the OpenAI prompt as context. This is more cost-effective and up-to-date than fine-tuning, and avoids hallucinations by grounding responses in retrieved text.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An application calls Azure OpenAI Service for completions and occasionally receives HTTP 429 (Too Many Requests) errors during peak hours. The team has a provisioned throughput deployment (PTU). What is the recommended way to handle this without losing requests?

- A) Catch the 429 error and immediately retry with a fixed 1-second delay.
- B) Implement exponential backoff with jitter on retry, and consider overflow to a pay-as-you-go deployment when PTU capacity is exhausted.
- C) Switch from PTU to pay-as-you-go to avoid rate limits entirely.
- D) Increase the `max_tokens` parameter to reduce the number of API calls.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure OpenAI recommends exponential backoff with jitter for 429 errors to avoid retry storms. For PTU deployments, overflow routing to a PAYG endpoint handles burst traffic beyond provisioned capacity. Fixed-interval retries worsen the problem under sustained load. Increasing `max_tokens` increases per-call cost/latency but does not reduce the rate of calls.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A healthcare company wants to extract structured information (patient name, diagnosis, medication, dosage) from unstructured clinical notes stored in Azure Blob Storage. Which Azure AI service is purpose-built for healthcare NLP?

- A) Azure OpenAI Service with a custom prompt
- B) Azure AI Language — Text Analytics for Health
- C) Azure Form Recognizer (Document Intelligence)
- D) Azure Cognitive Search with custom skills

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure AI Language\'s **Text Analytics for Health** is a pre-trained NLP model specifically designed to extract and structure healthcare entities (conditions, medications, dosages, anatomy, test results) from clinical text in compliance with healthcare standards. Form Recognizer extracts structured fields from forms/documents by layout, not by healthcare-specific NLP.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company trains a custom image classification model using Azure Machine Learning. After training, they want to expose the model as a REST API endpoint that auto-scales based on request volume and has no idle compute cost. Which Azure ML deployment target achieves this?

- A) Azure Machine Learning Managed Online Endpoint with Kubernetes compute
- B) Azure Machine Learning Serverless Online Endpoint (pay-per-call)
- C) Azure Container Instances with the model Docker image
- D) Deploy the model to Azure Functions as a Python function

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Machine Learning Serverless Online Endpoints (in preview/GA 2025) provide fully managed, auto-scaling inference with per-call billing — no idle compute charges. Managed Online Endpoints with dedicated compute incur costs even when idle. ACI is non-managed and does not auto-scale. Azure Functions imposes memory and timeout limits unsuitable for large ML models.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. Governance and Compliance

<br>

## Q. An organization wants to ensure that all new Azure resources are created only in approved regions (East US and West Europe) and that resource groups without a required `Owner` tag cannot be created. Which Azure service enforces these constraints declaratively at the control plane?

- A) Azure Role-Based Access Control (RBAC)
- B) Azure Policy with `deny` effect definitions
- C) Azure Blueprints
- D) Azure Advisor recommendations

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Policy with `deny` effect prevents non-compliant resource creation or updates at the ARM control plane. Built-in policies exist for allowed locations and required tags. RBAC controls who can perform operations but not what resource configurations are allowed. Blueprints orchestrate policy assignment but the enforcement mechanism is still Policy.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A security team discovers that a storage account is publicly accessible (blob public access enabled). They remediate it manually, but the same misconfiguration keeps reappearing. Which Azure Policy capability automatically remediates non-compliant resources without manual intervention?

- A) Azure Policy `audit` effect with email notifications
- B) Azure Policy `deployIfNotExists` or `modify` effect with a remediation task and a managed identity.
- C) Microsoft Defender for Cloud security recommendations
- D) Azure Advisor security score improvements

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Policy with `modify` effect can automatically correct resource properties (like setting `allowBlobPublicAccess: false`) on existing non-compliant resources via a remediation task. `deployIfNotExists` deploys a companion resource if missing. The policy is assigned a managed identity with the appropriate RBAC to make the change. `audit` only flags the issue without fixing it.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An enterprise has 5 business units, each with multiple subscriptions. The security team needs to apply consistent policies and RBAC across all 50+ subscriptions from a single location, with different policies for Production vs Non-Production subscriptions. What management structure achieves this?

- A) Assign policies to each subscription individually.
- B) Create a Management Group hierarchy: Root → Production MG → [prod subscriptions]; Root → Non-Production MG → [non-prod subscriptions]. Assign policies at each MG level; they inherit down.
- C) Use Azure Blueprints on each subscription separately.
- D) Create a single subscription with resource group-based separation.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Management Group hierarchies enable governance at scale. Policies and RBAC assigned to a Management Group automatically inherit to all child Management Groups and subscriptions. The Production and Non-Production MGs can have different policy sets (e.g., stricter network policies in Production), with shared baseline policies assigned at the Root MG.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An organization must demonstrate SOC 2 Type II compliance for their Azure-hosted SaaS application. They need a centralized dashboard showing which Azure controls are compliant and which have gaps. Which tool provides this?

- A) Azure Cost Management compliance reports
- B) Microsoft Defender for Cloud with Regulatory Compliance dashboard (SOC 2 standard)
- C) Azure Monitor Workbooks with compliance queries
- D) Azure Service Health compliance certificates

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Microsoft Defender for Cloud includes a Regulatory Compliance dashboard that maps Azure security controls to compliance standards including SOC 2, ISO 27001, PCI DSS, HIPAA, CIS, and NIST. It shows compliant vs non-compliant controls in real-time and provides remediation guidance, supporting audit evidence collection.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team accidentally deleted a critical Azure resource group containing production resources. Which Azure feature would have prevented this, and which feature could help with recovery if prevention was not in place?

- A) Prevention: Azure Backup; Recovery: Azure Site Recovery
- B) Prevention: Azure Resource Locks (`CanNotDelete` lock on the resource group); Recovery: Azure Resource Graph query to find deleted resources + restore from Azure Backup snapshots.
- C) Prevention: Azure Policy `deny` effect; Recovery: redeploy from ARM templates.
- D) Prevention: RBAC removing delete permissions; Recovery: Azure Activity Log replay.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A `CanNotDelete` resource lock prevents deletion of a resource or resource group even by subscription Owners, until the lock is explicitly removed. For recovery, Azure Backup point-in-time restore (for VMs, SQL, blobs) restores data. Azure Resource Graph can identify what was in the deleted resource group from historical data. The Activity Log records the delete event for audit purposes.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A governance team needs to block all new Azure resource deployments outside East US and West Europe, while only reporting (not blocking) existing non-compliant resources in other regions. Which Azure Policy configuration achieves this?

- A) A single policy with the `Modify` effect that automatically tags non-compliant resources for scheduled deletion.
- B) Assign a policy with the **`Deny` effect** (blocks new deployments outside allowed regions) and a **separate policy with the `Audit` effect** (reports existing non-compliant resources in the compliance dashboard without deleting them), both assigned at the Management Group level.
- C) Use `DeployIfNotExists` to redeploy non-compliant resources to the approved regions automatically.
- D) Use a single policy with both `Deny` and `Audit` as compound effects in the same rule definition.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Policy effects are mutually exclusive per assignment: **`Deny`** prevents non-compliant resource creation or updates at deployment time; **`Audit`** generates a compliance entry for existing non-compliant resources without blocking. The common governance pattern uses `Deny` for new resources and `Audit` for existing ones — often deployed as an initiative (policy set). `Modify` changes resource properties (tags, settings) but cannot relocate resources to different regions. `DeployIfNotExists` triggers deployments for missing configurations (e.g., missing diagnostic settings), not for regional placement. Azure Policy does not support compound effects in a single assignment.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer accidentally commits an Azure SQL Database connection string (including the password) to a public GitHub repository. The security team wants to eliminate this risk permanently for all applications. What is the recommended architecture?

- A) Detect committed credentials using GitHub Advanced Security secret scanning and rotate them immediately after detection.
- B) Store all connection strings and secrets in **Azure Key Vault**; grant each application\'s **Managed Identity** the `Get` secret permission on Key Vault; retrieve secrets at runtime via the Key Vault SDK or **App Service Key Vault references** (`@Microsoft.KeyVault(SecretUri=...)`), eliminating credentials from code, config files, and environment variables.
- C) Base64-encode all connection strings and store them in environment variables to obfuscate them.
- D) Store connection strings in Azure App Configuration with access control instead of Key Vault.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Key Vault + Managed Identity** is the gold-standard secrets management pattern. The application authenticates to Key Vault using its Managed Identity — no credentials are required or stored anywhere. Secrets never appear in source code, config files, CI/CD pipelines, or container images. **App Service Key Vault references** allow `@Microsoft.KeyVault(SecretUri=...)` syntax in App Settings, transparently resolving secrets at runtime. Option A (secret scanning) detects violations but doesn\'t prevent the root cause. Base64 encoding (Option C) provides zero security — it is trivially reversible. Azure App Configuration (Option D) is designed for feature flags and non-sensitive application settings, not secrets.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A production AKS cluster experiences intermittent pod crashes in the `payment-service` namespace. The operations team needs to query container logs from the past 24 hours, filter for ERROR entries, and create an automated alert when error count exceeds 50 in any 5-minute window. Which Azure services and tools enable this end-to-end?

- A) Azure Security Center → Threat Detection for container anomaly alerts.
- B) Enable **Azure Monitor Container Insights** on the AKS cluster to stream logs to a **Log Analytics workspace**; run the KQL query: `ContainerLogV2 | where PodNamespace == "payment-service" and LogMessage contains "ERROR" | summarize ErrorCount = count() by bin(TimeGenerated, 5m)`; create a **Log Analytics scheduled query alert rule** with threshold `ErrorCount > 50`.
- C) Use Azure Service Health to monitor cluster-level pod crash events.
- D) Enable Azure Diagnostics on AKS worker node VMs and query with Azure Storage Analytics logs.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Monitor Container Insights** collects container stdout/stderr logs, performance metrics (CPU, memory), and Kubernetes events into a **Log Analytics workspace**. **KQL (Kusto Query Language)** enables sophisticated log queries filtered by namespace, pod, and time window. **Scheduled query alert rules** run KQL on a defined cadence and trigger **Action Groups** (email, SMS, webhook, PagerDuty, Azure DevOps) when thresholds are breached. Azure Service Health (Option C) monitors Azure platform status, not application container logs. Querying VM-level diagnostics (Option D) doesn\'t surface container-level namespace-scoped log data effectively.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A Function App\'s performance degraded after a recent deployment. The team suspects a specific downstream dependency (a REST API call) is adding significant latency. They need to see a visual trace of each request showing all operations (HTTP calls, SQL queries, queue reads) with individual durations to identify the bottleneck. Which Azure service provides this?

- A) Azure Network Watcher with connection troubleshooting to measure hop-by-hop latency.
- B) **Azure Application Insights** with distributed tracing — enable auto-instrumentation on the Function App; view the **End-to-End Transaction** (Gantt chart) in the portal showing the full dependency call tree with durations, status codes, and exceptions for every operation in the request.
- C) Azure Monitor Metrics with a custom metric emitted for each dependency call duration.
- D) Azure Log Analytics with `Console.WriteLine` timing statements parsed as custom logs.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Application Insights** provides **distributed tracing** with automatic instrumentation for Azure Functions. It captures the full dependency call tree — outbound HTTP, SQL, Service Bus, Blob Storage — as a hierarchical **End-to-End Transaction** with individual durations, HTTP status codes, and exceptions. The visual Gantt chart immediately highlights the slowest component. Application Insights integrates natively with Azure Functions via the `APPLICATIONINSIGHTS_CONNECTION_STRING` app setting. Azure Monitor Metrics (Option C) provides aggregate statistics but not per-request call tree traces. Manual `Console.WriteLine` logging (Option D) requires custom parsing and provides no visual trace correlation.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 17. ARM Templates and Bicep Syntax

<br>

## Q. A team deploys an ARM template to production using Complete mode. Afterward, they discover that a manually created blob container used for shared configuration was deleted. What caused this?

- A) Incremental mode deletes resources not in the template.
- B) Complete mode makes the resource group exactly match the template — any resource in the resource group not defined in the template is permanently deleted.
- C) Validate mode applies deletions to untracked resources automatically.
- D) WhatIf mode applied the deletions when previewing the deployment.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

ARM\'s **Complete** deployment mode reconciles the resource group to exactly match the template — resources present in the group but absent from the template are deleted. **Incremental** mode (the safe default) only creates or updates template-defined resources and never removes existing ones. **Validate** and **WhatIf** are read-only analysis operations and make no changes. Always use Incremental mode in production unless a full reconciliation is intentionally required.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A team migrates to Bicep and has a `storage.bicep` module that creates a storage account. A `webapp.bicep` module needs the storage account\'s blob endpoint URL. How is the output passed between modules in the parent Bicep template?

- A) Use the `reference()` function with the storage account\'s resource ID inside `webapp.bicep`.
- B) Declare `output storageEndpoint string = storageAccount.properties.primaryEndpoints.blob` in `storage.bicep`, then reference it in the parent as `storageModule.outputs.storageEndpoint` when passing it to `webapp.bicep`.
- C) Hardcode the expected storage endpoint URL in a shared `parameters.json` file.
- D) Use `listKeys()` inside `webapp.bicep` to retrieve the endpoint directly.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Bicep modules expose values using the `output` keyword. The parent template consumes a module\'s output via `<moduleName>.outputs.<outputName>`, creating an implicit dependency that enforces correct deployment order. This is clean, type-safe, and idiomatic Bicep. `reference()` is ARM JSON syntax. Hardcoding endpoints breaks automation. `listKeys()` retrieves storage account keys, not endpoints.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer wants to preview exactly what resources will be created, modified, or deleted before applying a Bicep deployment. Which command achieves this?

- A) `az deployment group validate --template-file main.bicep`
- B) `az deployment group what-if --resource-group myRG --template-file main.bicep`
- C) `az deployment group create --template-file main.bicep --dry-run`
- D) `az bicep build --file main.bicep`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

`az deployment group what-if` performs a pre-deployment impact analysis and displays color-coded output showing resources that will be **created** (green `+`), **modified** (orange `~`), or **deleted** (red `-`) without making any changes. `validate` checks only for schema and syntax errors. `--dry-run` is not a valid ARM CLI flag. `bicep build` compiles Bicep to ARM JSON for inspection but does not perform deployment analysis.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A Bicep template must deploy a `Premium_LRS` storage account in production and a `Standard_LRS` account in dev. Both use the same template with an `env` parameter. Which Bicep feature implements environment-specific SKU selection without separate template files?

- A) Use a nested template with a `condition` property for each environment.
- B) Use Bicep\'s ternary operator: `sku: { name: env == 'prod' ? 'Premium_LRS' : 'Standard_LRS' }` directly in the resource property.
- C) Maintain two separate Bicep files — `prod.bicep` and `dev.bicep`.
- D) Use the `@allowed` decorator on the `env` parameter to restrict valid values.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Bicep natively supports the ternary conditional expression `condition ? trueValue : falseValue` within any resource property. A single template with an `env` parameter drives environment-specific configuration. This keeps code DRY and eliminates duplication. Separate files (Option C) create maintenance drift. `@allowed` restricts parameter values to a set but does not drive resource configuration logic. Nested templates with `condition` are an ARM JSON pattern — Bicep\'s ternary is cleaner.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. Serverless and Performance Management

<br>

## Q. A business workflow requires an Azure Function to send an approval request, then pause for hours or days waiting for a human response before continuing the workflow. The function must not time out or consume compute resources while waiting. Which Durable Functions pattern addresses this?

- A) Fan-out/Fan-in pattern — parallel execution of multiple activity functions.
- B) **Human interaction pattern** using `context.waitForExternalEvent()` in a Durable Orchestrator function — the orchestrator checkpoints its state to Azure Storage and suspends execution (no compute cost) until an external HTTP call delivers the approval event, then resumes.
- C) Chaining pattern — sequential execution of activity functions without waits.
- D) Eternal orchestration pattern — an infinite loop that periodically checks for approval.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Human Interaction pattern** in Durable Functions is specifically designed for workflows that require waiting for external events (human approval, third-party callbacks). The orchestrator calls `WaitForExternalEvent()`, checkpoints its state to Azure Storage, and releases its execution thread — **no compute cost** during the wait period. Duration can be days or weeks without timeout. An external system (e.g., approval portal) calls a Durable Functions HTTP API to raise the event, resuming the orchestrator from the checkpoint. Eternal orchestration (Option D) uses timers for polling, which consumes compute and is discouraged for long waits.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Function App on a Consumption plan processes IoT telemetry from an Azure Event Hub with 32 partitions. Under high load, only 1–3 function instances are processing messages, causing a growing backlog. How does Azure Functions scale for Event Hub triggers, and what is the maximum scale-out?

- A) Azure Functions does not support automatic scaling for Event Hub triggers — manual scaling is required.
- B) Azure Functions with an Event Hub trigger **automatically scales up to one function instance per Event Hub partition** — a maximum of 32 instances for 32 partitions. Each instance exclusively processes one partition, enabling full parallel throughput.
- C) Switching to a Premium plan allows unlimited instances beyond the partition count.
- D) Event Hub scaling requires deploying the Function App to Azure Kubernetes Service (AKS) with KEDA.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Functions with the **Event Hub trigger** uses the Event Processor Host protocol, which assigns partitions to function instances. The runtime scales out to **one instance per partition** — the maximum parallelism is bounded by partition count. For 32 partitions, up to 32 function instances process messages concurrently. This is automatic on both Consumption and Premium plans. Premium plan (Option C) removes the 200-instance Consumption cap, but partition count is still the ceiling for Event Hub parallelism. KEDA (Option D) enables Kubernetes-based scaling but is not required for standard Event Hub scaling.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Function App on a Premium EP1 plan (1 vCore, 3.5 GB RAM) consistently reports CPU utilization above 85%, causing slow response times. What is the most effective remediation?

- A) Increase the function timeout from 30 minutes to 60 minutes in `host.json`.
- B) **Scale up to EP2 or EP3** (2 or 4 vCores) to increase per-instance CPU capacity, and review function code for CPU-bound synchronous operations that can be replaced with async I/O patterns or offloaded to background processing.
- C) Place an Azure CDN in front of the Function App to reduce inbound request volume.
- D) Increase `WEBSITE_MAX_DYNAMIC_APPLICATION_SCALE_OUT` to allow more horizontal instances.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Consistent CPU utilization above 85% on a single EP1 instance indicates the workload exceeds single-instance capacity. **Scaling up** (EP2: 2 vCores, EP3: 4 vCores) directly increases per-instance CPU headroom. Code review should identify synchronous CPU-bound operations (e.g., image processing, serialization loops) that can be replaced with async patterns or offloaded to dedicated compute. `WEBSITE_MAX_DYNAMIC_APPLICATION_SCALE_OUT` (Option D) controls horizontal scale-out — adding more EP1 instances distributes load but each instance still hits the same per-instance CPU ceiling for CPU-bound operations. Increasing timeout (Option A) does not address CPU saturation.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company\'s Azure Function processes image resizing synchronously on each HTTP invocation. Each resize takes 3–5 seconds of CPU time. At 500 concurrent requests, function instances are CPU-saturated. How should the architecture be redesigned?

- A) Move the Function App to an Isolated App Service Environment (ASE) for dedicated hardware.
- B) Implement the **async offload pattern**: the HTTP trigger function accepts the request, enqueues a message to a Service Bus queue or Azure Storage Queue, returns HTTP 202 Accepted immediately, and a separate worker (second Function App, Azure Batch, or Container Instances) dequeues and performs the CPU-intensive resize asynchronously.
- C) Increase `maxConcurrentRequests` in `host.json` to handle more simultaneous resizes.
- D) Enable zone redundancy on the Premium plan to distribute CPU load across availability zones.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **async offload (queue-based load leveling) pattern** decouples request acceptance from CPU-intensive processing. The HTTP tier becomes lightweight (sub-millisecond enqueue), preventing CPU saturation. The worker tier scales independently based on queue depth — using KEDA or Azure Functions scale triggers. This is the canonical pattern for CPU-bound background workloads behind HTTP APIs. Increasing `maxConcurrentRequests` (Option C) would worsen CPU saturation by processing more resizes simultaneously. ASE (Option A) provides dedicated compute isolation but doesn\'t solve the architectural bottleneck of synchronous CPU work in the HTTP path.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A web application uses an Azure Load Balancer (Standard SKU) to distribute HTTP traffic across 5 VMs. The team needs cookie-based session affinity so each user\'s requests consistently route to the same backend VM. The Load Balancer\'s source-IP session persistence is insufficient because clients come through corporate proxies. Which service should be used instead?

- A) Azure Traffic Manager with performance routing method.
- B) **Azure Application Gateway**, which operates at Layer 7 and inserts an `ApplicationGatewayAffinity` cookie to reliably pin each client session to the same backend VM, regardless of NAT or proxies.
- C) Azure Front Door with origin-based routing rules.
- D) Azure API Management with round-robin routing policy.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Application Gateway** is a Layer 7 load balancer supporting **cookie-based session affinity**. It inserts an `ApplicationGatewayAffinity` cookie (or a custom named cookie) in the first response; subsequent requests from the same client carry this cookie, ensuring routing to the same backend. Azure Load Balancer (Standard SKU) only supports source-IP-based session persistence (Layer 4), which breaks behind corporate NAT/proxies. Traffic Manager is DNS-based (not suited for HTTP session stickiness). Front Door uses anycast and latency-based routing, not per-client session pinning at the pool level.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A database architect plans to move 10 Azure SQL Databases to an Elastic Pool to reduce costs. Under which condition does an Elastic Pool provide the greatest cost benefit?

- A) All 10 databases have synchronized peak workloads at the same time every day.
- B) The 10 databases have **asynchronous, offsetting workload peaks** — when some databases are busy, others are idle — so the shared eDTU/vCore pool covers collective peak demand without provisioning full capacity for each database individually.
- C) Each database requires dedicated guaranteed IOPS with no resource sharing.
- D) The databases are deployed across multiple Azure regions for geo-redundancy.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Elastic Pools share a pool of eDTUs or vCores across multiple databases. The economic benefit relies on **peak diversity** — when workloads don\'t all peak simultaneously, the pool\'s shared resources cover each peak in turn without over-provisioning per database. If all databases peak at the same time (Option A), the pool provides no advantage and may perform worse than individual databases. Elastic Pools are single-region resources (Option D is invalid), and they share resources by design (Option C is incompatible).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A microservices application uses Azure Service Bus. Service A and Service B each need to receive a copy of every order message. Service C should only receive orders with `amount > 1000`. How is this configured?

- A) Create three separate queues and have the publisher send to all three individually.
- B) Create one **Service Bus topic** with three subscriptions: Service A and B subscriptions use a `TrueFilter` (receive all messages); Service C\'s subscription uses a **SQL filter** `amount > 1000` evaluated against message properties.
- C) Use a single Service Bus queue with three competing consumer groups.
- D) Use Azure Event Grid with a custom event domain and three handlers.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Service Bus **topics** implement publish-subscribe: each **subscription** receives its own independent copy of matching messages. Subscriptions support **SQL filters** (e.g., `amount > 1000`) evaluated against custom message properties, **correlation filters** (for exact-match equality), and **TrueFilter** (match all). Service A and B use `TrueFilter`; Service C uses `amount > 1000`. Queues (Option C) are competing consumers — only one consumer gets each message. Three separate queues (Option A) requires the publisher to manage fan-out logic.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Two concurrent transactions in Azure SQL Database cause a deadlock: Transaction T1 locks Table A then Table B; Transaction T2 locks Table B then Table A — each waiting for the other. What is the recommended resolution?

- A) Increase the database\'s vCore count to reduce contention and eliminate deadlocks.
- B) **Enforce a consistent lock acquisition order** across all transactions (both T1 and T2 should lock Table A before Table B), and enable **Read Committed Snapshot Isolation (RCSI)** to allow readers to use row versions instead of acquiring shared locks — reducing reader-writer conflicts.
- C) Add `WITH (NOLOCK)` hints to all queries to bypass locking entirely.
- D) Increase `LOCK_TIMEOUT` for each transaction so they wait longer before failing.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Deadlocks occur due to a **circular lock dependency**. Enforcing a **consistent resource acquisition order** (always lock Table A before Table B) eliminates the circular wait condition. **RCSI** uses row versioning so readers never block writers and writers never block readers, reducing the frequency of lock conflicts that lead to deadlocks. `NOLOCK` (Option C) reads uncommitted data, risking dirty reads and data integrity violations. Increasing `LOCK_TIMEOUT` (Option D) causes one transaction to fail faster but does not prevent deadlocks. Scaling up (Option A) does not fix logical deadlock patterns.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 14. Error Handling, Deadlock Prevention, and Encryption

<br>

## Q. An application connecting to Azure SQL Database encounters transient error code 40501 (service busy / throttling) and immediately throws an unhandled exception, failing the user request. What is the recommended pattern for handling Azure SQL transient errors?

- A) Catch all SQL exceptions and redirect users to a static maintenance page until the error stops.
- B) Implement a **retry policy with exponential backoff and jitter**: retry after 1s, 2s, 4s, 8s (with random jitter to avoid thundering herd), targeting known transient error codes, using a library such as Polly (.NET) or Resilience4j (Java). Add a **circuit breaker** to stop retrying after sustained failures.
- C) Provision a higher Azure SQL service tier (Business Critical) to eliminate throttling errors entirely.
- D) Use `READ UNCOMMITTED` transaction isolation to avoid lock-wait timeouts.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure SQL Database explicitly publishes a list of transient error codes (throttling, connection resets during failover) and recommends **retry with exponential backoff**. **Jitter** randomizes retry intervals so that hundreds of clients don\'t retry simultaneously, preventing a thundering herd that re-triggers throttling. A **circuit breaker** halts retries when the database is genuinely unavailable, preventing resource exhaustion. Upgrading tier (Option C) reduces but never eliminates transient errors (e.g., planned failovers still cause brief disconnects). `READ UNCOMMITTED` (Option D) introduces dirty reads and does not address connection errors.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A .NET application processes Azure Service Bus messages using PeekLock mode. A downstream API is intermittently unavailable, causing message processing failures. After multiple retries, failed messages should be available for manual review without blocking the queue. What is the correct design?

- A) Delete the message immediately on first failure to prevent queue buildup.
- B) Allow Service Bus to automatically move the message to the **Dead-Letter Queue (DLQ)** after `MaxDeliveryCount` is exceeded (default 10); configure an Azure Monitor alert on DLQ message count; implement a dead-letter processor function to inspect, correct, and resubmit messages.
- C) Set the message lock duration to 24 hours to prevent redelivery during downstream outages.
- D) Increase the message TTL to 30 days and let messages accumulate until the downstream API recovers.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Service Bus **automatically moves** messages to the Dead-Letter Queue (`<queuename>/$DeadLetterQueue`) once the delivery count exceeds `MaxDeliveryCount`. The DLQ preserves messages with `DeadLetterReason` and `DeadLetterErrorDescription` properties for diagnosis. **Azure Monitor alerts** on DLQ depth provide operational visibility. A dedicated DLQ processor handles resubmission after the root cause is resolved. Immediately deleting messages (Option A) causes permanent data loss. Extending lock duration (Option C) only defers the problem during a single processing attempt. Accumulating unprocessed messages indefinitely (Option D) risks hitting queue size limits.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A compliance audit requires that Azure SQL Database data at rest must be encrypted with customer-managed keys (CMK) so the company retains full control over the encryption key lifecycle, including the ability to revoke access. How is this configured?

- A) Enable Azure SQL\'s default Transparent Data Encryption (TDE) with service-managed keys — Microsoft controls the key stored in Azure\'s key store.
- B) Configure **TDE with Customer-Managed Keys (BYOK)**: store the TDE protector key in **Azure Key Vault**, grant the SQL Server\'s system-assigned **Managed Identity** `Get`, `Wrap Key`, and `Unwrap Key` permissions on the Key Vault key, and designate it as the TDE protector in SQL Server settings.
- C) Use Always Encrypted with randomized encryption on all sensitive columns as a full-database encryption solution.
- D) Enable Azure SQL Auditing and Threat Detection to satisfy the encryption compliance requirement.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**TDE with Customer-Managed Keys (BYOK)** via Azure Key Vault gives the organization full control over the encryption key: rotation, expiry scheduling, access revocation, and audit logging. Deleting or disabling the key in Key Vault immediately renders the database inaccessible — providing a cryptographic "kill switch." The SQL Server managed identity needs `Get`, `Wrap Key`, and `Unwrap Key` on the Key Vault key. Service-managed TDE (Option A) encrypts data at rest but Microsoft controls the key. Always Encrypted (Option C) provides column-level client-side encryption for specific sensitive fields — it is not a full-database at-rest encryption replacement. Auditing (Option D) is monitoring, not encryption.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A security policy requires all data at rest and in transit in Azure Cosmos DB and Azure Service Bus to be encrypted, with encryption keys rotated annually using customer-managed keys. Which combination of configurations satisfies this?

- A) **Both Cosmos DB and Service Bus encrypt data at rest by default** (AES-256, service-managed keys); configure **Customer-Managed Keys** via Azure Key Vault for both services with an annual **Key Vault automatic rotation policy**; enforce HTTPS-only for data in transit.
- B) Manually encrypt all data in application code before writing to Cosmos DB or Service Bus to add a second encryption layer.
- C) Enable VNet service endpoints on Cosmos DB and Service Bus — this encrypts data at rest automatically.
- D) Enable Azure Disk Encryption on the VMs hosting Cosmos DB to satisfy the at-rest encryption requirement.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Azure Cosmos DB and Azure Service Bus **automatically encrypt all data at rest** using AES-256 with service-managed keys. Both services support **Customer-Managed Keys (CMK)** via Azure Key Vault, granting the organization control over key lifecycle. **Azure Key Vault\'s automatic rotation policy** rotates keys on a defined schedule (e.g., annually) without manual intervention. Enforcing HTTPS (TLS 1.2+) ensures data in transit encryption. VNet service endpoints (Option C) control network access paths — they do not affect at-rest encryption. Cosmos DB is fully managed PaaS — there are no underlying VMs to apply Disk Encryption (Option D is invalid).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. Infrastructure Design: Cost, Multi-region, and HA/DR

<br>

## Q. A company runs 10 Standard D4s_v3 VMs (4 vCores, 16 GB) 24/7 and has done so stably for 6 months. Azure Advisor recommends cost optimization. The VMs average 65% CPU utilization. Which strategy provides the greatest savings with minimal operational risk?

- A) Immediately delete 5 VMs to cut the fleet size by 50%.
- B) Purchase **1-year or 3-year Reserved VM Instances** for the 10 VMs (up to 40% or 63% savings over pay-as-you-go), combined with right-sizing to Standard D2s_v3 if 65% of 4 vCores (~2.6 vCores average) can be comfortably served by 2 vCores.
- C) Switch all 10 VMs to Azure Spot Instances for up to 90% discount.
- D) Migrate the workload to Azure App Service Free tier to eliminate VM costs.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Reserved Instances** provide the best savings for predictable, always-on workloads — 40% with 1-year, up to 63% with 3-year reservations vs. pay-as-you-go. **Right-sizing** (D4s → D2s if workload fits) further reduces the reservation cost. Spot VMs (Option C) are subject to eviction with as little as 30-second notice — never appropriate for production workloads requiring consistent availability. Deleting VMs (Option A) risks degraded performance without capacity planning. App Service Free tier (Option D) allows only 60 CPU minutes/day and is incompatible with production compute requirements.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A global SaaS web application serves users in North America and Europe. The team deploys to East US (primary) and West Europe (secondary) and needs automatic health-based failover with sub-second routing decisions, WAF protection, and CDN caching at the edge. Which Azure service is best suited?

- A) Azure Traffic Manager with Priority routing — DNS-based health-checked failover for any TCP protocol.
- B) Azure Load Balancer Standard with cross-region load balancing for global HTTP traffic.
- C) **Azure Front Door** — a global Layer 7 service using anycast that provides TCP/TLS termination at the edge, health-probe-based automatic failover in under 10 seconds, integrated WAF, CDN caching, and intelligent latency-based routing.
- D) Azure API Management with multi-region gateway deployment.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Azure Front Door** is the recommended global HTTP(S) load balancing solution. It uses **anycast** with TCP/TLS termination at the nearest edge PoP (Point of Presence), delivering low latency. Its health probes detect backend failures and reroute traffic in **under 10 seconds** — far faster than DNS TTL-based Traffic Manager failover (60–300 seconds). Built-in **WAF** and **CDN caching** satisfy the additional requirements. **Traffic Manager** (Option A) is DNS-based and suitable for non-HTTP protocols or when DNS-level routing is sufficient — but TTL propagation limits failover speed. Azure Load Balancer (Option B) is regional, not global.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company\'s RTO is 4 hours and RPO is 1 hour for a critical Azure SQL Database. A regional disaster must trigger automatic failover. Which configuration meets both objectives cost-effectively?

- A) Daily automated backups with Azure SQL long-term retention (LTR) — RPO up to 24 hours.
- B) Azure SQL Database active **geo-replication** with an **auto-failover group** — continuous asynchronous replication to a secondary region with typical RPO under 5 seconds and automatic failover RTO under 30 seconds, meeting both the 1-hour RPO and 4-hour RTO targets with significant headroom.
- C) Locally redundant backup (LRS) — protects against single-datacenter failure within the same region.
- D) Zone-redundant deployment within a single region using Availability Zones.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure SQL Database **active geo-replication** maintains a continuously synchronized readable secondary in a paired region via asynchronous replication (typical RPO: 5 seconds, worst-case 5 minutes). **Auto-failover groups** enable automatic failover with a configurable grace period (minimum 1 hour) and a listener endpoint that automatically redirects connections post-failover — RTO is typically under 30 seconds. Both RPO (≤1 hour) and RTO (≤4 hours) targets are met with substantial headroom. Daily backups (Option A) have RPO up to 24 hours — exceeds the 1-hour RPO requirement. LRS backups (Option C) and Availability Zone deployment (Option D) protect against within-region failures, not full regional outages.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect designs a globally active-active write architecture using Azure Cosmos DB replicated across 4 regions. Two regions simultaneously accept writes for the same document, creating a conflict. Which Cosmos DB configuration enables multi-region writes with automatic conflict resolution?

- A) Single write region with active geo-replication (active-passive) — only one region accepts writes at a time.
- B) Enable **multi-region writes (multi-master)** in Cosmos DB with a **conflict resolution policy**: use **Last Write Wins (LWW)** with a custom timestamp property for simple conflict resolution, or a **Custom merge procedure** (stored procedure) for complex business logic-driven conflict resolution.
- C) Use session consistency with a single write region to prevent conflicts.
- D) Enable zone redundancy within a single region to handle concurrent writes.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Cosmos DB multi-region writes (multi-master)** allows every configured region to accept writes simultaneously with local write latency. When two regions concurrently update the same document, **conflict resolution** is mandatory: **Last Write Wins (LWW)** uses a designated numeric timestamp property (e.g., `_ts` or a custom field) — the write with the higher value wins. **Custom conflict resolution** uses a server-side stored procedure to implement merge logic. Single write region (Option A) eliminates write conflicts but introduces write latency for remote regions and a single write point of failure. Zone redundancy (Option D) is a within-region resilience feature unrelated to multi-master writes.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 20. AZ-900: Azure Fundamentals Certification

> *AZ-900 validates foundational cloud concepts and core Azure services. No technical prerequisites required.*

<br>

## Q. The AZ-900 exam tests understanding of the shared responsibility model. In an IaaS deployment, which of the following is the **customer\'s** responsibility?

- A) Physical datacenter security
- B) Hypervisor and host OS patching
- C) Guest OS patching and application security
- D) Network fabric and cooling

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

In IaaS, Microsoft manages the physical infrastructure (datacenter, hardware, hypervisor, host OS). The customer is responsible for the guest OS (patching, configuration), middleware, runtime, applications, and data. In PaaS, the customer responsibility shrinks further to application and data only.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A small business owner asks: "What is the primary financial benefit of moving from on-premises to the Azure cloud?" Which AZ-900 concept best describes the benefit?

- A) Replacing Capital Expenditure (CapEx) with Operational Expenditure (OpEx) — pay only for what you use.
- B) Replacing Operational Expenditure (OpEx) with Capital Expenditure (CapEx) — buy infrastructure upfront.
- C) Eliminating all IT costs permanently.
- D) Guaranteeing 100% uptime for all workloads.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: A**

Cloud computing converts the large upfront capital investment in hardware (CapEx) into a predictable, usage-based operational expense (OpEx). This improves cash flow and reduces the risk of over-provisioning or under-provisioning infrastructure.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure service provides a **guarantee** of minimum uptime (SLA) for a virtual machine deployed across **two or more Availability Zones**?

- A) 99.9% SLA — same as a single VM with Premium SSD
- B) 99.99% SLA — the highest VM SLA, requiring zone-redundant deployment
- C) 99.95% SLA — same as an Availability Set
- D) 100% SLA — Azure guarantees no downtime with Availability Zones

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Microsoft\'s SLA for a single VM with Premium SSD is 99.9%. For VMs in an Availability Set it is 99.95%. For VMs deployed across two or more Availability Zones it is 99.99%. No cloud provider guarantees 100% uptime.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to estimate the monthly cost of running an Azure Virtual Machine and Azure SQL Database before provisioning them. Which free Azure tool provides this estimate?

- A) Azure Cost Management + Billing (for existing resources)
- B) Azure Pricing Calculator (pricing.azure.com)
- C) Azure Advisor cost recommendations
- D) Azure Total Cost of Ownership (TCO) Calculator

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Azure Pricing Calculator** lets you configure hypothetical Azure services (VM size, region, OS, hours per month) and get an estimated monthly cost before provisioning anything. The **TCO Calculator** compares on-premises costs to Azure — different use case. Azure Cost Management tracks actual spend on existing resources.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. According to AZ-900 concepts, which statement about Azure Availability Zones and Azure Region Pairs is **correct**?

- A) Availability Zones protect against full region failures; Region Pairs protect against rack-level failures.
- B) Availability Zones are physically separate datacenters within one region, protecting against datacenter failures; Region Pairs are two regions hundreds of miles apart, protecting against region-wide disasters.
- C) Region Pairs and Availability Zones provide identical levels of redundancy.
- D) Availability Zones are only available in the US; Region Pairs are a global feature.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Availability Zones = intra-region redundancy (datacenter-level protection). Region Pairs = inter-region redundancy (geographic disaster protection, e.g., East US ↔ West US). Azure prioritizes recovering one region in a pair first during a platform-wide outage, and ensures updates are not rolled out to both regions simultaneously.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company uses on-premises servers and wants to keep sensitive data on-premises while using Azure for burst workloads during peak seasons. Which cloud deployment model is described?

- A) Public cloud
- B) Private cloud
- C) Hybrid cloud
- D) Community cloud

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

A **hybrid cloud** combines on-premises (or private cloud) infrastructure with a public cloud, allowing data and applications to be shared between them. This model is common for organizations that must keep sensitive data on-premises for regulatory reasons while leveraging cloud elasticity for variable workloads.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which of the following is a benefit of the **consumption-based pricing model** in Azure?

- A) You pay a fixed monthly fee regardless of usage.
- B) You pay only for the resources you use, with no upfront cost.
- C) You must commit to a minimum spend for 12 months.
- D) You purchase a fixed block of compute hours in advance.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The consumption-based (pay-as-you-go) model is one of the fundamental cloud economics principles in AZ-900. There are no upfront infrastructure costs; you pay only for what you consume. This replaces the traditional CapEx model and allows organizations to scale costs in line with actual usage.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An organization wants to deploy a fully managed relational database without managing the underlying server OS, patches, or hardware. Which Azure service best fits this requirement?

- A) SQL Server on Azure Virtual Machine (IaaS)
- B) Azure SQL Database (PaaS)
- C) Azure SQL Managed Instance with manual OS patching
- D) Azure Cosmos DB with SQL API

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure SQL Database** is a fully managed PaaS relational database. Microsoft handles OS patching, hardware, backups, and high availability. SQL Server on Azure VM (IaaS) requires the customer to manage the OS and SQL Server installation. Cosmos DB is a NoSQL service.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure service allows you to create a **private, isolated network** in Azure and connect Azure resources to each other, to the internet, or to on-premises networks?

- A) Azure DNS
- B) Azure Virtual Network (VNet)
- C) Azure Content Delivery Network (CDN)
- D) Azure Traffic Manager

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Virtual Network (VNet)** is the fundamental networking building block in Azure. It enables Azure resources (VMs, App Services, etc.) to securely communicate with each other, the internet, and on-premises networks. Azure DNS resolves domain names; CDN caches content at edge nodes; Traffic Manager is a DNS-based load balancer for global routing.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure storage redundancy option replicates data **synchronously across three Availability Zones** within the same region?

- A) Locally Redundant Storage (LRS)
- B) Geo-Redundant Storage (GRS)
- C) Zone-Redundant Storage (ZRS)
- D) Geo-Zone-Redundant Storage (GZRS)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Zone-Redundant Storage (ZRS)** replicates data synchronously across three Availability Zones in a single region, protecting against datacenter-level failures. LRS replicates three times within a single datacenter. GRS replicates to a secondary region. GZRS combines ZRS within the primary region and asynchronous replication to a secondary region.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to run small event-driven code snippets that execute in response to HTTP requests, timers, or queue messages — without provisioning or managing any servers. Which Azure service should they use?

- A) Azure Virtual Machines
- B) Azure Kubernetes Service (AKS)
- C) Azure Functions
- D) Azure App Service (Basic tier)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Azure Functions** is a serverless compute service that runs event-triggered code without requiring infrastructure management. You pay only for execution time (consumption plan). AKS and VMs require managing infrastructure. App Service is PaaS but still requires a hosting plan configuration and is better suited for full web apps.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to ensure all new Azure resources created in their subscription have a `CostCenter` tag applied automatically. Which Azure service enforces this requirement?

- A) Azure Blueprints
- B) Azure Policy
- C) Azure Advisor
- D) Microsoft Defender for Cloud

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Policy** allows you to define rules (policies) that are evaluated against Azure resource properties at creation and during updates. A policy with the `append` or `deny` effect can enforce that all resources carry required tags like `CostCenter`. Azure Blueprints packages policies but the underlying enforcement is done by Azure Policy. Azure Advisor makes recommendations; Defender for Cloud focuses on security posture.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Microsoft portal provides information about Azure service compliance certifications, privacy documents, and regulatory standards such as ISO 27001, SOC 2, and GDPR?

- A) Azure Security Center (Defender for Cloud)
- B) Microsoft Trust Center (trustcenter.microsoft.com)
- C) Azure Policy Compliance dashboard
- D) Azure Advisor

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Microsoft Trust Center** is the single source of truth for Microsoft\'s approach to security, privacy, compliance, and transparency. It hosts audit reports, compliance certifications (ISO 27001, SOC 2, FedRAMP, GDPR documentation, etc.) and is a key resource referenced in the AZ-900 governance and compliance domain.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure feature enables you to run **Windows and Linux containers** without managing the underlying virtual machine infrastructure, and bills per container second?

- A) Azure Virtual Machine Scale Sets
- B) Azure Container Instances (ACI)
- C) Azure Kubernetes Service (AKS) with node pools
- D) Azure App Service (container deployment)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Container Instances (ACI)** is the fastest and simplest way to run a container in Azure without managing VMs or orchestration. Billing is per vCPU-second and GB-second. AKS is a managed Kubernetes service for orchestrating multiple containers at scale; App Service can host containers but requires a plan. ACI is commonly tested in AZ-900 as the "serverless containers" option.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An organization wants to connect their on-premises network to Azure over a **dedicated private connection** (not over the public internet) to achieve consistent bandwidth and lower latency. Which Azure service provides this?

- A) Azure VPN Gateway (Site-to-Site VPN)
- B) Azure ExpressRoute
- C) Azure Virtual WAN
- D) Azure Peering Service

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure ExpressRoute** provides a dedicated, private connection between on-premises infrastructure and Azure datacenters through a connectivity provider — traffic does not traverse the public internet. This delivers higher reliability, faster speeds, lower latency, and higher security than internet-based VPN connections. Site-to-Site VPN is encrypted but travels over the public internet.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. In Microsoft Entra ID (formerly Azure Active Directory), which feature requires users to provide **two or more verification methods** — such as a password plus a phone notification — before they can sign in?

- A) Single Sign-On (SSO)
- B) Conditional Access
- C) Multi-Factor Authentication (MFA)
- D) Azure AD B2C

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Multi-Factor Authentication (MFA)** adds a second verification layer beyond a password, such as the Microsoft Authenticator app, SMS code, or hardware token. It is a core identity security feature in Microsoft Entra ID. SSO lets users sign in once to access multiple apps; Conditional Access enforces policies based on signals (location, device, risk); B2C is for customer-facing identity.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which of the following correctly describes the **Azure management hierarchy** from the broadest to the most granular scope?

- A) Subscription → Management Group → Resource Group → Resource
- B) Management Group → Subscription → Resource Group → Resource
- C) Resource Group → Subscription → Management Group → Resource
- D) Management Group → Resource Group → Subscription → Resource

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The Azure management hierarchy (top to bottom) is:
1. **Management Groups** (can contain multiple subscriptions; up to 6 levels deep)
2. **Subscriptions** (billing and access boundary)
3. **Resource Groups** (logical containers for related resources)
4. **Resources** (individual Azure services like VMs, storage accounts)

Policies and RBAC assignments cascade downward through this hierarchy.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure tool provides **personalized recommendations** to help reduce costs, improve performance, enhance security, and achieve high availability for your existing Azure resources?

- A) Azure Pricing Calculator
- B) Azure Monitor
- C) Azure Advisor
- D) Azure Service Health

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Azure Advisor** analyzes your deployed resources and gives actionable recommendations across five categories: Cost, Security, Reliability, Operational Excellence, and Performance. For example, it may flag idle VMs, suggest Reserved Instances, or highlight resources without MFA. The Pricing Calculator estimates costs *before* deployment; Monitor collects telemetry; Service Health reports on platform incidents.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company\'s legal team needs to verify that Azure datacenters storing their EU customer data comply with GDPR. Which resource should they consult first?

- A) Azure Security Center compliance dashboard
- B) Microsoft Service Trust Portal (servicetrust.microsoft.com)
- C) Azure Policy regulatory compliance built-in initiatives
- D) Azure Defender for Cloud regulatory compliance blade

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Microsoft Service Trust Portal** hosts audit reports, compliance guides, and data protection resources — including GDPR documentation, ISO/IEC certificates, and SOC reports — that organizations can download to satisfy their own compliance obligations. Azure Policy compliance dashboards show *your* resource compliance against a policy, not Microsoft\'s platform certifications.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure service provides a **unified security management system** that strengthens the security posture of your cloud resources and provides threat protection across hybrid workloads?

- A) Azure Key Vault
- B) Microsoft Defender for Cloud (formerly Azure Security Center)
- C) Azure Firewall
- D) Microsoft Sentinel

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Microsoft Defender for Cloud** (formerly Azure Security Center + Azure Defender) continuously assesses security configurations, provides a Secure Score, and delivers threat protection for Azure, on-premises, and multicloud resources. Azure Key Vault stores secrets/keys; Azure Firewall is a network-level firewall; Microsoft Sentinel is a SIEM/SOAR solution for advanced threat hunting and response.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which Azure Blob storage access tier is optimized for data that is **accessed rarely** (less than once a month) and can tolerate a retrieval latency of hours, offering the lowest storage cost?

- A) Hot tier
- B) Cool tier
- C) Cold tier
- D) Archive tier

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: D**

The **Archive tier** has the lowest storage cost but the highest retrieval cost and latency (hours to rehydrate). It is designed for data that is rarely or never accessed — such as long-term backups, raw media archives, or compliance data. Hot is for frequently accessed data; Cool is for infrequently accessed data (30-day minimum); Cold tier (introduced 2023) is for data accessed less than once a month with a 90-day minimum.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A retail company wants to estimate how much money they would **save by migrating their on-premises servers to Azure**, comparing total cost of ownership including hardware, power, cooling, and labor. Which tool should they use?

- A) Azure Pricing Calculator
- B) Azure Cost Management + Billing
- C) Azure TCO (Total Cost of Ownership) Calculator
- D) Azure Advisor cost recommendations

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

The **Azure TCO Calculator** is specifically designed for this scenario — it takes your on-premises workload inventory (servers, storage, networking) and calculates a 5-year cost comparison between keeping on-premises vs migrating to Azure, factoring in hardware refresh, power, cooling, datacenter space, and IT labor. The Pricing Calculator estimates Azure-only costs; Cost Management tracks current Azure spend; Advisor gives optimization tips for existing resources.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which of the following Azure services is classified as **Software as a Service (SaaS)**?

- A) Azure Virtual Machines
- B) Azure App Service
- C) Microsoft 365 (Office 365)
- D) Azure SQL Database

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Microsoft 365** (formerly Office 365) — including Word, Excel, Teams, and Outlook delivered via the cloud — is a SaaS offering. The end user consumes a fully managed, ready-to-use application; they manage nothing. Azure VMs = IaaS; Azure App Service = PaaS; Azure SQL Database = PaaS. Knowing the SaaS/PaaS/IaaS classification of common services is a frequent AZ-900 exam topic.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Under the **shared responsibility model** in a **PaaS** deployment, which layer is the **customer\'s** responsibility? (Select the most accurate answer.)

- A) Physical hosts, network, and datacenter
- B) Operating system and middleware
- C) Application code and data
- D) Hypervisor and hardware maintenance

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

In PaaS, Microsoft manages the physical infrastructure, network controls, host OS, and middleware/runtime. The **customer** is responsible for their **applications and data** — including application security, input validation, and data protection. In IaaS the customer also owns the guest OS and middleware. In SaaS the customer is only responsible for accounts, access, and data.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which of the following statements about **Azure Resource Groups** is correct?

- A) A resource can exist in multiple resource groups simultaneously.
- B) Resources in a resource group must all be in the same Azure region.
- C) Deleting a resource group deletes all the resources contained within it.
- D) Resource groups can be nested inside other resource groups.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Deleting a resource group is a **permanent, cascading delete** — all resources within it are removed. Key facts: a resource belongs to exactly one resource group (cannot be in multiple); resources in a group can span different regions; resource groups cannot be nested. Resource groups are a logical container for lifecycle and billing management, not a geographic or hierarchical boundary.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company wants to allow users from a partner organization to access their Azure resources using the partner\'s existing **corporate identities** (from the partner\'s Entra ID tenant) without creating new accounts. Which Microsoft Entra ID feature enables this?

- A) Azure AD B2C (Business-to-Consumer)
- B) Azure AD B2B (Business-to-Business) Collaboration
- C) Microsoft Entra External Identities — Guest invitations
- D) Both B and C — B2B collaboration and Guest invitations are the same feature

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: D**

**Azure AD B2B collaboration** (now part of **Microsoft Entra External Identities**) lets you invite external users as **guests** into your tenant. They authenticate using their own organization\'s identity provider (Entra ID, Google, etc.) and receive limited access to your resources via RBAC. B2C is for consumer-facing apps where users create personal accounts — a different scenario entirely.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 21. AZ-104: Azure Administrator Certification

> *AZ-104 (which replaced the retired AZ-102) validates skills in implementing, managing, and monitoring Azure environments for enterprise organizations.*

<br>

## Q. An Azure Administrator needs to move a virtual machine from resource group **rg-dev** to resource group **rg-prod** within the same subscription. What happens to the VM\'s resource ID after the move?

- A) The resource ID remains unchanged.
- B) The resource ID changes to reflect the new resource group name.
- C) The move is not supported — VMs cannot be moved between resource groups.
- D) The VM is deleted and must be recreated in the new resource group.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

When you move a resource between resource groups (or subscriptions), its resource ID changes because the resource group name is part of the ID path (`/subscriptions/.../resourceGroups/rg-prod/...`). The VM\'s public/private IPs, disks, and configuration are preserved, but any hardcoded references to the old resource ID (in scripts, RBAC assignments, policy scopes) must be updated.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Administrator needs to allow a specific external vendor\'s application to access an Azure Storage account using its own identity — not a user account. No passwords should be stored. Which identity type in Microsoft Entra ID is appropriate?

- A) User account with a guest invitation (Azure AD B2B)
- B) Service Principal with certificate-based authentication or Workload Identity Federation
- C) Managed Identity assigned to the vendor\'s application
- D) Shared Access Signature stored in the vendor\'s app config

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A **Service Principal** is an application identity in Entra ID. For external vendor apps running outside Azure, a service principal with **certificate authentication** (avoiding client secrets) or **Workload Identity Federation** (OIDC) is the secure pattern. Managed Identities are only for Azure-hosted workloads. SAS tokens grant storage access but are not identity-based.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Administrator runs the following command and receives an error:

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

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure subscriptions have per-region vCPU quotas. When the quota is exhausted, new VMs cannot be deployed in that region for that VM family. Administrators request quota increases through the portal or support ticket. Alternatively, deploying in a different region (with available quota) is an immediate workaround.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Administrator needs to ensure that all Azure VMs in the production subscription have the Azure Monitor Agent installed and are sending logs to a specific Log Analytics workspace. New VMs provisioned in the future must also comply automatically. Which approach achieves this at scale?

- A) Manually install the agent on each VM after provisioning.
- B) Assign an Azure Policy Initiative (e.g., "Enable Azure Monitor for VMs") with `deployIfNotExists` effect, scoped to the production subscription. Create a remediation task for existing non-compliant VMs.
- C) Create an Azure Automation runbook that checks for the agent weekly.
- D) Use an ARM template to deploy the agent alongside each VM.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Policy with `deployIfNotExists` automatically installs the Azure Monitor Agent on VMs that don\'t have it, triggered at deployment time (for new VMs) and via remediation tasks (for existing VMs). This ensures continuous compliance without manual intervention. The built-in "Enable Azure Monitor for VMs" initiative includes the relevant policy definitions.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Administrator configures a Network Security Group with the following inbound rules:

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

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

NSG rules are evaluated in priority order (lowest number first). The source 10.0.2.0/24 does not match rule 100 (which only allows 10.0.1.0/24). Rule 200 matches (any source, port 443, Deny) and blocks the connection. Rule 300 is never reached because rule 200 already matched. NSGs do not have any built-in "allow HTTPS" default — all unlisted traffic follows the default deny-all rule.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to delegate the ability to manage virtual machines in a single resource group (`rg-compute`) to a junior admin, but the junior admin must **not** be able to modify networking or access control settings. Which built-in RBAC role should be assigned at the resource group scope?

- A) Contributor on the subscription
- B) Virtual Machine Contributor on `rg-compute`
- C) Owner on `rg-compute`
- D) Reader on the subscription + Contributor on `rg-compute`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Virtual Machine Contributor** built-in role grants permission to create and manage VMs, but does **not** allow managing the virtual network, storage account, or assigning roles (no `Microsoft.Authorization/*/write`). Assigning it at the resource group scope limits blast radius to that group only. Contributor would also allow modifying networking resources within the group; Owner grants full access including RBAC management.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator creates a new Azure Virtual Network with address space `10.0.0.0/16`. They then create a subnet `10.0.1.0/24`. How many **usable host IP addresses** are available in that subnet?

- A) 256
- B) 254
- C) 251
- D) 248

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Azure **reserves 5 IP addresses** in every subnet: the network address (.0), the default gateway (.1), two Azure DNS addresses (.2, .3), and the broadcast address (.255). A /24 subnet has 256 total addresses; 256 − 5 = **251 usable host addresses**. This is a frequently tested AZ-104 networking fact.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator wants to allow traffic between two Azure Virtual Networks (`vnet-east` and `vnet-west`) in the **same region** without routing traffic over the public internet. Which feature accomplishes this?

- A) Azure VPN Gateway with Site-to-Site connection
- B) VNet Peering
- C) Azure ExpressRoute
- D) Azure Virtual WAN hub

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**VNet Peering** creates a direct, low-latency, high-bandwidth connection between two VNets using Microsoft\'s backbone — traffic never traverses the public internet. For VNets in the same region it is called **local VNet peering**; across regions it is **global VNet peering**. VPN Gateway is used for encrypted tunnels to on-premises or between VNets when a gateway is required; ExpressRoute connects on-premises to Azure over a dedicated circuit.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to ensure that an Azure VM is **always reachable via the same DNS name** even if the VM is deallocated and reallocated (which changes its dynamic public IP). What is the simplest solution?

- A) Assign a static private IP address to the VM\'s NIC.
- B) Assign a static public IP address (Standard SKU) and associate it with the VM\'s NIC.
- C) Use a DNS A record pointing to the VM\'s current dynamic IP and update it via Azure Automation.
- D) Place the VM behind an Azure Load Balancer.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A **static public IP address** retains its address across VM deallocations and restarts. The administrator can then create a DNS record pointing to this static IP, ensuring consistent reachability. Dynamic IPs change when the VM is deallocated. A static private IP only helps with internal VNet connectivity, not external DNS. Standard SKU static IPs also support zone redundancy.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator runs the following Azure CLI command to create a storage account but receives an error stating the name is unavailable:

```bash
az storage account create \
  --name mystorageaccount \
  --resource-group rg-prod \
  --location eastus \
  --sku Standard_LRS
```

What is the most likely reason for the error?

- A) The `--sku` parameter value is invalid.
- B) Azure storage account names must be globally unique across all Azure customers (3–24 lowercase alphanumeric characters).
- C) The resource group `rg-prod` does not exist yet.
- D) The `eastus` region does not support `Standard_LRS`.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Storage account names form part of the public DNS endpoint (`mystorageaccount.blob.core.windows.net`), so they must be **globally unique** across all Azure tenants. Names must be 3–24 characters, lowercase letters and numbers only. The name `mystorageaccount` is almost certainly already taken by another customer. `Standard_LRS` is valid in all regions; if the resource group didn\'t exist the error message would say so explicitly.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to configure **Azure Bastion** to securely access VMs without exposing RDP (port 3389) or SSH (port 22) to the internet. Which subnet name and minimum size are required in the VNet?

- A) Any subnet name; /28 minimum
- B) `AzureBastionSubnet`; /26 minimum
- C) `BastionSubnet`; /27 minimum
- D) `GatewaySubnet`; /27 minimum

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Bastion requires a dedicated subnet named exactly **`AzureBastionSubnet`** (case-sensitive) with a minimum size of **/26** (64 addresses). A smaller subnet will cause deployment failure. `GatewaySubnet` is reserved for VPN/ExpressRoute gateways. Bastion provides browser-based RDP/SSH over TLS port 443, so no public IP or open RDP/SSH ports are needed on the VMs themselves.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company stores critical files in Azure Blob Storage. They need to prevent blobs from being **deleted or modified** for a mandatory 7-year retention period to meet regulatory compliance. Which feature should the administrator configure?

- A) Soft delete with a 7-year retention window
- B) Immutable blob storage with a time-based retention policy (WORM)
- C) Azure Backup with a 7-year vault policy
- D) Object replication to a secondary storage account

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Immutable blob storage** (Write Once, Read Many — WORM) enforces a time-based retention policy that prevents any modification or deletion of blobs during the retention interval. This is specifically designed for regulatory compliance (SEC 17a-4, CFTC, FINRA, etc.). Soft delete allows recovery after accidental deletion but can still be disabled by an admin; Azure Backup is for VM/DB workloads; object replication copies data but does not prevent deletion of the source.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator deploys an **Azure Application Gateway** in front of three web server VMs. After deployment, one backend VM becomes unhealthy. What does Application Gateway do?

- A) Stops routing all traffic to all backend VMs until the issue is resolved.
- B) Automatically removes the unhealthy VM from the backend pool and routes traffic only to healthy VMs; the unhealthy VM is re-added once health probes succeed.
- C) Terminates the unhealthy VM and replaces it with a new one.
- D) Sends an alert but continues routing traffic to all VMs including the unhealthy one.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Application Gateway continuously runs **health probes** against backend pool members. If a VM fails the probe, it is automatically taken out of rotation — no traffic is sent to it. Once the VM passes probes again, it is automatically reintroduced. This provides automatic high availability without manual intervention. Application Gateway does not manage VM lifecycle (that would be VMSS or Azure Automation).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Administrator needs to implement **just-in-time (JIT) VM access** to reduce the attack surface on management ports (RDP/SSH). Which service provides this capability?

- A) Azure Firewall with DNAT rules
- B) Microsoft Defender for Cloud — JIT VM Access
- C) Network Security Group with time-based rules
- D) Azure AD Conditional Access with device compliance

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Microsoft Defender for Cloud\'s Just-in-Time VM Access** locks down inbound traffic to management ports by default, then opens them temporarily only when an authorized user requests access (for a configurable time window and source IP). After the window expires, the NSG rule is automatically removed. NSGs don\'t natively support time-based rules; Conditional Access controls authentication, not network ports.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to automatically **scale out** the number of VM instances in a VM Scale Set (VMSS) when average CPU utilization exceeds 70% for 5 minutes, and scale in when it drops below 30%. Which Azure feature handles this?

- A) Azure Load Balancer health probes
- B) Azure Monitor Autoscale
- C) Azure Advisor right-sizing recommendations
- D) Azure Automation runbook triggered by alerts

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Monitor Autoscale** lets you define scale-out and scale-in rules based on metrics (CPU, memory, custom metrics) or schedules. Rules include a metric threshold, aggregation period, and cooldown period to prevent flapping. It is natively integrated with VM Scale Sets, App Service plans, and other scalable resources. Azure Advisor recommends right-sizing but does not perform automated scaling.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure Administrator must configure **Azure DNS** to resolve the custom domain `app.contoso.com` to an Azure Public IP address (`20.1.2.3`). Which DNS record type should they create?

- A) CNAME record pointing `app.contoso.com` → `20.1.2.3`
- B) A record pointing `app.contoso.com` → `20.1.2.3`
- C) MX record pointing `app.contoso.com` → `20.1.2.3`
- D) NS record pointing `app.contoso.com` → `20.1.2.3`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

An **A (Address) record** maps a hostname directly to an IPv4 address. CNAME records map a hostname to another hostname (canonical name) — you cannot use a CNAME for a zone apex (`@`). MX records define mail exchange servers. NS records define authoritative name servers for a zone. For direct IP mapping, always use an A record.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator creates an Azure **Recovery Services Vault** and configures backup for 10 Azure VMs. After 30 days, they need to restore a specific VM to a **point-in-time from 25 days ago**. What is required for this restore to be possible?

- A) The VM must not have been modified since the backup was taken.
- B) A recovery point (backup snapshot) from 25 days ago must exist in the vault within the configured retention period.
- C) The original VM must still exist and be running.
- D) Restores older than 7 days require a support ticket.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Backup stores **recovery points** according to the backup policy\'s retention schedule (daily, weekly, monthly, yearly). As long as a recovery point from 25 days ago exists and falls within the configured retention window, the restore is possible — regardless of the current state of the VM. The original VM does not need to exist; you can restore to a new VM or replace the existing one. No support ticket is required for self-service restore.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to copy **terabytes of on-premises data** to Azure Blob Storage. The internet connection is 10 Mbps and uploading online would take weeks. Which Azure service provides an **offline data transfer** solution using physical devices?

- A) Azure Data Factory (self-hosted integration runtime)
- B) Azure Data Box
- C) AzCopy with parallel uploads
- D) Azure File Sync

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Data Box** is Microsoft\'s family of offline data transfer appliances (Data Box Disk, Data Box, Data Box Heavy) for migrating large datasets when network transfer would be impractically slow or expensive. Microsoft ships a physical device; you copy data to it on-premises; ship it back to Microsoft; they upload it to your storage account. AzCopy and Data Factory use the network; Azure File Sync is for ongoing hybrid file sync, not bulk migration.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An organization\'s security policy requires that **all Azure administrator accounts** must use MFA and must only be able to sign in from corporate managed devices. Which Microsoft Entra ID feature enforces both conditions before granting access?

- A) Microsoft Entra MFA (per-user MFA)
- B) Conditional Access policies
- C) Azure AD Password Protection
- D) Privileged Identity Management (PIM)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Conditional Access** evaluates signals (user identity, device compliance, location, risk level, application) and enforces access controls (require MFA, block access, require compliant device) as a single policy. A single policy can simultaneously require MFA **and** a compliant/hybrid-joined device. Per-user MFA enforces MFA but cannot check device compliance. PIM governs privileged role activation, not the sign-in session conditions. Password Protection prevents weak passwords.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator uses the Azure Portal to create a **Diagnostic Setting** on an Azure SQL Database to send audit logs and metrics to a **Log Analytics workspace**. After 48 hours, they query the workspace but see no data. What is the most likely cause?

- A) Log Analytics workspaces only store data for 24 hours by default.
- B) The Diagnostic Setting was saved but the correct **log categories** (e.g., `SQLSecurityAuditEvents`, `Basic` metrics) were not selected before saving.
- C) Azure SQL Database does not support sending logs to Log Analytics.
- D) The workspace must be in the same region as the SQL Database.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

When creating a Diagnostic Setting, you must explicitly **check the log and metric categories** you want to export. If none are selected, the setting is saved but no data flows. This is a common misconfiguration. Log Analytics workspaces retain data for 30 days by default (configurable up to 2 years). SQL Database supports Log Analytics as a destination. The workspace can be in a different region than the source resource.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to deploy the same set of Azure resources (VNet, NSG, VMs, storage) consistently across 5 subscriptions, including RBAC assignments and Azure Policy assignments as a single atomic operation. Which Azure service is most appropriate?

- A) Azure Resource Manager (ARM) templates deployed per subscription
- B) Azure Blueprints
- C) Terraform with a multi-subscription backend
- D) Azure Policy Initiative assigned to a Management Group

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Blueprints** packages ARM templates, RBAC role assignments, policy assignments, and resource groups into a single reusable blueprint definition. It can be assigned to multiple subscriptions and tracks the relationship between the blueprint and deployed resources (unlike standalone ARM templates, which lose that link after deployment). A single blueprint assignment deploys all components atomically. Terraform can do this but is a third-party tool, not a native Azure service.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator configures **Azure File Sync** to sync an on-premises Windows Server file share to an Azure file share. After enabling **cloud tiering**, some files on the server appear as offline placeholders. What does this mean?

- A) The files are corrupted and need to be re-synced.
- B) Files that haven\'t been accessed recently have been tiered to Azure; the placeholder preserves the file\'s namespace locally, and the full content is recalled from Azure on first access.
- C) Cloud tiering has failed — all files should be fully present on the server.
- D) The files have been permanently deleted from the on-premises server to save space.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Cloud tiering** is an optional Azure File Sync feature that frees up local disk space by replacing infrequently accessed files with **reparse point (placeholder) files**. The namespace (file name, size, metadata) is visible locally, but the file content lives in Azure. When a user or application opens the file, Azure File Sync transparently recalls the data from Azure. Files are not deleted — they are fully recoverable.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure VM running Windows Server is experiencing high memory pressure. The administrator checks Azure Monitor and sees that the `Percentage CPU` metric is consistently below 20%, but the VM is slow. Which additional metric should the administrator examine?

- A) `Disk Read Bytes`
- B) `Available Memory Bytes` (or `\Memory\Available Bytes` via the Diagnostics extension)
- C) `Network In Total`
- D) `OS Disk Queue Depth`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Since CPU is low, the bottleneck is likely memory. **`Available Memory Bytes`** (collected via the Azure Monitor Agent / Diagnostics extension) shows how much RAM is free. Low available memory causes excessive paging to disk (thrashing), which explains the slowness despite low CPU. The standard VM host metrics don\'t include memory by default — the Azure Monitor Agent or Diagnostics extension must be configured to collect guest OS memory metrics.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator assigns the built-in **Reader** role to a user at the **subscription** scope. The same user is then assigned the **Contributor** role on a specific resource group `rg-dev`. What is the user\'s effective access to resources inside `rg-dev`?

- A) Reader only — the subscription-level role takes precedence.
- B) Contributor — the most permissive role at the narrowest scope applies.
- C) No access — conflicting role assignments cancel each other out.
- D) The effective permission is the union of both roles: Reader (subscription-wide) + Contributor (on `rg-dev`), resulting in Contributor permissions inside `rg-dev`.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: D**

Azure RBAC is **additive** — permissions accumulate across all role assignments. The user has Reader across the whole subscription **plus** Contributor on `rg-dev`. Inside `rg-dev`, the effective permission is the union: Contributor (which is broader). RBAC never cancels assignments; it only denies through explicit **Deny** assignments (which override all Allow). There is no "narrowest scope wins" rule — all assignments are combined.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to **lock** a production resource group to prevent accidental deletion of any resources inside it, while still allowing authorized users to read and modify resource configurations. Which Azure lock type should they apply?

- A) `ReadOnly` lock on the resource group
- B) `CanNotDelete` lock on the resource group
- C) `CanNotDelete` lock on each individual resource
- D) Azure Policy with a `deny` effect on delete operations

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A **`CanNotDelete`** lock (also called Delete lock) prevents deletion of the resource group and all resources within it, while still allowing reads and modifications (create/update). A `ReadOnly` lock would block write operations, preventing administrators from making configuration changes. Applying the lock at the resource group level automatically protects all child resources — there\'s no need to lock each resource individually.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An administrator needs to allow an Azure VM to **read secrets from Azure Key Vault** without storing any credentials in the VM\'s application code or configuration files. What is the recommended approach?

- A) Store the Key Vault access key in an environment variable on the VM.
- B) Enable a **System-assigned Managed Identity** on the VM, then grant it the Key Vault Secrets User role (or Key Vault access policy) on the Key Vault.
- C) Create a service principal with a client secret, store the secret in the VM\'s `appsettings.json`.
- D) Use a SAS token to authenticate to Key Vault.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Managed Identities** eliminate the need for credentials entirely. A system-assigned managed identity is automatically created and managed by Azure and tied to the VM\'s lifecycle. The VM obtains a token from the Azure Instance Metadata Service (IMDS) at runtime to authenticate to Key Vault — no secrets stored anywhere. Storing credentials in config files or environment variables violates the principle of least privilege and creates a secret management burden.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 22. AZ-204: Azure Developer Certification

> *AZ-204 validates skills in designing, building, testing, and maintaining cloud applications and services on Azure.*

<br>

## Q. A developer is building an Azure Function that must process each message from an Azure Service Bus queue **exactly once**, even if the function fails mid-processing and the message is retried. Which Service Bus feature ensures exactly-once processing?

- A) Set `maxDeliveryCount` to 1 so messages are never retried.
- B) Use Service Bus sessions with a session-aware receiver to enforce FIFO ordering per session.
- C) Enable duplicate detection on the queue with a detection window that covers the maximum processing time, and make the function idempotent.
- D) Use PeekLock mode combined with message deduplication and an idempotent processing logic that checks a processed-message store (e.g., Azure Table Storage) before acting.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: D**

Service Bus PeekLock holds the message invisible to other consumers until explicitly completed or abandoned. However, if the function crashes after processing but before completing the lock, the message reappears. True exactly-once semantics require idempotent processing: check a durable store (Table Storage, Redis, SQL) whether the message ID was already processed before acting, then complete the lock. Duplicate detection prevents the *same message from being enqueued twice*, not from being processed twice after delivery.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer implements the following code pattern in a .NET Azure Function:

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

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

`DefaultAzureCredential` tries multiple credential sources in order. In App Service/Functions with a Managed Identity, it uses `ManagedIdentityCredential`, which fetches a token from the IMDS endpoint (`169.254.169.254`) using the assigned managed identity — no client secrets, certificates, or stored credentials required. The Azure CLI fallback is used only in local development environments.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer registers an application in Microsoft Entra ID to call the Microsoft Graph API and read users' profiles. The app runs as a background service (no user interaction). Which OAuth 2.0 flow should the developer use?

- A) Authorization Code Flow with PKCE
- B) Client Credentials Flow
- C) Device Authorization Flow
- D) On-Behalf-Of (OBO) Flow

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Client Credentials Flow** is for daemon/service applications that run without a signed-in user. The app authenticates with its own identity (client ID + secret or certificate) and receives an access token scoped to application permissions. Authorization Code Flow requires user interaction. OBO is used when a service calls another service on behalf of a signed-in user.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to cache the results of expensive database queries in Azure Cache for Redis. The cache should automatically expire entries after 10 minutes and handle cache misses gracefully. Which pattern should be implemented?

- A) Write-through cache: write to Redis and the database simultaneously on every request.
- B) Cache-Aside (Lazy Loading): on a cache miss, read from the database, write the result to Redis with a 10-minute TTL, and return the result.
- C) Read-through cache: configure Redis to query the database automatically on a miss.
- D) Write-behind cache: write to Redis first and asynchronously sync to the database.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Cache-Aside** (also called Lazy Loading) is the recommended pattern with Azure Cache for Redis for read-heavy workloads. The application checks the cache first; on a miss, it fetches from the database, stores the result in Redis with a TTL (`SETEX key 600 value`), and returns the data. Azure Cache for Redis does not natively support read-through or write-behind without additional infrastructure.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer is publishing messages to Azure Event Hubs from an IoT application. The consumer application must process messages from the **same device** (same partition key) in strict order. What should the developer configure?

- A) Use multiple consumer groups, one per device.
- B) Set the `PartitionKey` on each event to the device ID when publishing; consumers reading from the same partition receive events in order.
- C) Use Event Hub Capture to write to Azure Storage and process files in order.
- D) Enable geo-replication on the Event Hub namespace.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Event Hubs guarantees message ordering **within a partition**. By setting `PartitionKey = deviceId`, all messages from the same device are routed to the same partition and delivered in arrival order to consumers. Different devices can be on different partitions, enabling parallel processing. Consumer groups provide independent read positions for different consumer applications, not ordering guarantees.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to deploy an Azure Function with a trigger that **automatically scales to zero** when idle and scales out to handle thousands of concurrent invocations with no infrastructure management. Which hosting plan should they choose?

- A) Dedicated (App Service) Plan
- B) Premium Plan (EP1)
- C) Consumption Plan
- D) Kubernetes-based KEDA plan

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

The **Consumption Plan** is the fully serverless Azure Functions hosting option. It scales out automatically based on events, scales to zero when idle, and you pay only per execution (per-second billing + per-million executions). The Premium Plan provides always-warm instances and VNet integration but has a minimum charge. The Dedicated Plan runs on reserved App Service Plan VMs — no scale-to-zero.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer is building an API with Azure API Management (APIM). They need to **rate-limit** callers to 100 requests per minute per subscription key and return an HTTP 429 when the limit is exceeded. Which APIM policy should they apply?

- A) `<quota>` policy
- B) `<rate-limit>` policy
- C) `<rate-limit-by-key>` policy
- D) `<throttle>` policy

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

`<rate-limit-by-key>` enforces a call rate limit per a custom key (e.g., `context.Subscription.Id` or client IP) over a fixed time window, returning HTTP 429 when exceeded. `<rate-limit>` limits by APIM subscription but is less flexible. `<quota>` enforces a total call count over a longer period (days/weeks), not a per-minute sliding window. There is no built-in `<throttle>` policy name in APIM.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to store application configuration values (feature flags, connection strings) centrally so that **multiple App Service instances and Azure Functions** can share the same configuration without redeployment when values change. Which Azure service is designed for this?

- A) Azure Key Vault
- B) Azure App Configuration
- C) Application Settings blade in each App Service
- D) Azure Storage Table

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure App Configuration** is a managed service for centralizing application settings and feature flags. Multiple applications (App Service, Functions, containers, on-premises) can reference the same configuration store. Changes are immediately available without redeployment. Azure Key Vault stores secrets and certificates (not general config); App Service Application Settings are per-app and require app restart to update; Table Storage can store config but has no built-in feature flag or push-notification support.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer wants to upload a large file (2 GB) to Azure Blob Storage from a web browser client. The browser should upload **directly to Blob Storage** without routing the file through the application server. What must the developer configure?

- A) Set the storage account to allow public access and provide the container URL to the browser.
- B) Enable **CORS** on the storage account and generate a **SAS token** with write permission scoped to the container; the browser uploads directly using the SAS URL.
- C) Use an Azure Function as a proxy to stream the file to Blob Storage.
- D) Enable Azure CDN on the storage account to handle large uploads.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

For direct browser-to-blob uploads: (1) Configure **CORS** on the storage account to allow the web origin; (2) Generate a **Shared Access Signature (SAS) token** with `Write`/`Create` permissions, scoped to the specific container or blob, with a short expiry. The browser uses the SAS URL to PUT the file directly to Blob Storage. This avoids routing gigabytes through the application server. Public access enables anonymous reads but doesn\'t handle authenticated uploads.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer writes an Azure Durable Function using the **Fan-out/Fan-in** pattern to process 1,000 items in parallel and aggregate results. Which function types are involved?

- A) One Trigger function and one Activity function
- B) One Orchestrator function that calls multiple Activity functions in parallel (using `Task.WhenAll`) and one aggregator Activity function
- C) One Orchestrator function, 1,000 separate Orchestrator sub-functions
- D) One Timer trigger function and multiple HTTP trigger functions

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

In the **Fan-out/Fan-in** Durable Functions pattern:
- The **Orchestrator** fans out by starting multiple **Activity functions** concurrently (using `Task.WhenAll` / `context.df.Task.all()`).
- Each Activity processes one item independently.
- The Orchestrator waits for all activities to complete (fan-in) and then aggregates the results.

Orchestrators must be deterministic and cannot call sub-orchestrators in a loop for simple fan-out; Activity functions perform the actual work. This pattern is natively supported by Durable Functions checkpointing.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to add **custom telemetry** (a business event named `"OrderPlaced"` with order amount) to an existing ASP.NET Core application deployed on Azure App Service that already has Application Insights configured. Which code is correct?

- A) `Log.Information("OrderPlaced: {Amount}", amount);`
- B) `telemetryClient.TrackEvent("OrderPlaced", properties: new Dictionary<string,string> { ["Amount"] = amount.ToString() });`
- C) `Console.WriteLine($"OrderPlaced: {amount}");`
- D) `EventSource.Write("OrderPlaced", new { Amount = amount });`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The Application Insights SDK\'s `TelemetryClient.TrackEvent()` method emits a **custom event** with a name and optional property/metric dictionaries. These appear in the `customEvents` table in the Application Insights workspace and can be queried with KQL, used in dashboards, and set up for alerts. `ILogger` logs go to `traces` not `customEvents`. `Console.WriteLine` is not captured by Application Insights unless stdout collection is enabled.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer is using Azure Cosmos DB for NoSQL. They need to design a container for an e-commerce application where queries frequently filter by `customerId` and retrieve all orders for that customer. What is the optimal **partition key** choice?

- A) `orderId` — ensures maximum distribution of data
- B) `orderDate` — useful for time-range queries
- C) `customerId` — groups all orders for a customer in the same logical partition, enabling efficient single-partition queries
- D) `productId` — groups orders by product for analytics

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Choosing `customerId` as the partition key places all orders for a customer in the same **logical partition**, so queries like `SELECT * FROM c WHERE c.customerId = 'C123'` execute as single-partition queries with low RU cost and low latency. `orderId` provides maximum distribution but makes customer-level queries cross-partition (fan-out). Good partition keys balance distribution (many distinct values) with query alignment. This is one of the most tested Cosmos DB design concepts in AZ-204.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer creates an Azure App Service **deployment slot** named `staging` and deploys a new version of the app there. After testing, they perform a **slot swap** to production. What happens to the production slot\'s app settings that are marked as **slot settings**?

- A) Slot settings swap along with the app content — production gets the staging values.
- B) Slot settings are **sticky** to their slot — they do NOT swap. The production slot keeps its own slot-specific settings even after the swap.
- C) Slot settings are deleted during a swap and must be reconfigured manually.
- D) Slot settings swap but revert automatically after 30 minutes.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Slot settings** (marked with the "Deployment slot setting" checkbox) are **sticky** — they remain with the slot, not the deployed code. This allows each slot to have environment-specific values (e.g., production DB connection string stays in production, staging DB connection string stays in staging) even as the application binaries are swapped between slots. Non-slot settings swap with the code.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to process messages from an Azure Storage Queue using an Azure Function. The function should **batch process up to 16 messages at a time** and should **not delete a message** until the function explicitly marks it as processed. Which binding configuration achieves this?

- A) Use `QueueTrigger` with `batchSize: 16` in `host.json`; messages are completed automatically upon successful function execution.
- B) Use `ServiceBusTrigger` with `maxConcurrentCalls: 16`.
- C) Use `QueueTrigger` with `batchSize: 16` in `host.json`; failed functions automatically return messages to the queue after the `visibilityTimeout` expires.
- D) Storage Queue triggers don\'t support batching — use Event Hubs instead.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

Azure Functions Storage Queue trigger uses PeekLock semantics: messages are made invisible (locked) for the duration of `visibilityTimeout`. The runtime deletes the message only on **successful function completion**. If the function throws, the message becomes visible again after the timeout for retry. `batchSize` (up to 32 for Storage Queues, configurable in `host.json`) controls how many messages are processed concurrently per host instance. This is the correct description of the Storage Queue trigger\'s behavior.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to expose an on-premises REST API to external clients through Azure **without opening inbound firewall ports** on the on-premises network. Which Azure service enables this?

- A) Azure API Management with a self-hosted gateway
- B) Azure Relay — Hybrid Connections
- C) Azure Application Gateway with private IP
- D) Azure Front Door with custom origin

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Relay — Hybrid Connections** (using WebSockets over outbound port 443) allows on-premises services to register a listener that accepts requests relayed through Azure — no inbound firewall rules required. The on-premises service makes an outbound connection to Azure Relay; Azure Relay forwards incoming client requests over that persistent connection. APIM self-hosted gateway deploys a container on-premises but still requires inbound connectivity to APIM; Application Gateway and Front Door require a reachable origin.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer uses the following Cosmos DB SDK code:

```csharp
var response = await container.ReadItemAsync<Order>(
    id: orderId,
    partitionKey: new PartitionKey(customerId)
);
```

The operation throws `CosmosException` with HTTP status **404**. What does this indicate?

- A) The Cosmos DB account is unavailable.
- B) The container does not exist in the database.
- C) No document with the given `id` exists in the specified partition key value.
- D) The SDK version is incompatible with the Cosmos DB API version.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

A **404 Not Found** from `ReadItemAsync` means no item with the specified `id` exists in the logical partition identified by the `partitionKey`. Cosmos DB requires both `id` and `partitionKey` to uniquely locate a point-read item. A missing container would throw a 404 with a different sub-status code (specifically `SubStatusCode = 0` vs `SubStatusCode = 1022` for collection not found). Account unavailability typically surfaces as 503 or a transport exception.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to implement a **webhook receiver** as an Azure Function that validates incoming events from Azure Event Grid. When Event Grid first delivers a subscription validation event, what must the function return?

- A) HTTP 200 with an empty body
- B) HTTP 200 with a JSON body containing the `validationCode` from the incoming event
- C) HTTP 202 Accepted
- D) HTTP 204 No Content

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Event Grid performs a **handshake validation** when a webhook subscription is created. It sends a `Microsoft.EventGrid.SubscriptionValidationEvent` containing a `validationCode`. The webhook endpoint must respond with HTTP 200 and a JSON body: `{ "validationResponse": "<validationCode>" }`. Failure to respond correctly causes Event Grid to reject the subscription. This synchronous handshake prevents event delivery to endpoints not controlled by the subscriber.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer builds an Azure Function with an **HTTP trigger** and deploys it to the Consumption plan. A load test shows that the first requests after a period of inactivity take 5–10 seconds to respond. What causes this and how is it resolved?

- A) The storage account trigger binding is slow — switch to Service Bus.
- B) Cold start latency — the function host is initialized from scratch when idle. Resolve by upgrading to the **Premium Plan** (always-warm pre-warmed instances) or enabling **Always On** in the App Service Plan.
- C) The Consumption Plan limits concurrency to 1 request — switch to a Dedicated plan.
- D) HTTP trigger functions are not supported on the Consumption plan.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Cold start** occurs when a Consumption Plan function host has been idle and scaled to zero — Azure must allocate a new host instance, load the .NET/Node runtime, and initialize the function app before serving the request. The **Premium Plan** keeps pre-warmed instances ready, eliminating cold starts. The Dedicated plan\'s **Always On** setting prevents the host from being unloaded. Cold starts are a key AZ-204 tradeoff between cost (Consumption) and latency (Premium/Dedicated).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to generate a **time-limited, read-only URL** that allows an external partner to download a specific blob from a private Azure Storage container for the next 2 hours, without granting the partner any other storage permissions. Which mechanism should the developer use?

- A) Temporarily change the container\'s access level to "Blob (anonymous read access)".
- B) Generate a **Blob-level SAS token** with `Read` permission and a 2-hour expiry, and append it to the blob URL.
- C) Add the partner\'s IP to the storage account firewall allow-list.
- D) Create a new storage account with public access and copy the blob there.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A **Shared Access Signature (SAS) token** at the blob level grants granular, time-limited access to a specific resource without exposing the storage account key. Parameters include: permissions (`r` = read), resource type (blob `b`), start/expiry times, and optional IP restrictions. The resulting SAS URL is `https://<account>.blob.core.windows.net/<container>/<blob>?<sas-token>`. Making the container public exposes all blobs permanently; firewall rules restrict at the network level but don\'t expire.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer is using the `@azure/service-bus` Node.js SDK to receive messages from a Service Bus queue. They call `receiveMessages()` with `maxMessageCount: 10`. After processing, they call `completeMessage()` on each. However, after a deployment outage lasting 10 minutes, they notice some messages were processed twice. What is the most likely cause?

- A) The Service Bus queue has duplicate detection enabled.
- B) Messages were received with `PeekLock` mode (default), but the lock duration (default 60 seconds) expired during processing before `completeMessage()` was called, making them visible again for re-delivery.
- C) The SDK automatically retries failed `completeMessage()` calls.
- D) Service Bus queues guarantee at-most-once delivery; this behavior is not possible.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Service Bus PeekLock holds a message invisible for the **lock duration** (default 60 seconds, configurable up to 5 minutes). If processing takes longer than the lock duration — due to a slow operation or outage — the lock expires and the message reappears on the queue as if not processed. The consumer must either: (1) renew the lock before expiry via `renewMessageLock()`, or (2) implement idempotent processing. Service Bus queues use **at-least-once** delivery semantics by design.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to call a downstream API from an ASP.NET Core web app on behalf of the signed-in user, passing the user\'s identity to the downstream API. Which OAuth 2.0 flow should be used with Microsoft Entra ID?

- A) Client Credentials Flow — app-only token
- B) Authorization Code Flow — get a new token for each downstream call
- C) On-Behalf-Of (OBO) Flow — exchange the user\'s access token for a new token scoped to the downstream API
- D) Device Authorization Flow — suitable for headless devices

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

The **On-Behalf-Of (OBO) Flow** (RFC 8693 token exchange) allows a middle-tier service to use the user\'s incoming access token to obtain a new access token scoped for a downstream API — preserving the user\'s identity throughout the call chain. Client Credentials uses the app\'s own identity (no user context). Authorization Code requires user interaction. OBO is the standard pattern for service-to-service calls in delegated permission scenarios with Microsoft Entra ID (MSAL supports it via `AcquireTokenOnBehalfOf`).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer containerizes an ASP.NET Core app and pushes the image to **Azure Container Registry (ACR)**. They need to deploy it to **Azure Container Apps** with automatic scaling to zero and scale-out based on HTTP request count. What is the correct approach?

- A) Deploy to Azure Kubernetes Service (AKS) and configure Horizontal Pod Autoscaler.
- B) Deploy to Azure Container Apps, configure the ingress (HTTP), and set the min replicas to 0 with an HTTP scaler rule — Container Apps will scale to zero and out based on concurrent HTTP requests.
- C) Deploy to Azure App Service (container) with autoscale rules based on CPU.
- D) Azure Container Apps do not support scale-to-zero for HTTP workloads.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Container Apps** (built on KEDA) supports scale-to-zero natively for HTTP workloads. Setting `minReplicas: 0` and using the built-in HTTP scaler scales the app to zero when idle and scales out based on concurrent requests. Ingress must be enabled with external or internal traffic. This is a fully managed serverless container platform — no Kubernetes cluster management needed. AKS requires managing node pools; App Service CPU scaling doesn\'t scale to zero.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer uses the Azure SDK to read a blob using the following code:

```python
from azure.storage.blob import BlobServiceClient
client = BlobServiceClient.from_connection_string(conn_str)
blob_client = client.get_blob_client("images", "photo.jpg")
data = blob_client.download_blob().readall()
```

The operation throws `ResourceNotFoundError`. What is the most likely cause?

- A) The connection string is invalid.
- B) The container `images` or blob `photo.jpg` does not exist in the storage account.
- C) The storage account has not been provisioned with blob service enabled.
- D) The `readall()` method is deprecated and should be replaced with `read()`.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

`ResourceNotFoundError` (HTTP 404) means either the **container** (`images`) or the **blob** (`photo.jpg`) does not exist in the storage account. Common causes: typo in container/blob name, blob was deleted, or the container was never created. Connection string errors throw `ValueError` or authentication errors (403). All Azure Blob Storage accounts have blob service available by default. `readall()` is a valid method in the current Azure Blob Storage SDK.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to implement **distributed tracing** across an Azure Function, an Azure App Service API, and an Azure SQL Database so that a single user request can be traced end-to-end in Application Insights. What is the recommended approach?

- A) Add custom `TrackEvent` calls at the start and end of each function.
- B) Enable Application Insights on each service with the **same Instrumentation Key / Connection String**; the SDK automatically propagates `traceparent` / `W3C Trace Context` headers and correlates all telemetry under a single `operation_Id`.
- C) Configure Azure Monitor diagnostic settings to export all logs to the same Log Analytics workspace.
- D) Use Azure Service Bus as a message bus between all services so requests can be correlated by message ID.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Application Insights implements **distributed tracing** using the W3C Trace Context standard (`traceparent` / `tracestate` headers). When all services share the same Application Insights resource (connection string), the SDK auto-instruments outbound HTTP calls to inject correlation headers. Downstream services pick up the headers and associate their telemetry with the same `operation_Id`, producing an end-to-end transaction map in the Application Insights **Application Map** and **Transaction Search**. Diagnostic settings export logs but do not provide distributed trace correlation.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A developer needs to implement the **Saga pattern** in a microservices application on Azure where each service has its own database. A multi-step order process (reserve inventory → charge payment → confirm order) must roll back completed steps if any step fails. Which Azure service combination supports this?

- A) Azure SQL Database with distributed transactions (MSDTC)
- B) Azure Durable Functions (Orchestrator) calling Activity functions for each step, with compensating transactions triggered on failure
- C) Azure Service Bus with dead-letter queue for failed messages
- D) Azure Logic Apps with built-in retry policies

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Durable Functions** is the recommended Azure-native implementation of the **Saga orchestration pattern**. The Orchestrator function drives the multi-step workflow; each Activity function calls a downstream microservice. If a step fails, the Orchestrator calls compensating Activity functions (e.g., release inventory, refund payment) in reverse order. Durable Functions checkpoints state after each step, ensuring the Saga can resume after failures. Distributed transactions (MSDTC) across independent microservice databases violate microservices isolation principles.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 23. AZ-305: Azure Solutions Architect Certification

> *AZ-305 validates skills in designing cloud and hybrid solutions including compute, network, storage, monitoring, identity, and governance at enterprise scale.*

<br>

## Q. An architect is designing a multi-region active-active web application. Each region has its own Azure SQL Database. Users in Asia write data to the Asia region, but reads must reflect data written by any region within 5 seconds. Which Azure SQL Database feature supports this?

- A) Azure SQL Database geo-replication (asynchronous, no multi-region writes)
- B) Azure SQL Database Hyperscale with named replicas
- C) Azure Cosmos DB with Session consistency (SQL API) — not Azure SQL
- D) Azure SQL Database Hyperscale does not support multi-region writes; use Cosmos DB multi-master with Bounded Staleness consistency (max 5-second lag) for this scenario.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: D**

Azure SQL Database does not support multi-region active-active writes natively. For true multi-region write with bounded replication lag (e.g., 5 seconds), **Azure Cosmos DB** with **Bounded Staleness** consistency is the correct choice. Bounded Staleness guarantees reads lag behind writes by no more than K operations or T seconds (configurable). This is a common AZ-305 architecture decision point — choosing the right database for multi-region write requirements.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect must design a solution where 50 branch offices each connect to Azure with a private connection, and all inter-branch traffic is routed through Azure. The solution must minimize the number of VPN tunnels and provide a hub-and-spoke topology that is easy to manage. Which Azure networking service is purpose-built for this?

- A) 50 separate VNet Peerings between branch VNets and a hub VNet
- B) Azure Virtual WAN (vWAN) with branch office VPN connections
- C) 50 Site-to-Site VPN Gateways, one per branch
- D) Azure ExpressRoute with 50 circuits

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Virtual WAN** is a managed, global transit network service that provides hub-spoke connectivity for branches, remote users, VNets, and ExpressRoute circuits from a single control plane. It automatically manages routing between all connected sites (any-to-any via the vWAN hub), supports thousands of branches, and eliminates the operational burden of managing individual VPN tunnels or peerings.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing a solution where an Azure Function needs to call an Azure SQL Database. The database is in a private VNet with no public endpoint. The Function is on the Consumption plan. How should the architect design network connectivity?

- A) Enable the SQL Database public endpoint and restrict by Azure Function outbound IPs.
- B) Upgrade the Function to a Premium or Dedicated plan, enable VNet Integration on the Function App, and configure a private endpoint for the SQL Database in the same VNet.
- C) Use Azure API Management as a proxy between the Function and SQL Database.
- D) Consumption plan Functions cannot access private VNet resources under any circumstance.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure Functions **VNet Integration** (outbound) is supported on Premium and Dedicated plans (not Consumption). It allows Functions to make outbound calls into a VNet where a private endpoint for Azure SQL is configured. The SQL Database\'s private endpoint exposes a private IP in the VNet, and the Function routes to it via the integrated VNet. This is a frequently tested AZ-305 network design pattern.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect needs to design a solution where legacy on-premises applications authenticate using Kerberos/NTLM (Windows Integrated Authentication) but users are managed in Microsoft Entra ID (no on-premises AD). Which Azure service bridges this gap?

- A) Microsoft Entra ID Connect (Azure AD Connect) — requires on-premises AD
- B) Azure AD Domain Services (Microsoft Entra Domain Services) — provides managed Kerberos/NTLM authentication from Entra ID without on-premises AD
- C) Microsoft Entra External ID (B2B)
- D) Azure Active Directory Federation Services (ADFS)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Microsoft Entra Domain Services** (formerly Azure AD Domain Services) provides managed domain services — LDAP, Kerberos, NTLM, Group Policy — backed by Microsoft Entra ID without deploying or managing domain controllers. Legacy apps that require Kerberos/NTLM can join the managed domain and authenticate using Entra ID users. This eliminates the need for on-premises AD while supporting legacy authentication protocols.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing a data pipeline that ingests 1 million events per second from IoT devices, processes them in real-time for anomaly detection, and stores raw events for 90-day historical analysis. Which Azure architecture best fits this?

- A) Azure Queue Storage → Azure Functions → Azure SQL Database
- B) Azure Event Hubs (ingestion, 90-day retention) → Azure Stream Analytics (real-time processing, anomaly detection) → Azure Data Lake Storage Gen2 (historical storage) + Azure Synapse Analytics (batch analysis)
- C) Azure Service Bus Premium → Azure Logic Apps → Azure Blob Storage
- D) Azure API Management → Azure Cache for Redis → Azure Cosmos DB

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

This is the classic **Lambda/Kappa architecture** on Azure: Event Hubs handles massive throughput ingestion with configurable retention (up to 90 days with Event Hubs Premium). Stream Analytics provides real-time processing with built-in anomaly detection ML functions. Data Lake Gen2 stores raw events cheaply at scale, and Synapse Analytics enables historical batch queries. Service Bus is for reliable message delivery, not high-throughput event streaming.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company needs a web application that serves global users with the **lowest possible latency**. Static assets (HTML, CSS, JS, images) should be cached close to users, while dynamic API calls go to an origin in East US. Which Azure service provides global SSL termination, WAF, caching, and intelligent routing in a single service?

- A) Azure Traffic Manager (DNS-based routing)
- B) Azure Application Gateway (regional L7 load balancer)
- C) Azure Front Door (global anycast entry point with CDN, WAF, and intelligent routing)
- D) Azure CDN from Verizon with origin group failover

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Azure Front Door** operates at the network edge across Microsoft\'s global PoP network. It provides: anycast routing (users connect to the nearest PoP), dynamic site acceleration, SSL/TLS offload, WAF policies, custom rules, caching for static content, and health-probe-based failover — all from a single globally distributed service. Traffic Manager is DNS-only (no caching, no SSL offload, no WAF). Application Gateway is a regional service. Azure CDN is purely a caching/delivery service without WAF or dynamic routing.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing an Azure landing zone for a large enterprise. The organization has 8 business units, each requiring isolated billing, separate Azure AD permissions, and independent policy governance. What is the recommended approach?

- A) Create 8 resource groups under a single subscription with separate RBAC assignments.
- B) Create 8 separate Azure subscriptions organized under a Management Group hierarchy, with governance policies applied at the Management Group level.
- C) Create 8 separate Azure AD tenants, one per business unit.
- D) Use a single subscription with resource tags to separate business unit costs.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **Azure landing zone** recommendation uses subscriptions as the unit of scale and isolation. Each business unit gets its own subscription for billing isolation, blast radius containment, and independent RBAC. **Management Groups** provide a hierarchy to apply Azure Policy and RBAC at scale across all subscriptions without repeating assignments. Separate tenants would fragment identity (no SSO across BUs) and are not recommended. Tags alone cannot enforce policy or subscription-level quota isolation.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company requires that all data stored in Azure Storage and Azure SQL Database be **encrypted with their own keys** stored in Azure Key Vault, and they must be able to **revoke the key** to immediately prevent access to data. Which feature satisfies this requirement?

- A) Azure Storage Service Encryption (SSE) with Microsoft-managed keys
- B) Customer-Managed Keys (CMK) with Azure Key Vault and Key Vault soft-delete disabled
- C) Customer-Managed Keys (CMK) stored in Azure Key Vault with **purge protection enabled**, used for Azure Storage SSE and Azure SQL Transparent Data Encryption (TDE)
- D) Client-Side Encryption using a local key store

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Customer-Managed Keys (CMK)** allow the customer to control the encryption key lifecycle. Revoking or deleting the key from Key Vault renders the data inaccessible. **Purge protection** must be enabled on Key Vault to prevent accidental permanent key deletion that would make data unrecoverable. CMK integrates with Azure Storage SSE and Azure SQL TDE. Microsoft-managed keys (option A) don\'t give customers revocation control. Client-side encryption is complex and not natively integrated.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect must design a **disaster recovery** solution for an Azure SQL Database (Business Critical tier) with an **RTO of under 1 hour** and **RPO of under 5 seconds**. Which feature meets both requirements?

- A) Automated geo-backup with restore to a new server (RPO up to 1 week)
- B) Active Geo-Replication — continuous asynchronous replication to a secondary region; manual or automatic failover in minutes; RPO typically < 5 seconds
- C) Azure SQL Database point-in-time restore (PITR)
- D) Azure Site Recovery for Azure SQL Database

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Active Geo-Replication** replicates transactions asynchronously to up to 4 readable secondary replicas in different regions. RPO is typically under 5 seconds (lag of asynchronous replication). RTO for a manual failover is typically under 30 seconds; with **Auto-failover groups** (which use geo-replication under the hood), automatic failover can be triggered with a configurable grace period. PITR restores to the same or new server but can take hours for large databases and RPO is up to 1–7 days for geo-backup. ASR does not support Azure SQL PaaS databases natively.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing a Hub-and-Spoke network topology in Azure. The hub VNet contains a shared **Azure Firewall** that should inspect all traffic between spoke VNets. After peering the hub to two spoke VNets, spoke-to-spoke traffic is NOT flowing through the Azure Firewall. What configuration is missing?

- A) The Azure Firewall needs a public IP address for spoke-to-spoke traffic.
- B) **User-Defined Routes (UDRs)** must be applied to each spoke subnet with the next hop set to the Azure Firewall\'s private IP address for the destination spoke\'s address space.
- C) The spoke VNets must be peered directly with each other in addition to peering with the hub.
- D) Azure Firewall must be in the Premium SKU to support spoke-to-spoke inspection.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

VNet Peering does not transitively route traffic — spoke A cannot reach spoke B through the hub by default. To force traffic through the Azure Firewall in the hub, **User-Defined Routes (UDRs)** must be configured on each spoke subnet with the next hop type `Virtual Appliance` pointing to the Azure Firewall\'s private IP for cross-spoke destinations. Additionally, `Allow Gateway Transit` and `Use Remote Gateways` peering options handle on-premises traffic. This is a core AZ-305 networking design pattern.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A SaaS company needs to allow customers to sign in using their **personal Microsoft accounts, Google accounts, or Facebook accounts**. After sign-in, customers should be able to manage their own profile data. Which Microsoft Entra feature is designed for this B2C scenario?

- A) Microsoft Entra ID B2B Collaboration (guest users)
- B) Microsoft Entra External ID for customers (formerly Azure AD B2C)
- C) Microsoft Entra ID with social identity providers in Conditional Access
- D) Microsoft Entra ID with SAML federation to each social provider

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Microsoft Entra External ID for customers** (formerly **Azure AD B2C**) is purpose-built for consumer-facing (B2C) identity scenarios. It provides identity federation with social providers (Google, Facebook, Apple, Microsoft accounts), customizable sign-up/sign-in user flows, self-service profile management, and a scalable identity store for millions of consumer accounts — all in a separate tenant from the corporate directory. B2B collaboration is for partner/vendor access to your corporate resources, not consumer identity.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect needs to design a **messaging solution** for an e-commerce platform. The `OrderPlaced` event must be delivered to three independent downstream services (inventory, billing, shipping) simultaneously. Each service processes events independently and at its own pace. Which Azure messaging pattern and service should be used?

- A) Azure Service Bus Queue — all three services read from the same queue (competing consumers pattern)
- B) Azure Service Bus Topic with three separate **subscriptions** — each service reads from its own subscription
- C) Azure Storage Queue with three consumers polling the same queue
- D) Azure Event Hubs with a single consumer group shared by all three services

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Service Bus Topics with subscriptions** implement the publish-subscribe pattern. Each subscription receives an independent copy of every message that matches its filter. The three downstream services each have their own subscription — they receive and process the `OrderPlaced` message independently, at their own pace, with individual dead-letter queues and retry policies. A single queue with competing consumers means only one service processes each message. Event Hubs with a shared consumer group also means only one consumer processes each partition.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing a solution where an application\'s secrets (API keys, database passwords) must **never be stored in code or configuration files**. Developers should be able to retrieve secrets by name at runtime. The solution must support secret rotation without application redeployment. Which approach best satisfies all requirements?

- A) Store secrets in Azure Blob Storage with private access and retrieve via Managed Identity.
- B) Store secrets in **Azure Key Vault**; grant the application a Managed Identity with the **Key Vault Secrets User** role; retrieve secrets by name at runtime using the SDK; use Key Vault secret versioning and application reference reload for rotation without redeployment.
- C) Encrypt secrets with AES-256 and store the ciphertext in application settings.
- D) Use Azure App Configuration to store secrets alongside application configuration.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Key Vault** is the purpose-built Azure service for storing and managing secrets, keys, and certificates. The Managed Identity authentication eliminates credential storage. Key Vault supports **secret versioning** — when a secret is rotated (new version created), the application can be configured to always retrieve the **latest version** (omit version in the URI) or reference a specific version. Azure App Configuration can reference Key Vault secrets but is not a replacement for Key Vault storage. Blob Storage has no native secret management or audit logging for individual secret reads.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A company is migrating 200 on-premises VMs to Azure. The VMs run a mix of Windows Server and Linux workloads. The architect needs to **assess which VMs are ready for migration**, estimate Azure costs, and identify dependencies between VMs before migrating. Which Azure service provides this?

- A) Azure Site Recovery (ASR) — disaster recovery service, not migration assessment
- B) Azure Migrate — Discovery and Assessment tool with dependency analysis
- C) Azure Database Migration Service
- D) Azure Advisor cost recommendations

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Migrate** is the unified migration hub. The **Discovery and Assessment** tool deploys a lightweight appliance on-premises that discovers VMs (VMware, Hyper-V, physical), collects performance data, identifies dependencies via **Dependency Analysis** (agentless or agent-based), assesses Azure readiness, and produces sizing recommendations and cost estimates. Azure Site Recovery is for replication/DR; Database Migration Service is database-specific; Azure Advisor analyzes existing Azure resources, not on-premises.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect must design a solution for a web application that needs to **share session state** across multiple App Service instances behind a load balancer so that users don\'t lose their session if routed to a different instance. Which approach is recommended on Azure?

- A) Enable ARR Affinity (sticky sessions) on the App Service load balancer.
- B) Store session state in **Azure Cache for Redis** (distributed cache); all instances read/write session data from the same Redis cache.
- C) Use Azure Service Bus to replicate session state between instances.
- D) Store session state in Azure Blob Storage with a shared access signature.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Cache for Redis** is the recommended distributed session store for horizontally scaled web applications. All App Service instances connect to the same Redis cache and share session state — any instance can serve any user\'s request without affinity. ARR Affinity (sticky sessions) routes users to the same instance, which works but reduces the effectiveness of load balancing and causes cascading failures if an instance goes down. Redis provides high availability, sub-millisecond latency, and native session provider support for ASP.NET/ASP.NET Core.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing a **zero-trust network** for Azure workloads. Applications in `subnet-app` must connect to Azure SQL Database without the connection ever traversing the public internet or being exposed to a public endpoint. Which combination achieves this?

- A) Azure SQL Database firewall rules allowing the App Service outbound IPs
- B) **Private Endpoint** for Azure SQL Database in the application VNet + **Private DNS zone** (`privatelink.database.windows.net`) linked to the VNet + disable Azure SQL public network access
- C) Azure SQL Database VNet service endpoint on `subnet-app`
- D) Azure SQL Database with TLS 1.2 enforced and IP firewall restricted to the VNet address space

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A **Private Endpoint** places a private NIC with a private IP (e.g., `10.0.1.5`) inside the VNet for the Azure SQL Database. Combined with a **Private DNS Zone** that resolves `<server>.database.windows.net` to the private IP (instead of the public IP), all connections stay entirely within the Microsoft backbone — no public internet traversal. **Disabling public network access** on the SQL server ensures no path via the public endpoint exists. VNet service endpoints keep traffic on the Microsoft backbone but the SQL server still has a public endpoint; private endpoints fully privatize the service.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An organization wants to enforce that **no Azure resource can be deployed outside of the regions `eastus` and `westeurope`** across all subscriptions in the organization. Which governance approach achieves this with the least operational overhead?

- A) Train all developers to only select approved regions when deploying resources.
- B) Assign a built-in **Azure Policy** (`Allowed locations`) with `eastus` and `westeurope` as allowed values to the root **Management Group**.
- C) Create a custom RBAC role that restricts the `location` property for resource deployments.
- D) Configure Azure Blueprints to deny deployments in other regions per subscription.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The **`Allowed locations`** built-in Azure Policy with the `deny` effect blocks resource creation in any region not in the allowed list. Assigning it to the **root Management Group** automatically applies it to all subscriptions and resource groups in the entire tenant hierarchy — maximum coverage with a single assignment. Azure Blueprints also works but involves more packaging overhead. RBAC cannot restrict resource location (`location` is a resource property, not a permission action). Manual training is not an enforceable control.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect designs a microservices application on **Azure Kubernetes Service (AKS)**. Individual microservices need to communicate with each other securely using **mutual TLS (mTLS)** without modifying application code. Which approach is recommended?

- A) Configure Kubernetes NetworkPolicy objects to restrict pod-to-pod communication.
- B) Deploy a **service mesh** (e.g., **Istio** or **Open Service Mesh / Azure Service Mesh**) on AKS — it injects sidecar proxies that handle mTLS transparently at the network layer.
- C) Issue TLS certificates manually to each pod using cert-manager.
- D) Use Azure Application Gateway Ingress Controller (AGIC) for east-west traffic.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

A **service mesh** (Istio, Linkerd, or Azure\'s managed **Open Service Mesh**) injects a sidecar proxy (e.g., Envoy) into each pod. The control plane automatically provisions and rotates mTLS certificates; the sidecar handles mTLS handshakes transparently — **zero application code changes required**. NetworkPolicy restricts which pods can communicate but does not provide encryption. Manual cert-manager certificates require application code to handle TLS. AGIC handles north-south (external) traffic, not east-west (service-to-service).

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect must design an **event-driven architecture** where changes to an Azure Cosmos DB container (new orders inserted) should trigger downstream processing (send confirmation emails, update analytics). Which Azure service natively integrates with Cosmos DB change feed for this purpose?

- A) Azure Event Grid subscription on the Cosmos DB account
- B) Azure Functions with **Cosmos DB trigger** (change feed processor)
- C) Azure Logic Apps with a Cosmos DB connector polling on a schedule
- D) Azure Data Factory pipeline with a Cosmos DB source

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Functions Cosmos DB trigger** uses the **Change Feed Processor** library under the hood to monitor a Cosmos DB container for new inserts and updates. It delivers batches of changed documents to the function automatically, with checkpointing managed via a lease container. This is a native, push-based, zero-polling integration. Event Grid can receive Cosmos DB events but requires manual change feed processing. Logic Apps polling is periodic (not real-time). Data Factory is for batch data movement, not event-driven triggers.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A global e-commerce application experiences read-heavy traffic (95% reads, 5% writes) on its Azure SQL Database. Query response times are degrading under load. The architect wants to offload read traffic without application schema changes and maintain automatic failover. Which feature should be used?

- A) Azure SQL Database Elastic Pool to share resources across multiple databases
- B) **Read Scale-Out** using the readable secondary replica (Business Critical tier) or **Geo-Replication readable secondary** — direct read-only connections to the secondary using `ApplicationIntent=ReadOnly` in the connection string
- C) Azure Cache for Redis as a write-through cache for all queries
- D) Increase the DTUs/vCores of the primary database

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure SQL Database **Business Critical** and **Premium** tiers include built-in readable secondary replicas. By adding `ApplicationIntent=ReadOnly` to the connection string, the driver automatically routes the connection to the secondary, offloading read queries from the primary with no schema changes. For cross-region read scaling, **Active Geo-Replication** secondary replicas also accept read connections. Elastic Pools share compute across databases but don\'t reduce read load on a single database. Redis caching helps but requires application changes and cache invalidation logic.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect is designing a **hybrid identity** solution. The company has an on-premises Active Directory with 50,000 users and wants to allow those users to sign in to Azure and Microsoft 365 using their corporate AD credentials, with passwords validated **on-premises** (never synced to the cloud). Which Microsoft Entra ID Connect authentication method should be used?

- A) Password Hash Synchronization (PHS) — hashes are synced to Entra ID
- B) Pass-through Authentication (PTA) — sign-in requests are validated against on-premises AD in real-time via a lightweight agent; no password hashes leave the on-premises environment
- C) Active Directory Federation Services (ADFS) — complex to deploy but required for this scenario
- D) Cloud-only accounts with Azure AD SSPR

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Pass-through Authentication (PTA)** installs lightweight agents on-premises. When a user signs in to Azure/M365, Entra ID forwards the credential validation request to the on-premises AD via the PTA agent — the password is never stored or hashed in the cloud. PHS syncs (hashed) passwords to Entra ID, which violates the "no cloud password sync" requirement. ADFS also keeps passwords on-premises but requires significant infrastructure (proxy servers, WAP, certificates). PTA is simpler than ADFS for this requirement.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An architect needs to design a solution to **host a containerized application** on Azure that requires: GPU nodes for ML inference, auto-scaling to zero when idle, no cluster management overhead, and support for custom domains with HTTPS. Which service is the best fit?

- A) Azure Kubernetes Service (AKS) with GPU node pool and KEDA
- B) Azure Container Instances (ACI) with GPU SKU
- C) **Azure Container Apps** with GPU workload profiles (Dedicated plan), scale-to-zero, and built-in ingress with custom domain + managed certificate
- D) Azure App Service with Docker container and GPU-optimized pricing tier

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

**Azure Container Apps** (GA with GPU workload profiles in 2024) supports dedicated GPU nodes via workload profiles, scale-to-zero (min replicas = 0), built-in HTTPS ingress with custom domain support and automatic managed TLS certificates — all without managing Kubernetes clusters. AKS provides the most control but requires cluster management. ACI supports GPU SKUs but has no built-in custom domain/HTTPS or autoscale. App Service does not support GPU compute.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 24. AZ-400: Azure DevOps Engineer Certification

> *AZ-400 validates skills in designing and implementing DevOps practices including source control, CI/CD, infrastructure as code, monitoring, and security integration.*

<br>

## Q. A DevOps engineer needs to implement a branching strategy that supports multiple versions of a product in production simultaneously (e.g., v1.x still receiving security patches while v2.x is in active development). Which Git branching strategy is most appropriate?

- A) Trunk-Based Development with feature flags
- B) Git Flow with `main`, `develop`, `release/v1.x`, `release/v2.x`, `hotfix/*`, and `feature/*` branches
- C) GitHub Flow with a single `main` branch and short-lived feature branches
- D) Feature Branch Workflow with one branch per developer

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Git Flow** explicitly supports parallel version maintenance through long-lived release branches (`release/v1.x`, `release/v2.x`). Hotfixes can be cherry-picked to both. Trunk-Based Development and GitHub Flow work best for teams deploying a single version continuously — they do not naturally support simultaneous multi-version maintenance without complexity.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer wants to prevent secrets from being committed to the Azure Repos Git repository. Which mechanism detects secrets **before** they are pushed to the remote repository?

- A) Azure DevOps branch policies with a required build validation pipeline
- B) A pre-commit hook (e.g., `git-secrets`, `detect-secrets`, or `gitleaks`) installed on developers' machines, combined with a pipeline secret-scanning step as a second line of defense
- C) Azure Key Vault with access policies for the repository
- D) Microsoft Defender for DevOps repository scanning (post-push only)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Pre-commit hooks run locally before a commit is created, catching secrets before they ever enter Git history. Tools like `gitleaks`, `detect-secrets`, or `git-secrets` scan staged files for patterns. A pipeline scanning step (Defender for DevOps, GitHub Advanced Security for ADO) acts as a second gate after the push. Defender for DevOps scans the remote repo — it does not prevent the push from occurring.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer is designing a release pipeline for a microservices application on AKS. The team requires that a new version is deployed to 10% of users first, monitored for error rate increases, and automatically promoted to 100% if healthy or rolled back if error rate exceeds 1%. Which deployment strategy implements this?

- A) Blue-Green deployment
- B) Canary release with automated progressive delivery (e.g., using Argo Rollouts or Azure DevOps + Application Insights release gates)
- C) Rolling update with `maxSurge: 1`
- D) Recreate deployment strategy (scale old to 0, deploy new)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Canary release with automated progressive delivery** routes a small percentage of traffic to the new version, monitors metrics (error rate, latency) against defined thresholds, and automatically promotes or rolls back. Azure DevOps release gates integrate with Application Insights to query metrics before promoting between stages. Argo Rollouts on AKS provides native Kubernetes canary with metric-based analysis. Blue-Green switches 100% at once; rolling updates don\'t support traffic percentage control natively.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An AZ-400 engineer is setting up a dependency management strategy. The team uses internal NuGet packages shared across 15 microservices. They want a central repository for internal packages with access control, versioning, and upstream proxy to nuget.org. Which Azure service provides this?

- A) Azure Blob Storage with a custom NuGet feed URL
- B) Azure Artifacts with a NuGet feed (supports upstreaming, access control, versioning, and retention policies)
- C) GitHub Packages with a public registry
- D) Azure Container Registry (for NuGet packages)

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure Artifacts** is the package management service in Azure DevOps that supports NuGet, npm, Maven, Python (PyPI), and Cargo feeds. It supports upstream sources (proxying nuget.org so all package traffic routes through Artifacts), RBAC for feed access, semantic versioning, and retention policies to control storage. ACR is for container images, not NuGet packages.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer implements a Continuous Integration pipeline. A code review policy requires that all builds pass before a PR can be merged. However, the full test suite takes 45 minutes. What technique reduces the feedback loop without removing test coverage?

- A) Remove integration tests from the CI pipeline entirely.
- B) Implement test parallelization (split tests across multiple agents) and test impact analysis (run only tests affected by changed files), targeting a sub-10-minute PR validation pipeline.
- C) Increase the pipeline agent\'s VM size to speed up the build.
- D) Merge without tests and run the full suite nightly.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Test parallelization** distributes test execution across multiple agents simultaneously (Azure DevOps supports parallel jobs with `strategy: parallel`). **Test Impact Analysis** (TIA) — supported in Visual Studio Test task — identifies which tests are affected by code changes and runs only those, skipping unrelated tests. Together these techniques reduce PR feedback time from 45 minutes to under 10 minutes while maintaining coverage. Nightly-only testing is a risk management failure.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer needs to deploy infrastructure to Azure using **Terraform** in an Azure DevOps YAML pipeline. The Terraform state file must be stored remotely and shared across pipeline runs. Where should the state file be stored?

- A) In the Git repository alongside the Terraform code
- B) In an **Azure Storage Account** (blob container) configured as the Terraform backend — provides locking, versioning, and shared access across all pipeline agents
- C) On the Azure DevOps pipeline agent\'s local disk
- D) In Azure Key Vault as a secret

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Terraform\'s **`azurerm` backend** stores state in Azure Blob Storage. This provides: (1) **State locking** via blob lease to prevent concurrent `terraform apply` conflicts, (2) **versioning** via storage account versioning for state history, (3) **shared access** so any pipeline agent can retrieve the latest state. Storing state in Git exposes sensitive resource metadata and causes merge conflicts. Local disk state is lost when the ephemeral agent is recycled. Key Vault is for secrets, not structured state files.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer configures a YAML pipeline in Azure DevOps with the following stage structure. What does the `dependsOn: []` setting on the `DeployProd` stage mean?

```yaml
stages:
- stage: Build
- stage: DeployDev
  dependsOn: Build
- stage: DeployProd
  dependsOn: []
```

- A) `DeployProd` runs only after all other stages complete.
- B) `DeployProd` has no dependencies and runs **in parallel** with `Build` and `DeployDev` at pipeline start.
- C) `DeployProd` is skipped if `DeployDev` fails.
- D) `DeployProd` requires manual approval before running.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

In Azure DevOps YAML pipelines, `dependsOn: []` (empty array) removes all implicit dependencies, causing the stage to run immediately when the pipeline starts — **in parallel** with all other stages that have no blocking dependencies. This is distinct from omitting `dependsOn` entirely (which defaults to the previous stage in the file). This is a common exam trap that tests understanding of YAML pipeline dependency graphs.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer needs to store a sensitive API key in an Azure DevOps pipeline and use it as an environment variable inside a build job, ensuring the value is **never printed in logs**. What is the correct approach?

- A) Hard-code the key in the YAML pipeline file and mark the variable as `readonly`.
- B) Store the key as a **secret variable** in the Azure DevOps pipeline variable group (marked as secret); reference it with `$(MY_SECRET)` in the pipeline — Azure DevOps automatically masks the value in all log output.
- C) Store the key in a `.env` file committed to the repository and load it in the pipeline.
- D) Pass the key as a pipeline parameter in the `parameters:` block.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure DevOps **secret variables** (marked with the lock icon in the pipeline UI or variable group) are: (1) encrypted at rest, (2) never passed to forked PR builds by default, (3) automatically **masked in all log output** — any occurrence of the value is replaced with `***`. The recommended pattern is to store secrets in **Azure Key Vault** linked to an Azure DevOps variable group (Key Vault integration), so the pipeline retrieves secrets at runtime without storing them in DevOps. Hard-coding or committing secrets to Git violates security best practices and would be flagged by secret-scanning tools.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer sets up a **GitHub Actions** workflow to deploy a containerized app to Azure Kubernetes Service. Instead of storing Azure service principal credentials as GitHub secrets, they want to use a **credential-free** authentication method. Which approach should they use?

- A) Store the Azure subscription ID and tenant ID as GitHub secrets and use them directly.
- B) Configure **Workload Identity Federation** (OIDC) between GitHub Actions and Microsoft Entra ID — GitHub\'s OIDC token is exchanged for an Azure access token with no stored secrets.
- C) Generate a long-lived service principal client secret and store it as `AZURE_CREDENTIALS` in GitHub secrets.
- D) Use Azure Managed Identity on the GitHub Actions runner VM.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Workload Identity Federation** with OIDC lets GitHub Actions present a short-lived OIDC token (issued by GitHub\'s identity provider) to Microsoft Entra ID in exchange for an Azure access token. No client secrets or certificates are stored anywhere — they don\'t exist. The `azure/login` GitHub Action supports this via `client-id`, `tenant-id`, `subscription-id` and `permissions: id-token: write` in the workflow. This is the current Microsoft recommended best practice and is frequently tested in AZ-400.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer is implementing **Infrastructure as Code** with Azure Bicep. They need to deploy a module that creates an Azure Storage account only when the environment is `production`. Which Bicep feature controls conditional resource deployment?

- A) A `for` loop with a filter expression
- B) The `if` condition on the resource declaration: `resource storageAccount 'Microsoft.Storage/storageAccounts@2023-01-01' = if (env == 'production') { ... }`
- C) A `@allowed` decorator on the parameter
- D) A `targetScope` set to `subscription`

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Bicep supports **conditional resource deployment** using an `if` expression directly on the resource declaration. The resource is only deployed when the condition evaluates to `true`. This is equivalent to ARM template\'s `"condition"` property. The `@allowed` decorator restricts parameter values but doesn\'t control deployment. `targetScope` sets the deployment scope (subscription, resource group, etc.), not conditions. `for` loops iterate over arrays for resource copies.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. An Azure DevOps pipeline deploys to a production environment. The team requires that **at least 2 approvers from the "Release Approvers" group** must approve before the deployment stage runs, and the approval must happen within 48 hours or the deployment is auto-rejected. Which Azure DevOps feature enforces this?

- A) Branch policies with required reviewers on the `main` branch
- B) **Environment approvals and checks** on the Azure DevOps Environment resource — configure an "Approvals" check with required approvers count, approver group, and timeout
- C) Pipeline variable group with a manual validation task
- D) Azure DevOps work item linking requirement on the release

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

Azure DevOps **Environments** support **Approvals and Checks** natively. You configure: approver users/groups, minimum required approvers, instructions, and a timeout (after which the check auto-rejects). The deployment stage gates on this check before proceeding. Branch policies control PR merges, not deployment approvals. The `ManualValidation` task exists in YAML pipelines as an alternative but doesn\'t support group-based approvals as cleanly. Environments are the recommended YAML pipeline deployment gate mechanism.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer runs `terraform plan` in a pipeline and sees the output: `Plan: 0 to add, 1 to change, 1 to destroy`. The engineer wants the pipeline to **fail** and alert the team if any resource destruction is planned, preventing accidental deletions. How should this be implemented?

- A) Add a manual approval step after every `terraform plan`.
- B) Run `terraform plan -out=tfplan`, then run `terraform show -json tfplan | jq` to parse the plan JSON and fail the pipeline step if any resource with `"change": {"actions": ["delete"]}` is detected.
- C) Use `terraform apply -target` to only apply changes to specific resources.
- D) Enable `prevent_destroy = true` in every resource\'s lifecycle block.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The recommended CI pattern is: `terraform plan -out=tfplan` → `terraform show -json tfplan > plan.json` → parse `plan.json` with `jq` (or a policy tool like **OPA/conftest** or **Checkov**) to detect destructive actions. If a `"delete"` or `"replace"` action is found, the pipeline step exits with a non-zero code, failing the pipeline and alerting the team. `prevent_destroy` blocks Terraform from destroying in the state but requires code changes per resource. `-target` applies selectively but doesn\'t prevent the issue.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer needs to implement **Static Application Security Testing (SAST)** and **Software Composition Analysis (SCA)** in an Azure DevOps pipeline to detect code vulnerabilities and vulnerable open-source dependencies before merging to `main`. Which tools integrate natively with Azure DevOps?

- A) Azure Advisor and Microsoft Defender for Cloud
- B) **GitHub Advanced Security for Azure DevOps (GHAS for ADO)** — provides CodeQL SAST scanning and dependency vulnerability alerts natively in Azure Repos pull requests
- C) Azure Policy with a custom security initiative
- D) Azure Monitor with Log Analytics workspace alerts

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**GitHub Advanced Security for Azure DevOps (GHAS for ADO)** brings CodeQL-powered SAST (static analysis for code vulnerabilities), secret scanning, and dependency review (SCA — scanning `package-lock.json`, `pom.xml`, etc. for known CVEs via the GitHub Advisory Database) directly into Azure Repos. Results appear as PR annotations and in the Advanced Security dashboard. This is the AZ-400 exam\'s primary reference for integrated DevSecOps tooling in Azure DevOps. Azure Advisor and Defender for Cloud operate on deployed Azure resources, not source code.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer is configuring **Azure DevOps pipeline agents**. For security reasons, the company cannot allow Microsoft-hosted agents to access internal Azure resources (databases, storage) in a private VNet. What is the recommended solution?

- A) Whitelist all Microsoft-hosted agent IP ranges in the Azure firewall.
- B) Deploy **self-hosted agents** on Azure VMs or Azure Container Instances inside the same VNet (or a peered VNet) as the private resources; register them in an Azure DevOps agent pool.
- C) Use Azure DevOps service connections with private endpoint support.
- D) Deploy all resources with public endpoints and restrict access using NSG rules.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Self-hosted agents** are VMs, containers, or on-premises machines that you control and register with Azure DevOps. By deploying them inside or peered to the private VNet, they can reach private endpoints, internal databases, and storage accounts. Microsoft-hosted agent IP ranges are large and change frequently — maintaining firewall rules is operationally fragile and a security risk. Azure DevOps Scale Set agents (using Azure VM Scale Sets) provide a managed self-hosted option with autoscaling.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer configures the following Azure DevOps YAML pipeline trigger. When will the pipeline run?

```yaml
trigger:
  branches:
    include:
      - main
      - release/*
  paths:
    exclude:
      - docs/**
      - '*.md'
```

- A) On every commit to any branch, excluding changes to `docs/` and `.md` files.
- B) On commits to `main` or any `release/*` branch, **only when the commit includes changes outside** `docs/**` or `*.md` files.
- C) The pipeline runs manually only — `trigger` without `pr:` disables automatic triggers.
- D) On every pull request targeting `main` or `release/*` branches.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The `trigger` block with `branches.include` limits CI triggers to `main` and `release/*` branches. The `paths.exclude` filter means: if **all** changed files in the push match the excluded patterns (`docs/**` or `*.md`), the pipeline is **not triggered**. If at least one changed file falls outside the exclusions, the pipeline triggers. This is a documentation-only change exclusion pattern commonly used to avoid unnecessary CI runs for markdown/docs-only commits.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps team practices **continuous deployment** to production. After a bad deployment, the team needs to roll back to the last known good version within 5 minutes. Which strategy provides the fastest rollback on Azure App Service?

- A) Re-run the previous pipeline to redeploy the last good build artifact.
- B) Use **App Service deployment slots** — swap staging (previous good version) back to production with a single swap operation (< 1 minute), with zero downtime.
- C) Restore the App Service from an Azure Backup snapshot.
- D) Scale down the new instances and scale up old instances from a VM image.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Deployment slot swap** is the fastest rollback mechanism for Azure App Service. When deploying, the new version is deployed to the staging slot first; production holds the last good version. If the new deployment has issues, a single swap back to the previous slot takes under 60 seconds with zero downtime (App Service warms up the slot before switching traffic). Re-running a pipeline takes minutes to tens of minutes. Backup restore is slow (minutes to hours). VM image rollback doesn\'t apply to PaaS App Service.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer needs to implement **compliance as code** — ensuring all Terraform-provisioned Azure resources meet security policies (e.g., storage accounts must have `https_only = true`, VMs must not have public IPs) **before** `terraform apply` is run. Which tool enforces pre-apply policy checks?

- A) Azure Policy (only enforces post-deployment on existing resources)
- B) **Checkov** or **OPA/Conftest** — policy-as-code tools that scan Terraform plan JSON files and fail the pipeline if violations are found before any resource is provisioned
- C) Microsoft Defender for Cloud (scans deployed resources, not Terraform plans)
- D) Azure Security Center recommendation alerts

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Checkov** (by Bridgecrew/Prisma Cloud) and **OPA with Conftest** scan Terraform source code or plan JSON files for policy violations — before any resource is created. They can be added as a pipeline step: `checkov -d . --framework terraform` fails the pipeline if any defined policy (e.g., storage must have HTTPS only) is violated. Azure Policy and Defender for Cloud are reactive (they evaluate deployed resources); for **shift-left** pre-deployment enforcement in a DevOps pipeline, policy-as-code tools like Checkov are the standard AZ-400 answer.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer sets up monitoring for a production pipeline. They want to be **automatically notified** when a pipeline in Azure DevOps fails on the `main` branch, and the notification should be sent to a Microsoft Teams channel. What is the simplest configuration?

- A) Write a custom Azure Function that polls the Azure DevOps REST API every minute.
- B) Configure an **Azure DevOps service hook** to send a `build.complete` (status: failed) event to the **Microsoft Teams incoming webhook** URL for the channel — no custom code required.
- C) Enable Azure Monitor alerts on the pipeline agent VM\'s CPU metric.
- D) Set up an Azure Logic App that triggers on a timer and calls the ADO API.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

**Azure DevOps Service Hooks** natively integrate with Microsoft Teams (and Slack, WebHooks, etc.). You configure a service hook subscription for the `Build completed` event with a filter `Status = Failed` and branch `= main`, pointing to a Teams incoming webhook URL. No code is needed — this is a first-class integration in Azure DevOps Project Settings → Service Hooks. Logic Apps and Azure Functions work but add unnecessary complexity for this built-in integration.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer is implementing **Blue-Green deployment** for a containerized application running on Azure Kubernetes Service. After deploying the green (new) version, they want to switch 100% of traffic from blue to green **instantaneously** with a single command. Which Kubernetes resource controls traffic routing between the two deployments?

- A) Kubernetes Ingress resource with weighted routing rules
- B) Update the **Kubernetes Service** selector to point from the blue deployment\'s pod label to the green deployment\'s pod label — traffic switches instantly to the green pods.
- C) Scale the blue deployment to 0 replicas and wait for green to scale up.
- D) Delete the blue deployment after the green deployment is healthy.

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

In Kubernetes Blue-Green deployments, both blue (`app: myapp, version: blue`) and green (`app: myapp, version: green`) Deployments run simultaneously. The **Service** selector defines which pods receive traffic. Patching the Service selector from `version: blue` to `version: green` (e.g., `kubectl patch service myapp -p '{"spec":{"selector":{"version":"green"}}}'`) is **instantaneous** — all new connections immediately go to green pods. No downtime, no scaling. This is the canonical Kubernetes Blue-Green pattern tested in AZ-400.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer is designing a **multi-stage YAML pipeline** with the following requirements: the `DeployProd` stage must only run if the source branch is `main`, the `Build` stage succeeded, and a manual approval is granted. Which combination of YAML constructs achieves all three conditions?

- A) `condition` expression on the stage + `dependsOn: Build` + Environment approval check
- B) A `trigger` filter on the stage + `dependsOn: Build` + Manual validation task
- C) `condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))` on `DeployProd` + `dependsOn: Build` + **Environment approvals check** on the production Environment
- D) Branch policy on `main` + `dependsOn: Build` + `timeout: 48h` on the stage

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: C**

This combines three orthogonal YAML pipeline mechanisms:
1. **`condition:`** — evaluates `succeeded()` (Build passed) AND checks the source branch is `main` using `eq(variables['Build.SourceBranch'], 'refs/heads/main')`.
2. **`dependsOn: Build`** — ensures `DeployProd` only runs after `Build` completes.
3. **Environment approval check** — pauses the stage and waits for manual approval from the designated approvers before any deployment jobs run.

Together these enforce all three requirements without overlap. `trigger` filters apply to pipeline entry, not individual stages.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. A DevOps engineer is tasked with measuring **DORA metrics** for the engineering team. Which four metrics constitute the DORA (DevOps Research and Assessment) framework?

- A) Code coverage, build success rate, test pass rate, deployment frequency
- B) **Deployment Frequency**, **Lead Time for Changes**, **Change Failure Rate**, **Mean Time to Restore (MTTR)**
- C) Sprint velocity, defect escape rate, release cycle time, incident count
- D) Mean Time Between Failures (MTBF), availability SLA, error budget burn rate, deployment frequency

<details>
<summary><strong>Answer & Explanation</strong></summary>

**Answer: B**

The four **DORA metrics** (from Google\'s DevOps Research and Assessment program) are:
1. **Deployment Frequency** — how often code is deployed to production
2. **Lead Time for Changes** — time from code commit to production deployment
3. **Change Failure Rate** — percentage of deployments that cause a production failure
4. **Mean Time to Restore (MTTR)** — how quickly the team recovers from a production incident

These metrics are used to classify teams as Elite, High, Medium, or Low performers. AZ-400 explicitly tests knowledge of DORA metrics as part of the DevOps metrics and monitoring domain.

</details>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

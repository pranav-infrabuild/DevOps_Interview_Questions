<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:2699E6,100:FFB347&height=140&section=header&text=AZURE%20TECHNICAL%20BASED%20INTERVIEW%20QUESTIONS&fontSize=28&fontColor=fff" />
</p>


## 🌐 🔹 Q1. Tenant Root Group / Azure Tenant

<details>

When your company signs up for Azure, Microsoft creates a Tenant — a dedicated, isolated instance of Azure Entra ID just for your organization. Think of it as your company's identity headquarters in Microsoft's cloud.

### What Lives at the Tenant Level?
- All your user identities (employees, service accounts)
- All your groups (IT-Team, Developers, Finance-Team)
- All your subscriptions hang under this
- Your Azure policies at the broadest level
- Your security configurations (MFA, Conditional Access)

### Who Controls the Tenant?

| Role | What They Can Do |
|------|----------------|
| Global Administrator | Full control over Entra ID — create users, reset passwords, manage licenses, configure security |
| Owner (at Root) | Full control over all Azure resources across all subscriptions |

### Key Point
The tenant is the trust boundary. Everything inside one tenant trusts each other's identities. You cannot have Azure resources without a tenant — it's the foundation of identity and access.

</details>

---

## 🏗️ 🔹 Q2. What is a Management Group?

<details>

This sits between the Tenant and Subscriptions — it's a way to group subscriptions together and apply policies/access at scale.

### Why Does This Matter?

Without Management Groups:
- You'd have to apply the same policy to multiple subscriptions one by one
- One missed subscription = security gap

With Management Groups:
- Apply a policy once → automatically applies to all subscriptions
- Assign RBAC role at group level → inherited by all subscriptions below

### Real-World Use Case
Apply a security policy to a Production Management Group → enforced across all production subscriptions

</details>

---

## 💳 🔹 Q3. What is a Subscription?

<details>

A subscription serves two purposes simultaneously:

### Purpose 1 — Billing Boundary:
- Separate billing for departments with budgets, invoices, and cost tracking.

### Purpose 2 — Resource Container:
- All Azure resources live inside a subscription. You cannot create resources without a subscription.

### Access Roles:
- Owner → Full access
- Contributor → Manage resources only
- Reader → View only
- User Access Administrator → Manage access only

</details>

---

## 📤 🔹 Q4. How to Export Users from Azure AD (Entra ID)?

<details>

### 🔹 Using Azure CLI

```bash
az ad user list \
  --output table \
  --query "[].{Name:displayName, Email:mail, UPN:userPrincipalName}"
```

### 🔹 Explanation
- `az ad user list` → Fetches all users  
- `--output table` → Displays output in table format  
- `--query` → Filters specific fields (Name, Email, UPN)

</details>

---

## 🔐 🔹 Q5. What is Azure Lock?

<details>

Azure Locks are governance controls that prevent accidental deletion or modification of resources — even by users with high privileges like Owners.

### 🔹 Types of Azure Locks

#### 1. CanNotDelete Lock
- Resource can be **read and modified**
- But **cannot be deleted**

```bash
az lock create \
  --name "protect-prod-db" \
  --resource-group myRG \
  --resource-name mySQL \
  --resource-type Microsoft.Sql/servers \
  --lock-type CanNotDelete
```

---

#### 2. ReadOnly Lock
- Resource can only be **read**
- Cannot be **modified or deleted**

```bash
az lock create \
  --name "freeze-prod-vnet" \
  --resource-group myRG \
  --lock-type ReadOnly
```

---

### 🔹 Lock Inheritance

```
Lock on Resource Group
        ↓ (inherited)
All resources inside that RG are also locked
```

---

### 🔹 Who Can Remove Locks?
- Only users with:
  - **Owner**
  - **User Access Administrator**

---

### 🔹 Interview Answer

"Azure Locks are governance controls used to prevent accidental changes to critical resources. There are two types — CanNotDelete and ReadOnly.

CanNotDelete allows modifications but blocks deletion, which we typically use for production databases or key vaults. ReadOnly restricts both modification and deletion, useful during audits or change freezes.

Locks are inherited, so applying a lock at the resource group level protects all resources inside it."

</details>

---

## 🏷️ 🔹 Q6. What Are Tags?

<details>

Tags are **key-value pairs** attached to Azure resources for **organization, cost tracking, and automation**.

---

### 🔹 Example (Azure CLI)

```bash
az resource tag \
  --tags Environment=Production \
         Team=DevOps \
         CostCenter=IT-001 \
         Project=EcommerceApp \
  --resource-group myRG
```

---

### 🔹 Use Cases

#### 1. Cost Management
```text
Tag: CostCenter=Marketing
→ Azure Cost Management shows all marketing spend
```

#### 2. Automation
```text
Tag: AutoShutdown=true
→ Azure Automation runbook shuts down tagged VMs atnight
```

#### 3. Governance via Azure Policy
```text
Policy: "Require Environment tag on all resources"
→ Resources without this tag cannot be created
```

---

### 🔹 Interview Answer

"Tags are metadata labels — key-value pairs attached to Azure resources. We use them mainly for cost allocation, automation, and governance. 

For example, CostCenter tags help map cloud spend to departments, AutoShutdown tags can trigger automation like turning off VMs, and Azure Policy ensures required tags are enforced on all new resources."

</details>

---

## 🆔 🔹 Q7. What is MS Entra ID (Azure Active Directory)?

<details>

Microsoft Entra ID (formerly Azure Active Directory) is Azure’s **identity and access management platform**.

It is responsible for:
- **Authentication** → Who are you?
- **Authorization** → What can you access?

---

### 🔹 What Exists in Entra ID?
- Users (employees, admins)
- Groups (teams, departments)
- Service Principals (applications, automation identities)
- Managed Identities (for Azure resources)

---

### 🔹 Key Features
- **Single Sign-On (SSO)** → One login for multiple applications
- **Multi-Factor Authentication (MFA)** → Adds extra security layer
- **Conditional Access** → Control access based on:
  - Device compliance
  - Location
  - Risk level

---

### 🔹 Real-World Usage
- Central identity system for all Azure resources
- Secure access to applications like Azure Portal, Office 365, etc.
- Enforce security policies across the organization

---

### 🔹 Interview Answer

"Microsoft Entra ID is Azure’s identity platform that handles authentication and authorization. All users, groups, and service principals are managed here. 

We use it to enable SSO across applications, enforce MFA for all users, and apply Conditional Access policies to restrict access based on device compliance or location."

</details>

---

## 🌐 🔹 Q8. What Is a Subnet?

<details>

A Subnet is a **subdivision of a Virtual Network (VNet)**. It allows you to divide a large network into smaller segments for better organization, security, and scalability.

---

### 🔹 Example

```
VNet: 10.0.0.0/16 (65,536 IPs)

├── Subnet: WebTier   → 10.0.1.0/24 (256 IPs)
├── Subnet: AppTier   → 10.0.2.0/24 (256 IPs)
└── Subnet: DBTier    → 10.0.3.0/24 (256 IPs)
```

---

### 🔹 IP Calculation Example (/24)

- Total IPs: **256**
- Azure Reserved IPs:
  - `.0` → Network address  
  - `.1` → Default gateway  
  - `.2` → Azure DNS  
  - `.3` → Azure DNS  
  - `.255` → Broadcast  

✅ **Usable IPs:**  
`256 - 5 = 251 usable IPs`  
Range: **10.0.1.4 → 10.0.1.254**

---

### 🔹 Why Use Subnets?
- Separate tiers (Web, App, DB)
- Apply different **Network Security Groups (NSGs)**
- Control traffic flow and improve security
- Better IP address management

---

### 🔹 Interview Answer

"A subnet is a logical subdivision of a VNet used to organize resources into tiers like web, app, and database. 

In Azure, each subnet reserves 5 IP addresses — network, gateway, two DNS, and broadcast. So a /24 subnet provides 256 total IPs but only 251 usable. This is important for capacity planning in large deployments."

</details>

---

## 🔗 🔹 Q9. What Is VNet Peering?

<details>

Connecting two Virtual Networks so resources can communicate privately using Microsoft's backbone — no public internet, no VPN, very low latency

```
VNet-A (10.0.0.0/16)  ←——Peering——→  VNet-B (10.1.0.0/16)
VM: 10.0.1.4                          VM: 10.1.1.4
```

VM in VNet-A can ping VM in VNet-B as if they're on the same network.

---

### Types:

#### Regional Peering: Same Azure region
```text
VNet-EastUS ←→ VNet-EastUS-2 (same region)
```

#### Global Peering: Different Azure regions
```text
VNet-EastUS ←→ VNet-WestEurope (cross-region)
```

---

### Key Rules:

VNet address spaces must not overlap (10.0.0.0/16 and 10.0.0.0/24 would conflict)

Peering is non-transitive by default:

```text
VNet-A ←→ VNet-B ←→ VNet-C
VNet-A CANNOT reach VNet-C automatically
(Need to peer A-C directly, or use Azure Virtual WAN/Hub)
```

---

### Interview Answer:

"VNet peering connects two VNets privately over Microsoft's backbone with low latency and no bandwidth overhead. It's non-transitive — if A peers with B and B peers with C, A still can't reach C. For hub-and-spoke architectures with many VNets, we use Azure Virtual WAN or a hub VNet with peering to manage transitive routing."

</details>

---

## 🛡️ 🔹 Q10. What is Application Security Group (ASG)

<details>

### Problem NSGs Alone Have:
```text
# Without ASG — you specify IP addresses
Allow traffic from: 10.0.1.4, 10.0.1.5, 10.0.1.6 to port 1433
```

When IPs change or scale sets spin up new VMs, you have to update rules.

---

### How ASG Solves This:
```text
# Create ASGs (logical groups)
ASG: WebServers   → Assign to all web VMs
ASG: DBServers    → Assign to all database VMs

# NSG rule uses ASG names instead of IPs
Allow: WebServers → DBServers on port 1433
```

Now when new web VMs are added, just add them to the WebServers ASG — the NSG rule automatically applies.

</details>



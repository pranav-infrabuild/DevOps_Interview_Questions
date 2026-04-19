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

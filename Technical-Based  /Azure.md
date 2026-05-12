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

## 🌐 🔹 Q8. What is Azure App Service?

<details>

Azure App Service is a fully managed Platform-as-a-Service offering from Microsoft that lets you build, deploy, and scale web applications and APIs without managing the underlying infrastructure.

Think of it like a furnished apartment — you move in with just your belongings, and all the plumbing and electricity is already taken care of.

### Key Features
- Fully managed PaaS hosting
- No server management required
- Automatic OS patching and runtime management
- Built-in load balancing
- Easy scaling
- Support for multiple runtimes

### Supported Runtimes
- Python
- Node.js
- Java
- .NET

### Real-World Use Case
Host Flask, FastAPI, web applications, and APIs without worrying about infrastructure management.

### Interview Answer

"Azure App Service is a fully managed Platform-as-a-Service offering from Microsoft that lets you build, deploy, and scale web applications and APIs without managing the underlying infrastructure. You just bring your code — Azure handles the OS, runtime, patching, and load balancing automatically. It supports multiple runtimes like Python, Node.js, Java, and .NET. Think of it like a furnished apartment — you move in with just your belongings, and all the plumbing and electricity is already taken care of."

</details>

---

## 🏗️ 🔹 Q9. What is the difference between App Service and a Virtual Machine?

<details>

Azure App Service and Virtual Machines differ mainly in infrastructure ownership and operational responsibility.

### App Service (PaaS)
- Microsoft manages infrastructure
- No OS access
- Automatic patching
- Built-in scaling
- Faster deployments
- Best for modern applications

### Virtual Machine (IaaS)
- Full OS control
- Manual patching and maintenance
- Install any software
- Custom configurations possible
- Best for legacy/custom applications

### Key Difference
App Service removes infrastructure overhead, while Virtual Machines provide complete control.

### Interview Answer

"A Virtual Machine gives you full control over the OS — you decide what gets installed, patched, and configured. That's IaaS. App Service, on the other hand, is PaaS — Microsoft manages the infrastructure layer entirely. You only deploy your application code. I'd choose a VM when I need a custom OS configuration or legacy software, but App Service is my preference for modern web apps because it eliminates operational overhead and lets the team focus purely on business logic."

</details>

---

## ⚙️ 🔹 Q10. What is an App Service Plan?

<details>

An App Service Plan defines the compute resources used by your application.

### It Defines
- Region
- Number of instances
- Pricing tier
- Performance capacity
- Feature availability

### Important Point
Multiple App Service apps can share a single App Service Plan, making it cost-efficient.

### Feature Availability
- Free / Basic → Limited features
- Standard and above → Autoscaling + Deployment Slots

### Interview Answer

"An App Service Plan defines the region, number of instances, and the pricing tier for your app. It's essentially the compute resource that your app runs on. What's interesting is that multiple App Service apps can share a single plan — which makes it cost-efficient. The plan also determines what features you get: for example, autoscaling and deployment slots are only available on Standard tier and above."

</details>

---

## 🚀 🔹 Q11. How do you deploy a Python app to Azure App Service?

<details>

Deploying a Python application to Azure App Service is straightforward.

### Steps
1. Create Flask or FastAPI application
2. Add dependencies in `requirements.txt`
3. Configure startup command
4. Create Azure Web App resource
5. Deploy using CLI or CI/CD

### Example Startup Command
```bash
gunicorn app:app
```

### Deployment Options
- Azure CLI (`az webapp up`)
- GitHub Actions
- Azure DevOps CI/CD

### Interview Answer

"First I'd create a Flask or FastAPI application, make sure the dependencies are listed in `requirements.txt`, and configure the startup command — for Flask it's typically `gunicorn app:app`. Then in Azure, I'd create a Web App resource with the Python 3.11 runtime. For deployment, I prefer using the Azure CLI with `az webapp up` for quick iterations, or I'd set up a CI/CD pipeline through GitHub Actions for production scenarios, so every push to main automatically deploys. The app gets a default URL at `azurewebsites.net`."

</details>

---

## 📈 🔹 Q12. Scale Up vs Scale Out

<details>

Scaling helps applications handle increased traffic and performance demands.

### Scale Up (Vertical Scaling)
Increase resources of a single instance.

Example:
```text
2 CPU → 8 CPU
8 GB RAM → 32 GB RAM
```

Pros:
- Simple
- Fast upgrade

Cons:
- Single point of failure
- Limited scaling ceiling

### Scale Out (Horizontal Scaling)
Add multiple instances of the application.

Pros:
- High availability
- Better fault tolerance
- Supports autoscaling
- Production-friendly

### Preferred Approach
Scale Out is generally preferred in production because one failed instance does not impact total service availability.

### Interview Answer

"Scale Up, or vertical scaling, means increasing the resources of a single instance — moving from a 2-core machine to an 8-core machine. Scale Out, or horizontal scaling, means adding more instances of the same size. In production, I always prefer Scale Out because it provides high availability — if one instance crashes, the others keep serving traffic. Scale Up has a ceiling, and it introduces a single point of failure. Azure's autoscale feature works on Scale Out and can automatically add or remove instances based on CPU usage, memory, or request queue length — which makes the system self-healing under load."

</details>

---

## 🔄 🔹 Q13. What are Deployment Slots?

<details>

Deployment Slots are separate live environments within the same Azure App Service.

### Common Slots
- Production
- Staging
- QA
- Testing

### Benefits
- Zero downtime deployment
- Safe pre-production testing
- Instant rollback
- Blue-Green deployment support

### How It Works
Deploy code to staging → test → swap with production.

### Interview Answer

"Deployment Slots are live environments within a single App Service — like staging, QA, and production. Each slot has its own hostname and can run a different version of the app. The key value is zero-downtime deployment: you deploy your new code to the staging slot, run smoke tests, and when you're confident, you do a slot swap which is instantaneous from the user's perspective. It also gives you a built-in rollback mechanism — if something goes wrong in production, you just swap back. This is essentially Azure's implementation of the Blue-Green deployment pattern."

</details>

---

## 🔐 🔹 Q14. Why should you never store secrets in your source code?

<details>

Storing secrets directly in source code is a serious security risk.

### Risks
- Git history exposure
- Repository leaks
- Log exposure
- Unauthorized access

### Better Approaches
- Environment Variables
- Azure App Settings
- Azure Key Vault
- Managed Identity

### Best Practice
Use Azure Key Vault with Managed Identity for secure passwordless access.

### Interview Answer

"Storing secrets in source code is a critical security anti-pattern. Once code is pushed to a repository — even a private one — the secret can be exposed through git history, logs, or accidental access grants. The minimum safe approach is using environment variables, which in App Service are configured under Application Settings and injected at runtime. The gold standard though is Azure Key Vault with Managed Identity — the app has no password to store at all, it authenticates to Key Vault via its Azure identity and retrieves secrets at runtime. No credentials in code, no credentials to rotate, no credentials to accidentally leak."

</details>

---

## 📊 🔹 Q15. How does autoscaling work in Azure App Service?

<details>

Autoscaling automatically adjusts the number of running instances based on load.

### Common Metrics
- CPU usage
- Memory usage
- HTTP queue length
- Request volume

### Example Rule
```text
CPU > 70% for 5 mins → Add instance
CPU < 30% for 10 mins → Remove instance
```

### Important Note
Autoscaling is available only on Standard App Service Plan and above.

### Interview Answer

"Autoscaling in App Service lets you define rules that trigger instance count changes based on metrics like CPU percentage, memory usage, or HTTP queue length. For example, a typical rule would be: if average CPU exceeds 70% for 5 minutes, add one instance; if it drops below 30% for 10 minutes, remove one. You define a minimum and maximum instance count — the minimum ensures availability during off-peak hours, and the maximum is critical to cap your costs. One important thing — autoscale is only available on Standard plan and above, not on Free or Basic tiers."

</details>

---

## 🆔 🔹 Q16. What is Managed Identity?

<details>

Managed Identity provides passwordless authentication between Azure services.

### Benefits
- No credentials stored in code
- No secret rotation
- Improved security
- Native Azure integration

### Common Use Cases
Access:
- Azure Key Vault
- Azure Storage
- SQL Database
- Service Bus

### Python Example
```python
from azure.identity import DefaultAzureCredential
```

### Interview Answer

"Managed Identity is a feature where Azure automatically assigns an identity to your App Service instance, registered in Azure Active Directory. This identity can be granted permissions to other Azure resources — like Key Vault, Storage, or SQL — without any passwords or connection strings. It's passwordless authentication at the infrastructure level. The major advantages are: there are no credentials to store, no credentials to rotate on a schedule, and no risk of secrets leaking through logs or git history. In Python, `DefaultAzureCredential` from the `azure-identity` SDK automatically picks up the Managed Identity in production — you write the same code locally and in Azure."

</details>

---

## ❤️ 🔹 Q17. What is Health Check in App Service?

<details>

Health Check monitors the health of your application instances.

### How It Works
Azure periodically checks a configured endpoint like:

```text
/health
```

If unhealthy:
- Instance removed from load balancer
- Traffic shifted to healthy instances
- Automatic recovery

### Benefits
- Self-healing applications
- Better uptime
- Faster recovery

### Interview Answer

"App Service Health Check is a feature where Azure periodically pings a specified endpoint on your app — typically something like `/health`. If an instance fails to respond with a 200 status for a configured number of times, Azure automatically removes it from the load balancer rotation and replaces it. On the application side, in Flask you'd expose a lightweight endpoint that checks critical dependencies — like a database ping — and returns 200 if everything is healthy or 503 if not. This makes the system self-healing without any manual intervention, which is essential for production-grade availability."

</details>

---

## 🛠️ 🔹 Q18. Production App Running Slow — How to Debug?

<details>

Follow a systematic debugging approach.

### Debug Steps
1. Check Azure Monitor
2. Review Log Stream
3. Analyze Application Insights
4. Check CPU / Memory metrics
5. Identify dependency bottlenecks
6. Consider scaling
7. Optimize static content using CDN

### Tools
- Azure Monitor
- Application Insights
- App Service Diagnostics
- Azure CDN

### Interview Answer

"I'd approach it systematically. First, I'd check Azure Monitor and the Log Stream for any exceptions or errors — this rules out application crashes. Second, I'd look at Application Insights, which gives me request duration breakdown, slow dependency calls, and exception traces. That usually pinpoints whether the bottleneck is in my code, a database query, or an external API. Third, I'd check resource metrics — if CPU or memory is consistently high, it might be a scaling issue and I'd consider increasing instances or upgrading the plan. Finally, for static assets, adding Azure CDN offloads a significant amount of traffic from the app server and improves perceived performance globally."

</details>

---

## 🚦 🔹 Q19. Blue-Green vs Canary Deployment

<details>

Both are zero-downtime deployment strategies.

### Blue-Green Deployment
- Two identical environments
- Blue = current live
- Green = new version
- Instant full traffic switch

Pros:
- Fast rollback
- Simple implementation

Cons:
- Higher risk during switch

---

### Canary Deployment
Gradual traffic shift.

Example:
```text
90% → Old Version
10% → New Version
```

Pros:
- Lower deployment risk
- Controlled rollout
- Better monitoring opportunity

Cons:
- More operational complexity

### Azure Implementation
- Deployment Slots
- Traffic Splitting

### Interview Answer

"Both are zero-downtime deployment strategies but they differ in risk exposure. In Blue-Green deployment, you run two identical environments — blue is live, green gets the new version. Once green is validated, you switch all traffic over instantly. It's clean and simple but the switch is all-or-nothing. Canary deployment is more gradual — you route a small percentage of traffic, say 5–10%, to the new version while the rest still hits the old version. You monitor error rates and latency, and only roll it out to 100% if metrics look healthy. Canary is safer for high-risk changes because the blast radius is limited. In Azure App Service, both can be implemented using Deployment Slots — Canary specifically uses the Traffic Splitting feature where you assign a percentage of traffic to each slot."

</details>

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

### Using Azure CLI
```bash
az ad user list \
--output table \
--query "[].{Name:displayName, Email:mail, UPN:userPrincipalName}"

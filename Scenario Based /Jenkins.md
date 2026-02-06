### Question 1. How Do You Manage Pipelines for Multiple Microservices with Interdependencies?

<details>
 
When multiple microservices depend on each other, the goal is to **build, test, and deploy them independently**, while still **handling shared dependencies safely**.

---

## 1. Separate Pipeline per Microservice (Recommended)

Each microservice has:
- Its own **Git repo**
- Its own **Jenkins pipeline**
- Its own **CI/CD lifecycle**

### ✅ Benefits:
- Independent builds
- Faster deployments
- Failures don't block other services

---

## 2. Use Shared Libraries for Common Pipeline Logic

Create a **Jenkins Shared Library**

Reuse common steps like:
- Build
- Test
- Docker image creation
- Security scans

👉 This avoids code duplication across pipelines.

### Example:
```groovy
@Library('ci-shared-lib') _
buildAndTest()
dockerBuildAndPush()
```

---

## 3. Versioned Artifacts to Manage Dependencies

- Each microservice produces a **versioned artifact**
  - Docker image tag
  - JAR version
- Other services depend on **released versions**, not source code

👉 **Never depend on "latest" in production.**

---

## 4. Dependency-Aware Pipeline Triggering

Use **upstream/downstream triggers** or events:

If **Service A** is a dependency of **Service B**:
1. Deploy A first
2. Then trigger B pipeline

### Options:
- Jenkins build job: step
- Webhooks
- Event-based triggers

---

## 5. Contract Testing Between Services

To avoid breaking dependent services:
- Use **contract testing** (e.g., Pact)
- Validate APIs between services

👉 Ensures changes in one service don't break others.

---

## 6. Environment-Based Deployment Strategy

Deploy in stages:

**Dev** → **QA** → **Staging** → **Prod**

- Validate dependencies at each environment

### Use:
- Feature flags
- Canary or blue-green deployments

---

## 7. Infrastructure & Config as Code

Use:
- **Helm charts**
- **Kubernetes manifests**
- **Terraform**

Keep configs **version-controlled**

👉 Makes dependency changes predictable.

---

## 8. Centralized Monitoring and Rollback

### Monitor:
- Pipeline failures
- Deployment health

### Enable:
- Fast rollback if dependent service fails

---

</details>

### Question 2. The CI pipeline just failed with the error: "No space left on device". Everything was running smoothly yesterday. What's the first thing we should look at?
<details>

---

## ✅ The Answer

### 🔍 Step 1: Identify the Root Cause

The error **"No space left on device"** usually means the build server has filled up with:

- 🐳 Old Docker images
- 📝 Build logs
- 📦 Leftover artifacts

**Why it happens:** Over time, these accumulate quietly until the disk is completely full.

---

### 🧹 Step 2: Immediate Fix - Clean Docker Resources

Start by cleaning up unused Docker resources.

**Command:**
```bash
docker system prune -af
```

**What it does:**
- ✅ Removes all unused images
- ✅ Removes stopped containers
- ✅ Removes unused cache layers

⚠️ **Warning:** Use carefully! This removes ALL unused Docker resources.

---

### 📂 Step 3: Clean CI Tool Artifacts

CI tools like **Jenkins** or **GitLab** often store:

- 📝 Large log files
- 📦 Old build artifacts

**Solution:**
- 🔄 Rotate logs regularly
- 📥 Archive old builds
- 🗑️ Automatically clean on a schedule

---

### 🚨 Step 4: Prevent Future Issues - Monitor & Alert

Set up **disk usage monitoring and alerts**:

**Best Practice:**
- 📊 Set threshold alert at **70-80%** disk usage
- 🔔 Get notified before disk fills up
- ⏰ Schedule automated cleanup jobs

**Benefits:**
- ✅ Saves hours of debugging
- ✅ Prevents sudden failures
- ✅ Keeps CI pipeline running smoothly

---


</details>

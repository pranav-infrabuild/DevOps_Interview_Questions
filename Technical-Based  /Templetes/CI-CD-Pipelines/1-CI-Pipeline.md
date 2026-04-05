## 🚀 Azure DevOps CI Pipeline (SonarQube + Docker + Trivy + ACR)

### 📄 `azure-pipelines.yml`

```yaml id="az_ci_pipeline"
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

variables:
  imageName: 'myapp'
  acrName: '<your-acr-name>'

stages:
- stage: Build_and_Scan
  jobs:
  - job: CI
    steps:

    # 1. Checkout Code
    - checkout: self

    # 2. Use Python
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.11'

    # 3. Install Dependencies
    - script: pip install -r requirements.txt
      displayName: Install Dependencies

    # 4. Run Tests
    - script: pytest
      displayName: Run Tests

    # 5. SonarQube Prepare
    - task: SonarQubePrepare@5
      inputs:
        SonarQube: 'SonarQube-Service-Connection'
        scannerMode: 'CLI'
        configMode: 'manual'
        cliProjectKey: 'myapp'
        cliSources: '.'

    # 6. SonarQube Analyze
    - task: SonarQubeAnalyze@5

    # 7. SonarQube Publish
    - task: SonarQubePublish@5
      inputs:
        pollingTimeoutSec: '300'

    # 8. Build Docker Image
    - script: |
        docker build -t $(imageName):$(Build.BuildId) .
      displayName: Build Docker Image

    # 9. Trivy Scan
    - script: |
        docker run --rm \
        -v /var/run/docker.sock:/var/run/docker.sock \
        aquasec/trivy image $(imageName):$(Build.BuildId)
      displayName: Trivy Scan

    # 10. Login to ACR
    - task: AzureCLI@2
      inputs:
        azureSubscription: 'Azure-Service-Connection'
        scriptType: bash
        scriptLocation: inlineScript
        inlineScript: |
          az acr login --name $(acrName)

    # 11. Tag Image
    - script: |
        docker tag $(imageName):$(Build.BuildId) $(acrName).azurecr.io/$(imageName):$(Build.BuildId)
      displayName: Tag Image

    # 12. Push Image to ACR
    - script: |
        docker push $(acrName).azurecr.io/$(imageName):$(Build.BuildId)
      displayName: Push to ACR
```

---

# 🎤 How to Explain This in Interview (Strong Answer)

> “This Azure DevOps pipeline is triggered on every commit to the main branch. It starts by setting up Python, installing dependencies, and running tests to ensure code quality.
> Then it integrates SonarQube for static code analysis to detect bugs and vulnerabilities.

After that, it builds a Docker image and performs a security scan using Trivy.
If the image passes the scan, the pipeline logs into Azure Container Registry using a service connection, tags the image with the build ID for traceability, and pushes it to ACR.”

---
---

## 🔄 Flow in Simple Words

```text
Code → Install → Test → SonarQube → Docker Build → Trivy Scan → Push to ACR
```

---

## 🧱 Step-by-Step (Super Simple)

### 1. Trigger

```yaml
trigger:
- main
```

👉 Pipeline runs automatically when code is pushed to `main` branch

---

### 2. Agent (Machine)

```yaml
pool:
  vmImage: 'ubuntu-latest'
```

👉 Uses a Linux machine to run all steps

---

### 3. Variables

```yaml
variables:
  imageName: 'myapp'
  acrName: '<your-acr-name>'
```

👉 Stores reusable values like:

* Docker image name
* Azure Container Registry name

---

## ⚙️ Main Steps

---

### 4. Checkout Code

```yaml
- checkout: self
```

👉 Downloads your code from repo

---

### 5. Setup Python

```yaml
UsePythonVersion@0
```

👉 Installs Python 3.11

---

### 6. Install Dependencies

```yaml
pip install -r requirements.txt
```

👉 Installs required libraries

---

### 7. Run Tests

```yaml
pytest
```

👉 Checks if code is working correctly

---

### 8. SonarQube (Code Quality)

```yaml
SonarQubePrepare → Analyze → Publish
```

👉 Scans code for:

* Bugs
* Security issues
* Code quality

---

### 9. Build Docker Image

```yaml
docker build
```

👉 Converts app into a container image

---

### 10. Trivy Scan (Security)

```yaml
trivy image
```

👉 Scans Docker image for vulnerabilities
👉 Ensures it's safe to deploy

---

### 11. Login to Azure

```yaml
az acr login
```

👉 Connects to Azure Container Registry (ACR)

---

### 12. Tag Image

```yaml
docker tag
```

👉 Prepares image with ACR format

---

### 13. Push to ACR

```yaml
docker push
```

👉 Uploads image to ACR (private registry)

---

# 🎯 Final Simple Summary

> “This pipeline automatically takes my code, tests it, checks quality using SonarQube, builds a Docker image, scans it for security using Trivy, and finally pushes it to Azure Container Registry.”

---

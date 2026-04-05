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


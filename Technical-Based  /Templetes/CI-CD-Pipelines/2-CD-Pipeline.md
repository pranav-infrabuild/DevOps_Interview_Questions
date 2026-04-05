##  Continuous Deployment (CD)

* **Purpose:** Deploy Docker image to Kubernetes using manifests
* **Steps in CD:**

```yaml
# cd-pipeline.yml
stages:
- stage: Deploy
  jobs:
  - job: DeployToK8s
    steps:

    # Kubernetes service connection
    - task: Kubernetes@1
      inputs:
        connectionType: 'Kubernetes Service Connection'
        kubernetesServiceEndpoint: 'aks-connection'
        namespace: 'default'
        command: 'apply'
        arguments: '-f deployment.yaml'

    # Optionally update image dynamically
    - script: |
        kubectl set image deployment/my-app \
        my-app=$(acrName).azurecr.io/myapp:$(Build.BuildId)
      displayName: Update Deployment Image
```

---

> “In CI, we checkout code, install dependencies, run tests, check code quality with SonarQube, build a Docker image, and scan it for vulnerabilities using Trivy. The image is then pushed to Azure Container Registry.

> In CD, the pipeline applies Kubernetes manifests to the cluster. The manifest defines the desired state, and we update the container image dynamically. Kubernetes performs rolling updates to deploy the new version with zero downtime.”



* `dependsOn: CI` → **CD runs automatically** after CI completes successfully.

---

> “In my project, CD can be triggered **manually** for production or **automatically** for development/staging. By integrating it with CI, the pipeline can automatically deploy the latest Docker image using Kubernetes manifests after a successful build, enabling zero-downtime rolling updates.”

---


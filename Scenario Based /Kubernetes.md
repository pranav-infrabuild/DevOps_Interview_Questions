## Kubernates Common Erros :-

### 1. `CrashLoopBackOff`

<details>

The `CrashLoopBackOff` status in Kubernetes indicates that a container in a Pod is repeatedly failing and being restarted by the kubelet. Here's a breakdown of the issue:

### What is `CrashLoopBackOff`?

- **Definition**: `CrashLoopBackOff` is a status indicating that a container in a Pod has failed to start correctly and is continuously crashing and being restarted by Kubernetes.
- **Behavior**: When a container crashes, Kubernetes will try to restart it. If the container fails repeatedly, Kubernetes applies an increasing backoff interval between restart attempts to avoid overwhelming the system.

### Common Causes

1. **Application Errors**: The application inside the container might be failing due to bugs, misconfigurations, or other runtime issues.
2. **Configuration Issues**: Incorrect configuration files, environment variables, or secrets might be causing the container to fail.
3. **Resource Limits**: The container might be exceeding resource limits like CPU or memory, leading to crashes.
4. **Dependency Failures**: The container might be dependent on other services or containers that are not available or functioning correctly.
5. **Incorrect Start Command**: The entry point or command defined for the container might be incorrect or failing.

### Troubleshooting Steps

1. **Check Logs**: Use `kubectl logs <pod-name> -c <container-name>` to view the logs and identify the cause of the crash.
   
   **Example**:
   ```bash
   kubectl logs my-pod -c my-container
   ```

2. **Describe Pod**: Use `kubectl describe pod <pod-name>` to get detailed information about the Pod and look for events or error messages.

   **Example**:
   ```bash
   kubectl describe pod my-pod
   ```

3. **Verify Configuration**: Ensure that all configuration files, environment variables, and secrets are correctly set up.

4. **Check Resource Limits**: Ensure that the resource limits and requests for the container are set appropriately and that the container is not being killed due to exceeding these limits.

5. **Inspect Docker Image**: Make sure the Docker image used for the container is built correctly and that it can run without issues in isolation.

6. **Adjust Restart Policy**: Review and adjust the restart policy or backoff settings if necessary, although this is usually a temporary solution while resolving the root cause.


</details>

### 2. `Failed to pull image" or "ImagePullBackOff`

<details>

When Kubernetes is unable to pull a container image, it typically results in an error and the Pod will not start properly. Here are common reasons for this issue and steps to troubleshoot:

### Common Reasons for Image Pull Issues

1. **Incorrect Image Name or Tag**: The image name or tag specified in your Pod or Deployment configuration may be incorrect or misspelled.
2. **Image Not Found**: The specified image may not exist in the container registry.
3. **Authentication Issues**: Kubernetes may not have the required credentials to access a private container registry.
4. **Registry Connectivity Issues**: There may be network issues preventing Kubernetes from reaching the container registry.
5. **Rate Limiting**: Some container registries impose rate limits on image pulls, which may cause failures if exceeded.

### Troubleshooting Steps

1. **Check Pod Events**: Use `kubectl describe pod <pod-name>` to check the events and error messages related to the image pull.

   **Example**:
   ```bash
   kubectl describe pod my-pod
   ```

   Look for messages like "Failed to pull image" or "ImagePullBackOff."

2. **Verify Image Name and Tag**: Ensure that the image name and tag are correctly specified in your Pod or Deployment YAML file. The format should be `repository/image:tag`.

   **Example**:
   ```yaml
   spec:
     containers:
     - name: my-container
       image: my-repo/my-image:latest
   ```

3. **Check Image Availability**: Verify that the image exists in the container registry and is accessible. You can try pulling the image manually using Docker:

   **Example**:
   ```bash
   docker pull my-repo/my-image:latest
   ```

4. **Verify Registry Credentials**:
   - If using a private registry, ensure that you have created a Kubernetes Secret with the registry credentials and configured it in your Pod or Deployment.

   **Example**:
   ```bash
   kubectl create secret docker-registry my-registry-secret \
     --docker-server=<registry-url> \
     --docker-username=<username> \
     --docker-password=<password> \
     --docker-email=<email>
   ```

   Then, reference the Secret in your Pod or Deployment YAML:

   **Example**:
   ```yaml
   spec:
     containers:
     - name: my-container
       image: my-repo/my-image:latest
     imagePullSecrets:
     - name: my-registry-secret
   ```

5. **Check Network Connectivity**: Ensure that your Kubernetes nodes have network access to the container registry. This might involve checking firewall rules, network policies, or DNS settings.

6. **Inspect Registry Limits**: If you suspect rate limiting, check the registry documentation or contact support to understand any rate limits that might be affecting your pulls.

By following these steps, you can identify and resolve issues related to pulling container images in Kubernetes.

</details>

### 3. `pod is in a `Pending` state`
<details>


When a pod is in a `Pending` state, it means that the pod has been accepted by the Kubernetes system but it is not yet running on any node in the cluster. 

### 1. **Insufficient Resources:**
   - **Issue:** The nodes in your cluster may not have enough CPU, memory, or other resources to schedule the pod.
   - **Solution:** 
     - Check the available resources in your nodes using `kubectl describe nodes`.
     - If resources are insufficient, consider scaling up your cluster by adding more nodes or resizing the existing ones.



You can gather more information about why the pod is in a `Pending` state by running the following command:
```bash
kubectl describe pod <pod-name>
```
This will provide detailed information, including events that might explain why the pod is not being scheduled.
</details>

### 4. `Pod spec contains invalid configurations`
<details>

An invalid pod specification error typically occurs when there are issues in the YAML configuration of the pod. This might include syntax errors, missing required fields, or incorrect values.

### Troubleshooting Steps

1. **Review YAML Syntax:**
   - **Issue:** YAML is sensitive to indentation and formatting. Even a small mistake can cause errors.
   - **Solution:** 
     - Use a YAML validator or linting tool to check the syntax.
     - Ensure correct indentation, spacing, and structure.

2. **Check Required Fields:**
   - **Issue:** Missing required fields like `apiVersion`, `kind`, `metadata`, or `spec` can lead to errors.
   - **Solution:**
     - Ensure that the YAML file includes all the necessary fields. A basic pod spec should include:
       ```yaml
       apiVersion: v1
       kind: Pod
       metadata:
         name: my-pod
       spec:
         containers:
         - name: my-container
           image: nginx
       ```

3. **Verify Field Values:**
   - **Issue:** Incorrect values for fields (e.g., incorrect image name, invalid port number) can cause the pod to fail.
   - **Solution:**
     - Double-check all field values to ensure they are correct. For example, make sure the container image exists and the port numbers are valid.

4. **Use `kubectl apply --dry-run`:**
   - **Issue:** Applying the configuration without checking for errors can cause issues in the cluster.
   - **Solution:**
     - Use the `--dry-run=client` option to validate the pod spec without actually creating the pod:
       ```bash
       kubectl apply -f <pod-spec.yaml> --dry-run=client
       ```
     - This command will check for any errors in the YAML file and report them without applying the changes.

5. **Check for Typo in Fields:**
   - **Issue:** A typo in field names (e.g., `containters` instead of `containers`) can invalidate the specification.
   - **Solution:**
     - Carefully review the field names in the YAML file.


</details>

### 5. `Service Unavailable`
<details>

If you're encountering a "Service Unavailable" error in Kubernetes, it typically indicates that the service is not reachable or the backend Pods are not responding correctly. Here’s a step-by-step guide to troubleshoot this issue:

### 1. **Check Service Configuration**
   - Ensure that the service is correctly configured and running:
     ```bash
     kubectl get svc
     ```
   - Review the service configuration using:
     ```bash
     kubectl describe svc <service-name>
     ```
   - Ensure that the service type (ClusterIP, NodePort, LoadBalancer) is correctly set and the ports are properly configured.

### 2. **Verify Pods are Running**
   - Confirm that the backend Pods are running and ready to serve requests:
     ```bash
     kubectl get pods
     ```
   - Use `kubectl describe pod <pod-name>` to check the Pod's status, including readiness and liveness probes.

### 3. **Check Endpoint Readiness**
   - Ensure that the service has healthy endpoints:
     ```bash
     kubectl get endpoints <service-name>
     ```
   - Verify that the endpoints are correctly mapped to the Pods. If the endpoints list is empty, it indicates that no Pods match the service’s selector.

### 4. **Review Network Policies**
   - If network policies are implemented, make sure they are not blocking traffic between the service and the Pods:
     ```bash
     kubectl get networkpolicy
     ```
   - Review the policies using `kubectl describe networkpolicy <policy-name>` to ensure they allow traffic to the service.

### 5. **Check Service Connectivity**
   - Test connectivity to the service using `kubectl exec` to run commands inside a Pod:
     ```bash
     kubectl exec -it <pod-name> -- curl http://<service-name>:<port>
     ```
   - This command checks if the service is accessible from within the cluster.

### 6. **DNS Resolution**
   - Confirm that the service's DNS name is resolving correctly within the cluster:
     ```bash
     kubectl exec -it <pod-name> -- nslookup <service-name>
     ```
   - Ensure that the DNS setup is working properly if the service is not resolving.

### 7. **Review Service Logs**
   - Check the logs of the Pods backing the service to see if they are receiving requests and if any errors are occurring:
     ```bash
     kubectl logs <pod-name>
     ```

### 8. **Restart Pods or Service**
   - As a last resort, try restarting the Pods or the service to see if it resolves the issue:
     ```bash
     kubectl delete pod <pod-name>
     kubectl rollout restart deployment <deployment-name>
     ```

By following these steps, you should be able to diagnose and resolve the "Service Unavailable" issue in your Kubernetes cluster. If you need further assistance or have specific configurations you'd like to review, feel free to share them.

</details>

### Question 6. How would you define a Kubernetes Pod YAML to run a Spring Boot application as a single container exposing port 8080?

<details>

So Kubernetes needs to know:

1. **What kind of object** → Pod
2. **Which image to run** → Docker image
3. **Which port the app listens on** → 8080


---

## Correct Pod YAML (Final Answer)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: springboot-app
spec:
  containers:
    - name: springboot-app-container
      image: myrepo/springboot-app:1.0
      ports:
        - containerPort: 8080
```

---

## Explanation in Very Simple Language

### 1️⃣ `apiVersion: v1`

- Tells Kubernetes which API version to use
- Pods use `v1`

### 2️⃣ `kind: Pod`

- Tells Kubernetes what you are creating
- Here → a Pod

### 3️⃣ `metadata`

```yaml
metadata:
  name: springboot-app
```

- Name of the Pod
- Used to identify it in Kubernetes

### 4️⃣ `spec`

- Defines how the Pod should run

### 5️⃣ `containers`

```yaml
containers:
  - name: springboot-app-container
```

- A Pod can have multiple containers
- Here we have one container
- `-` means list item

### 6️⃣ `image`

```yaml
image: myrepo/springboot-app:1.0
```

- Docker image that contains your Spring Boot app

### 7️⃣ `ports`

```yaml
ports:
  - containerPort: 8080
```

- Tells Kubernetes: 👉 "This container listens on port 8080"
- Spring Boot runs on 8080 by default

⚠️ **Note:**
This does NOT expose the app outside. For external access, you need:
- Service (ClusterIP / NodePort / LoadBalancer)
- or Ingress

---


   
</details>


### Question 7: How do you inject environment variables from a ConfigMap into a Node.js Pod in Kubernetes?

<details>
---

## ✅ Correct Pod YAML (Final Answer)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: node-app-pod
  labels:
    app: node-app
spec:
  containers:
    - name: app
      image: node:18
      envFrom:
        - configMapRef:
            name: node-env-config
```

---

## Very Simple Explanation (Real-time Scenario)

### Scenario

You have:
- A Node.js app
- It needs environment variables
- Variables are already stored in a ConfigMap
- ConfigMap name → `node-env-config`
- You want Kubernetes to inject those variables into the container

---

## Key Part (Most Important)

```yaml
envFrom:
  - configMapRef:
      name: node-env-config
```

👉 **This tells Kubernetes:**

> "Take all key-value pairs from the ConfigMap `node-env-config` and load them as environment variables inside my container."

### Example ConfigMap:

```yaml
NODE_ENV=production
DB_HOST=localhost
```

### Inside the Node.js App:

```javascript
process.env.NODE_ENV
process.env.DB_HOST
```

---

## Interview-friendly Explanation (1–2 Lines)

> "I use `envFrom` with `configMapRef` in the container spec to inject all environment variables from a ConfigMap into my Node.js Pod."

---
</details>


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

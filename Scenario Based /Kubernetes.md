# Kubernetes Common errors

#### 1)ImageBackPullOff:

We face this issue when image is not present in registry or the given image tag is wrong.This error occurs when Kubernetes repeatedly tries and fails to pull the container image specified in the pod definition

**Steps to troubleshoot this issue:**

1.Ensure that the container image specified in your pod definition is available and accessible

2.Ensure that the Kubernetes nodes have proper network connectivity to reach the image registry.

3.Review the imagePullPolicy specified in your pod definition.

4.Use kubectl describe pod <pod_name> to get detailed information about the pod and any associated events.

5.Review logs from the image registry (e.g., Docker Hub, Amazon ECR, Google Container Registry) for any errors or issues related to image pulling.

6.Insufficient resources on the nodes can cause timeouts or failures during image pulling.

7.Attempt to manually pull the container image on one of the Kubernetes nodes

docker pull <image_name>:<tag>

8.Once the pod is scheduled, review the container logs to diagnose any issues within the container.

kubectl logs <pod_name> -c <container_name>


#### 2) CrashLoopBackOff:

The "CrashLoopBackOff" error in Kubernetes occurs when a pod crashes repeatedly shortly after 
starting, causing Kubernetes to continuously restart the pod, which ultimately results in a 
back-off delay between restart attempts. This error is often indicative of an issue within the 
containerized application running in the pod.

**Steps to troubleshoot this issue**:

1.Use kubectl get pods to list the pods in your namespace and identify pods in a "CrashLoopBackOff" state:

kubectl get pods

Describe the problematic pod to view detailed events and status:

kubectl describe pod <pod_name>

Look for events related to pod initialization, container startup, or application errors.

2.Retrieve logs from the container within the problematic pod to understand why it is crashing

kubectl logs <pod_name> -c <container_name>

3.Ensure that the pod has adequate resources (CPU, memory) allocated to run the containerized application

4.Check the readiness and liveness probes defined for the container in the pod spec.

5.Ensure that all necessary dependencies (e.g., database connections, external services) required by the application are accessible and properly configured

6.Validate the container's configuration and startup commands

#### 3) OOM Killed -out of memory

we face this issue when pod tries to utilise more memory than the limits we have set.

we can resolve it by setting appropriate resource request and resource limit.

#### 4) POD Status- Pending

The "Pending" status for a pod in Kubernetes indicates that the pod is not scheduled onto a node and is waiting to be assigned resources to run

**Steps to troubleshoot this issue**:

1.Ensure that your Kubernetes cluster has available nodes where the pod can be scheduled

kubectl get nodes


2. Review the resource requests and limits specified in the pod's YAML definition.

![image](https://github.com/pranav278/DevOps_Senariao_Based_Questions/assets/157089767/21a6f7af-939e-4d81-bf25-98ecb9d908a3)

3.Verify if the pod specifies any node selector or affinity rules that restrict where it can be scheduled.

![image](https://github.com/pranav278/DevOps_Senariao_Based_Questions/assets/157089767/960b44c7-ad93-4474-b3b4-421f5ed86f37)

4.Check if there are any node taints that prevent the pod from scheduling.

kubectl describe node <node_name>

#### 5) POD Status- Waiting


The "Waiting" status for a pod in Kubernetes indicates that the pod is not able to progress to a running state due to specific conditions or issues


**Steps to troubleshoot this issue**:

1.Check Pod Events.

2.Inspect Container State

3. Review Container Logs

4.Check Resource Requests and Limits

5.Verify Image Pulling

6. Look for Dependencies

kubectl get configmaps
kubectl get secrets
kubectl get pvc


7.Monitor Pod Status Changes

#### 6) POD Status- Evicted

We can resolve it by setting appropriate resource requests and resource limits for the PODs and having

enough resources in worker nodes.

#### 7) POD will be up and running application is not accessible:


If your Kubernetes pod is up and running, but the application within the pod is not accessible, it typically indicates an issue with the application configuration, networking, or service definition.

**Steps to troubleshoot this issue**:

1.Ensure that the pod is in the Running state.

2.Retrieve logs from the container to check for any application errors or startup issues.

3.Confirm that the pod has an assigned IP address.

kubectl describe pod <pod_name>

4.Check the service definition associated with your application

kubectl get svc
kubectl describe svc <service_name>


5.Verify that the service is exposing the correct port that the application is listening on

6.Use curl or kubectl port-forward to test access to the application within the pod

kubectl port-forward <pod_name> 8080:80
curl http://localhost:8080


# Cluster - Level issues in Kubernetes:

#### 1) Nodes not ready

The "Nodes not ready" error in Kubernetes indicates that one or more nodes in your cluster are not in a healthy state and cannot accept workloads or schedule pods. This can happen due to various reasons, including networking issues, node failures, or resource constraints.

#### 2) Networking issue


"Networking issue"  in Kubernetes can manifest in various ways and are often related to problems with networking configuration, connectivity, or communication between components in the cluster. 

#### 3) etcd Cluster is unavailable


The "etcd cluster is unavailable" error in Kubernetes typically indicates a problem with the underlying etcd datastore, which is a critical component used for storing cluster state and configuration. 

#### 4) Persistent volume Claims Stuck in Pending State:

The "PersistentVolumeClaims (PVCs) stuck in Pending state" error in Kubernetes typically indicates that the system is unable to fulfill the PVC requests due to various reasons.


**Steps to troubleshoot this issue**:

1.Ensure that the required StorageClass exists and is accessible:

kubectl get storageclasses

2.Ensure the StorageClass has volumeBindingMode: Immediate for immediate binding.

3.List available PersistentVolumes (PVs) in the cluster:

kubectl get pv

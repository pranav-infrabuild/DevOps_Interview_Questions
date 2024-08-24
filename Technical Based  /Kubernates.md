## Kubernates Interview Questions 

### Question 1: What is Kubernetes?
<details>

- Kubernetes, also known as K8s
- Is an open-source platform designed to automate deploying, scaling, and operating application containers. 
- It allows you to manage containerized applications across a cluster of nodes, providing mechanisms for deployment, maintenance, and scaling of applications

</details>

### Question 2. What are the main components of Kubernetes architecture?
<details>
  
Answer: The main components of Kubernetes architecture include:

Master Node: This includes components like the API server, etcd (key- value store), controller manager, and scheduler.

Worker Nodes: These include the kubelet (agent running on each node), kube-proxy (networking component), and container runtime (e.g., Docker).

Pods: The smallest deployable units that can contain one or more containers.

Services: logical set of Pods and a policy by which to access them. . Services enable communication between different components within a Kubernetes cluster, allowing Pods. even if the pod scale up or down.

Namespaces: Namespaces allow for the isolation of resources within a Kubernetes cluster. Resources like Pods, Services, ConfigMaps, and Secrets are scoped to a particular namespace. Provide a way to divide cluster resources between multiple users.

</details>

### Question 3: What is a Pod in Kubernetes?
<details>

- A Pod is the smallest deployable unit in Kubernetes and represents a single instance of a running process in a cluster. A Pod can contain one or more containers that share the same network namespace and storage volumes.

</details>

### Question 4: How does Kubernetes handle load balancing?
<details>

Kubernetes handles load balancing primarily through **Services**, which distribute network traffic across a set of Pods to ensure that no single Pod is overwhelmed. 

### 1. **Service-Based Load Balancing**
   - **ClusterIP (default)**: 
     - The default type of Service, `ClusterIP`, creates an internal IP address for a set of Pods within the cluster. This type of Service is only accessible within the cluster and is typically used for internal communication between services.

   - **NodePort**: 
     - If you have a web application running on a Kubernetes cluster, that makes an application accessible from outside the cluster by opening a specific port on all the nodes

   - **LoadBalancer**: 
     - The `LoadBalancer` type of Service integrates that routes traffic to the Kubernetes Service. The external load balancer directs traffic to the appropriate node and then distributes it across the Pods in the Service. 

   - **ExternalName**: 
     - it maps a Service to an external DNS name
</details>

### Question 5: What is a ReplicaSet in Kubernetes?
<details>

- A ReplicaSet ensures that a specified number of pod replicas arerunning at any given time. It can be used to scale pods up or down, replace failed pods, and ensure the desired state of the application is maintained. A ReplicaSet is defined using a YAML or JSON file, specifying the desired number of replicas and the template for the pods.

</details>

### Question 6: What is a Deployment in Kubernetes?
<details>
  
A Deployment provides declarative updates to applications and ensures that the desired number of pod replicas are running. It allows for rolling updates, rollbacks, and scaling of applications. Deployments use a Pod template to create Pods and manage the lifecycle of these Pods through ReplicaSets.
</details>

### Question 7 :Explain the difference between a ReplicaSet and a ReplicationController.
<details>

- **ReplicationController** is an older method for ensuring a specified number of Pods are running, limited to equality-based selectors.
- **ReplicaSet** is a more modern and flexible method, supporting both equality-based and set-based selectors, and is preferred for most use cases.
Supports both **equality-based** and **set-based** selectors. Set-based selectors allow for more flexible selection criteria, such as selecting Pods with labels that are in a specified set or not in a set. For example, you could use a selector like `env in (production, staging)` to select Pods with the `env` label set to either `production` or `staging`.

</details



### Question 8: What is a StatefulSet in Kubernetes?
<details>

A **StatefulSet** in Kubernetes is a resource designed for managing stateful applications that require persistent storage and stable network identities. 

### Key Features of StatefulSet:

1. **Stable Network Identities**:
   - Each Pod in a StatefulSet has a stable, unique network identity. Pods are assigned a unique name that includes an ordinal index (e.g., `myapp-0`, `myapp-1`, etc.), which is maintained across Pod restarts. 

2. **Stable Persistent Storage**:
   - StatefulSets can be configured with PersistentVolumeClaims (PVCs) that provide stable, persistent storage. Each Pod gets its own PVC that is not shared with other Pods. 

3. **Ordered Deployment and Scaling**:
   - StatefulSets deploy and scale Pods in a specific, sequential order.

4. **Graceful Shutdown**:
   - StatefulSets ensure that Pods are shut down gracefully and in the reverse order of their creation. 

</details>


### Question 9. What are ConfigMaps in Kubernetes?
<details>

**ConfigMaps** in Kubernetes are used to store non-confidential configuration data in key-value pairs. They allow you to decouple configuration details from your application code, making your containerized applications more portable and easier to manage.

### Key Features of ConfigMaps:

1. **Storage of Configuration Data**:
   - ConfigMaps are designed to store configuration data that doesn't contain sensitive information. Examples include environment variables, configuration files, command-line arguments, or any other data your application needs to function properly.

2. **Decoupling Configuration from Code**:
   - By using ConfigMaps, you can separate your application's configuration from its container image. This means you can reuse the same container image across different environments (e.g., development, staging, production) while providing environment-specific configurations via ConfigMaps.


</details>

### Question 10. What are Secrets in Kubernetes?
<details>
  
- Secrets are used to store sensitive data, such as passwords, OAuth tokens, and SSH keys. They are similar to Config Maps but provide additional functionalities to ensure the data is handled securely. Secrets can be encrypted at rest and are only accessible to Pods that have been explicitly granted access.

</details>


### Question 11. How does Kubernetes handle storage
<details>

Kubernetes manages storage through several key abstractions and components that allow Pods to manage, and use storage resources effectively. 

### 1. **Volumes**

- **Volumes**: Volumes are storage resources that can be attached to Pods. They exist as long as the Pod exists, and can be shared between containers in the same Pod. 

Kubernetes supports various types of volumes:
  - **emptyDir**: Provides a temporary storage volume that is created when a Pod is assigned to a Node and is deleted when the Pod is removed. Useful for scratch space.
  - **hostPath**: Mounts a file or directory from the host Node’s filesystem into a Pod. Typically used for testing or development.
  - **nfs**: Mounts a Network File System (NFS) share into the Pod, allowing Pods to share storage across Nodes.
  - **configMap**: Provides configuration data to Pods in the form of files or environment variables.
  - **secret**: Provides sensitive data to Pods in a secure way, often used for credentials or keys.

### 2. **Persistent Volumes (PV)**

- **Persistent Volumes (PV)**:  that have been provisioned by an administrator or dynamically created by the system. PVs can represent various types of storage, such as local disks, network storage (NFS, iSCSI), cloud provider storage (AWS EBS, GCE PD), and more.

### 3. **PersistentVolumeClaims (PVC)**

- **PersistentVolumeClaims (PVC)**: PVCs are requests for storage made by users. They specify the amount of storage required and can include other parameters such as access modes (e.g., ReadWriteOnce, ReadOnlyMany). When a PVC is created, Kubernetes will find an available PV that meets the criteria and bind them together.


</details>

### Question 12. What are DaemonSets in Kubernetes?
<details>
- DaemonSets ensure that a copy of a Pod runs on all (or some) nodes. in the cluster. They are typically used for background processes such as logging, monitoring, and other system-level services that need to run on every node.
</details>

### Question 13. What is etcd ?
<details>

- etcd stores the entire state of the Kubernetes cluster, including information about nodes, Pods, ConfigMaps, Secrets, services, and more. Any change in the cluster's state, such as the creation or deletion of resources, is recorded in etcd.
</details>

### Question 14. What is ingress ?
<details>

- Ingress is a Kubernetes resource that manages external access to services within a cluster. It acts as a single entry point for incoming traffic, routing it to the appropriate services based on the routing rules the developer sets. Kubernetes Ingress provides a way to securely and efficiently expose services
</details>

### Question 15. What is the difference between ingress and load balancer ?
<details>

- Ingress in Kubernetes that manages external access to services within the cluster, typically HTTP and HTTPS traffic. It provides rules that define how incoming requests should be routed to the appropriate services based on the URL path, host, or other criteria.
- A Load Balancer in Kubernetes is a type of service that automatically distributes incoming network traffic across multiple Pods

</details>

## Some Commands 
<details>
1. **`kubectl cluster-info`**: This command displays essential information about your Kubernetes cluster, such as the addresses of the Kubernetes master and any services that are running.

   **Usage**: 
   ```bash
   kubectl cluster-info
   ```

   **Example Output**:
   ```
   Kubernetes master is running at https://192.168.0.1:6443
   KubeDNS is running at https://192.168.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
   ```

2. **`kubectl get nodes`**: This command lists all the nodes in the Kubernetes cluster. It provides information such as the node name, status, roles, age, and version.

   **Usage**:
   ```bash
   kubectl get nodes
   ```

   **Example Output**:
   ```
   NAME              STATUS   ROLES    AGE   VERSION
   node1             Ready    master   1d    v1.18.0
   node2             Ready    <none>   1d    v1.18.0
   node3             Ready    <none>   1d    v1.18.0
   ```

These commands are useful for checking the health and status of your Kubernetes cluster and its nodes.
</details>

Here’s how the `kubectl` commands for managing namespaces work:

1. **`kubectl get namespaces`**: This command lists all the namespaces in your Kubernetes cluster. Namespaces are used to organize and isolate resources within the cluster.

   **Usage**:
   ```bash
   kubectl get namespaces
   ```

   **Example Output**:
   ```
   NAME              STATUS   AGE
   default           Active   5d
   kube-system       Active   5d
   kube-public       Active   5d
   custom-namespace  Active   2d
   ```

2. **`kubectl create namespace <name>`**: This command creates a new namespace in the cluster. Replace `<name>` with the desired name of the namespace.

   **Usage**:
   ```bash
   kubectl create namespace <name>
   ```

   **Example**:
   ```bash
   kubectl create namespace my-namespace
   ```
   This would create a namespace called `my-namespace`.

3. **`kubectl delete namespace <name>`**: This command deletes a specified namespace along with all the resources within it. Replace `<name>` with the name of the namespace you want to delete.

   **Usage**:
   ```bash
   kubectl delete namespace <name>
   ```

   **Example**:
   ```bash
   kubectl delete namespace my-namespace
   ```
   This would delete the `my-namespace` and all resources within it. 

Be careful when deleting namespaces, as it will remove all resources associated with that namespace.

Here's how you can manage Pods using `kubectl`:

1. **`kubectl get pods`**: This command lists all the Pods in the default namespace. It provides basic information about each Pod, such as its name, status, and age.

   **Usage**:
   ```bash
   kubectl get pods
   ```

   **Example Output**:
   ```
   NAME                      READY   STATUS    RESTARTS   AGE
   my-app-1                  1/1     Running   0          3d
   my-app-2                  1/1     Running   0          3d
   my-db-1                   1/1     Running   0          3d
   ```

2. **`kubectl get pods -n <namespace>`**: This command lists all the Pods in a specific namespace. Replace `<namespace>` with the name of the namespace you're interested in.

   **Usage**:
   ```bash
   kubectl get pods -n <namespace>
   ```

   **Example**:
   ```bash
   kubectl get pods -n my-namespace
   ```
   This would list all Pods in the `my-namespace` namespace.

3. **`kubectl describe pod <pod-name>`**: This command provides detailed information about a specific Pod, such as its labels, annotations, events, and the state of each container within the Pod. Replace `<pod-name>` with the name of the Pod you want to inspect.

   **Usage**:
   ```bash
   kubectl describe pod <pod-name>
   ```

   **Example**:
   ```bash
   kubectl describe pod my-app-1
   ```

4. **`kubectl delete pod <pod-name>`**: This command deletes a specific Pod from the cluster. Replace `<pod-name>` with the name of the Pod you want to remove.

   **Usage**:
   ```bash
   kubectl delete pod <pod-name>
   ```

   **Example**:
   ```bash
   kubectl delete pod my-app-1
   ```
   This would delete the Pod named `my-app-1`. Be cautious when deleting Pods, as they might be critical for your applications.

These commands are essential for managing Pods, which are the smallest and simplest Kubernetes objects, representing a single instance of a running process in your cluster.

Here’s how you can manage Kubernetes Deployments using `kubectl`:

1. **`kubectl get deployments`**: This command lists all the Deployments in the current namespace. A Deployment manages a set of identical Pods, ensuring the desired number of Pods are running and updating them as needed.

   **Usage**:
   ```bash
   kubectl get deployments
   ```

   **Example Output**:
   ```
   NAME               READY   UP-TO-DATE   AVAILABLE   AGE
   my-deployment      3/3     3            3           10d
   another-deployment 2/2     2            2           7d
   ```

2. **`kubectl describe deployment <deployment-name>`**: This command provides detailed information about a specific Deployment, including its status, strategies, replica sets, and any events related to the Deployment. Replace `<deployment-name>` with the name of the Deployment you want to inspect.

   **Usage**:
   ```bash
   kubectl describe deployment <deployment-name>
   ```

   **Example**:
   ```bash
   kubectl describe deployment my-deployment
   ```

3. **`kubectl apply -f <file.yaml>`**: This command applies the configuration specified in a YAML file to your cluster. It can be used to create or update resources like Deployments, Services, and more.

   **Usage**:
   ```bash
   kubectl apply -f <file.yaml>
   ```

   **Example**:
   ```bash
   kubectl apply -f deployment.yaml
   ```
   This would apply the configuration specified in `deployment.yaml` to your cluster.

4. **`kubectl delete deployment <deployment-name>`**: This command deletes a specific Deployment from the cluster, along with its associated Pods. Replace `<deployment-name>` with the name of the Deployment you want to delete.

   **Usage**:
   ```bash
   kubectl delete deployment <deployment-name>
   ```

   **Example**:
   ```bash
   kubectl delete deployment my-deployment
   ```
   This would delete the Deployment named `my-deployment`.

5. **`kubectl scale deployment <deployment-name> --replicas=<num>`**: This command scales a Deployment to a specific number of replicas, which means it adjusts the number of Pods that should be running. Replace `<deployment-name>` with the name of the Deployment and `<num>` with the desired number of replicas.

   **Usage**:
   ```bash
   kubectl scale deployment <deployment-name> --replicas=<num>
   ```

   **Example**:
   ```bash
   kubectl scale deployment my-deployment --replicas=5
   ```
   This would scale `my-deployment` to 5 replicas, ensuring that 5 Pods are running.

These commands are key for managing Deployments, which are used to ensure that a specified number of replicas of a Pod are running at any given time. They also help with updating your application in a controlled manner.

Here’s how you can manage Services in Kubernetes using `kubectl`:

1. **`kubectl get services`**: This command lists all the Services in the current namespace. A Service in Kubernetes is an abstraction that defines a logical set of Pods and a policy by which to access them, usually over a network.

   **Usage**:
   ```bash
   kubectl get services
   ```

   **Example Output**:
   ```
   NAME           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
   my-service     ClusterIP   10.0.0.1       <none>        80/TCP           5d
   another-svc    NodePort    10.0.0.2       <none>        80:30001/TCP     3d
   ```

2. **`kubectl describe service <service-name>`**: This command provides detailed information about a specific Service, including its endpoints, type, ports, and associated Pods. Replace `<service-name>` with the name of the Service you want to inspect.

   **Usage**:
   ```bash
   kubectl describe service <service-name>
   ```

   **Example**:
   ```bash
   kubectl describe service my-service
   ```

   **Output** (abridged):
   ```
   Name:              my-service
   Namespace:         default
   Labels:            app=my-app
   Selector:          app=my-app
   Type:              ClusterIP
   IP:                10.0.0.1
   Port:              <unset>  80/TCP
   TargetPort:        80/TCP
   Endpoints:         192.168.1.10:80
   ```

3. **`kubectl delete service <service-name>`**: This command deletes a specific Service from the cluster. Once deleted, the Service will no longer route traffic to the associated Pods. Replace `<service-name>` with the name of the Service you want to delete.

   **Usage**:
   ```bash
   kubectl delete service <service-name>
   ```

   **Example**:
   ```bash
   kubectl delete service my-service
   ```
   This would delete the Service named `my-service`.

These commands are essential for managing Kubernetes Services, which allow you to expose your applications running on a set of Pods, either within the cluster (using `ClusterIP`), outside the cluster (using `NodePort` or `LoadBalancer`), or using external services.

Here's how you can fetch logs from Pods and their containers using `kubectl`:

1. **`kubectl logs <pod-name>`**: This command fetches the logs from the main container within a specified Pod. Logs provide insights into what's happening inside the Pod, such as application output or error messages.

   **Usage**:
   ```bash
   kubectl logs <pod-name>
   ```

   **Example**:
   ```bash
   kubectl logs my-app-pod
   ```
   This command retrieves the logs from the Pod named `my-app-pod`.

2. **`kubectl logs <pod-name> -c <container-name>`**: If a Pod has multiple containers, this command fetches the logs from a specific container within that Pod. Replace `<pod-name>` with the name of the Pod and `<container-name>` with the name of the container.

   **Usage**:
   ```bash
   kubectl logs <pod-name> -c <container-name>
   ```

   **Example**:
   ```bash
   kubectl logs my-app-pod -c my-container
   ```
   This command retrieves the logs from the container named `my-container` in the Pod `my-app-pod`.

These commands are essential for debugging and monitoring your applications running in Kubernetes, as they allow you to view the logs generated by the containers in your Pods.


Here’s how you can use `kubectl exec` to interact with Pods:

1. **`kubectl exec -it <pod-name> -- /bin/bash`**: This command starts an interactive bash session inside a specified Pod. It’s useful for debugging or running commands interactively within the Pod.

   **Usage**:
   ```bash
   kubectl exec -it <pod-name> -- /bin/bash
   ```

   **Example**:
   ```bash
   kubectl exec -it my-app-pod -- /bin/bash
   ```
   This command opens a bash shell in the Pod named `my-app-pod`. The `-it` flags are used to allocate a terminal and keep it interactive.

2. **`kubectl exec <pod-name> -- <command>`**: This command runs a specified command inside a Pod. It’s useful for executing commands or scripts within the Pod without starting an interactive session.

   **Usage**:
   ```bash
   kubectl exec <pod-name> -- <command>
   ```

   **Example**:
   ```bash
   kubectl exec my-app-pod -- ls /app
   ```
   This command runs `ls /app` inside the Pod named `my-app-pod`, listing files in the `/app` directory.

You can use these commands to troubleshoot issues, run administrative tasks, or test configurations directly inside your Pods.


Here's how you can manage ConfigMaps and Secrets in Kubernetes using `kubectl`:

### ConfigMaps

1. **`kubectl get configmaps`**: List all ConfigMaps in the current namespace. ConfigMaps are used to store configuration data in key-value pairs.

   **Usage**:
   ```bash
   kubectl get configmaps
   ```

   **Example Output**:
   ```
   NAME              DATA   AGE
   my-configmap      2      5d
   another-configmap 1      3d
   ```

2. **`kubectl describe configmap <configmap-name>`**: Get detailed information about a specific ConfigMap. Replace `<configmap-name>` with the name of the ConfigMap.

   **Usage**:
   ```bash
   kubectl describe configmap <configmap-name>
   ```

   **Example**:
   ```bash
   kubectl describe configmap my-configmap
   ```

   **Output**:
   ```
   Name:         my-configmap
   Namespace:    default
   Labels:       <none>
   Annotations:  <none>
   Data
   ====
   key1: value1
   key2: value2
   ```

3. **`kubectl create configmap <name> --from-literal=<key>=<value>`**: Create a ConfigMap from literal values. Replace `<name>` with the desired name of the ConfigMap, and `<key>` and `<value>` with the key-value pair you want to store.

   **Usage**:
   ```bash
   kubectl create configmap <name> --from-literal=<key>=<value>
   ```

   **Example**:
   ```bash
   kubectl create configmap my-configmap --from-literal=key1=value1 --from-literal=key2=value2
   ```

4. **`kubectl delete configmap <configmap-name>`**: Delete a specific ConfigMap from the cluster.

   **Usage**:
   ```bash
   kubectl delete configmap <configmap-name>
   ```

   **Example**:
   ```bash
   kubectl delete configmap my-configmap
   ```

### Secrets

1. **`kubectl get secrets`**: List all Secrets in the current namespace. Secrets are used to store sensitive information, such as passwords or tokens.

   **Usage**:
   ```bash
   kubectl get secrets
   ```

   **Example Output**:
   ```
   NAME              TYPE                                  DATA   AGE
   my-secret         Opaque                                1      5d
   another-secret    kubernetes.io/dockerconfigjson        1      3d
   ```

2. **`kubectl describe secret <secret-name>`**: Get detailed information about a specific Secret. Replace `<secret-name>` with the name of the Secret.

   **Usage**:
   ```bash
   kubectl describe secret <secret-name>
   ```

   **Example**:
   ```bash
   kubectl describe secret my-secret
   ```

   **Output**:
   ```
   Name:         my-secret
   Namespace:    default
   Labels:       <none>
   Annotations:  <none>
   Type:         Opaque
   Data
   ====
   key1:  6 bytes
   ```

3. **`kubectl create secret generic <name> --from-literal=<key>=<value>`**: Create a Secret from literal values. Replace `<name>` with the desired name of the Secret, and `<key>` and `<value>` with the key-value pair you want to store.

   **Usage**:
   ```bash
   kubectl create secret generic <name> --from-literal=<key>=<value>
   ```

   **Example**:
   ```bash
   kubectl create secret generic my-secret --from-literal=password=my-secret-password
   ```

4. **`kubectl delete secret <secret-name>`**: Delete a specific Secret from the cluster.

   **Usage**:
   ```bash
   kubectl delete secret <secret-name>
   ```

   **Example**:
   ```bash
   kubectl delete secret my-secret
   ```

These commands are crucial for managing configuration and sensitive information in Kubernetes, ensuring that data is available and secure for your applications.

</details>

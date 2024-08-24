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

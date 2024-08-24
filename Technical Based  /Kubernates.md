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

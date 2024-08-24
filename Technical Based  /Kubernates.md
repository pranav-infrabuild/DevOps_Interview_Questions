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

<details>

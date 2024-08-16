---
---
## Question 1. How will you run multiple Docker containers in one single host?
### Answer:- Docker Compose is the best way to run multiple containers as a single service by defining them in a docker-compose.yml file.

---
---
## Question 2. If you delete a running container, what happens to the data stored in that container?
### Answer:- When a running container is deleted, all data in its file system also goes away. However, we can use Docker Data Volumes to persist data even if the container is deleted.


---
---
## Question 3. How do you manage sensitive security data like passwords in Docker?
### Answer:- Docker Secrets and Docker Environment Variables can be used to manage sensitive data. 
- Docker Secrets is like a secret locker managed by Docker itself. It keeps your sensitive information safe and only gives it to the containers that absolutely need it. On the other hand, using environment variables is like writing your secrets on sticky notes and sticking them to the containers. While it works, it's not as secure because those sticky notes might be seen by others.
So, if security is a top concern, Docker Secrets is the preferred way to manage sensitive data in Docker containers.

---
---


### Question 4. Can you briefly explain what Docker is and how it differs from traditional virtualization technologies?
<details>

### Key Differences Between Docker and Traditional Virtualization:

1. **Architecture**:
   - **Docker**: Containers run on the same operating system kernel as the host, using the host's OS to manage resources. Each container is isolated but shares the host OS, making them lightweight.
   - **Traditional Virtualization**: Virtual machines (VMs) include a full guest operating system along with the application. Each VM runs on a hypervisor, which abstracts and manages the hardware, creating a more substantial overhead.

2. **Resource Efficiency**:
   - **Docker**: Containers are lightweight because they share the OS kernel and do not require a full OS instance. This allows for faster startup times and better resource utilization.
   - **Traditional Virtualization**: VMs are heavier since they require their own OS, leading to higher resource consumption (CPU, memory, disk space).

3. **Isolation**:
   - **Docker**: Containers provide process-level isolation. While they are isolated from each other and the host, they are less isolated compared to VMs, which can be both an advantage and a disadvantage depending on the use case.
   - **Traditional Virtualization**: VMs provide strong isolation by emulating separate hardware environments, making them suitable for running multiple, potentially conflicting, OS instances on the same physical machine.

4. **Portability**:
   - **Docker**: Containers are highly portable across different environments (development, testing, production) because they package the application and its dependencies together.
   - **Traditional Virtualization**: VMs are also portable but require more resources and have larger footprints, making them less convenient to move across environments.


</details>

## Question 5. How do you ensure that the Docker images you use are secure and free from vulnerabilities?
<details>

**Answer**: To ensure Docker image security, I regularly scan images using security tools like Trivy or Anchore to detect and address vulnerabilities. I prioritize using images from trusted sources or official repositories to minimize risks. Additionally, I keep my base images and dependencies up to date to ensure that any known vulnerabilities are promptly patched. This proactive approach helps maintain a secure and stable environment for my applications.
   
</details>

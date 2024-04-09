## 1. How will you run multiple Docker containers in one single host?
### Answer :- Docker Compose is the best way to run multiple containers as a single service by defining them in a docker-compose.yml file.


## 2. If you delete a running container, what happens to the data stored in that container?
### Answer: When a running container is deleted, all data in its file system also goes away. However, we can use Docker Data Volumes to persist data even if the container is deleted.


## 3. How do you manage sensitive security data like passwords in Docker?
### Answer: - Docker Secrets and Docker Environment Variables can be used to manage sensitive data. 
- Docker Secrets is like a secret locker managed by Docker itself. It keeps your sensitive information safe and only gives it to the containers that absolutely need it. On the other hand, using environment variables is like writing your secrets on sticky notes and sticking them to the containers. While it works, it's not as secure because those sticky notes might be seen by others.

So, if security is a top concern, Docker Secrets is the preferred way to manage sensitive data in Docker containers.

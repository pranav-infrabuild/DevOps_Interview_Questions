# Scenario:

#### 1) Imagine you are tasked with containerizing a Java based application for deployment.you start by creating a docker image using the openjdk:11 base image;as it includes the java run time environment and seems like a logical choice.However you soon realize that this image also includes unnecessary packages and tools,bloating the image size and potentially increasing the attack surface.How can you avoid including unnecessary components in your Docker imagewhile ensuring that it remains secure and efficient?

**ANS**: we can use distroless base image for our java application such as gcr.io/distroless/java, which includes only the java runtime and necessary libraries.This approach results in a similar more secure image that is fitting to your applications need.

**What are Distroless Images?**

:- Distroless images are the docker images that contain only the necessary dependencies to run your application, without including the full linux distribution or package manager.They are designed to be lean ,secure and optimized for specific use cases.


#### 2) You are a devops engineer who received a complaint from a developer about running out of diskspace on their docker machine. upon investigation you discoverd that the issue was caused by large number of stopped containers consuming disk space. How can you resolve this issue and free up disk space on the Docker machine and also help developer to avoid this issue in the future?

**ANS**: To free up disk space we can use the **docker rm $(docker ps -a -q)** command,which removes all stopped containers.This command first lists all containers(including thos that are stopped).To avoid running into diskspace issues in the future consider using the --rm flags when running containers for temporary tasks or experiment **docker run -it --rm image-name**

        

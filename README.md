### Life without Docker:
- (1) "It works on my machine" problems
- Developers would build and test apps on their laptops. When they move the app to servers, it often breaks because of differences in OS, libraries, versions, configurations, etc.
- Without Docker, teams would spend hours or days debugging these environment differences.

- (2) Complex Software Installations
- installing something like a database (e.g., PostgreSQL) required manually:
- download right version of s/w
- install dependencies
- configure services properly

- (3) Heavy Virtual Machines (VMs)
- Before Docker, developers used Virtual Machines like VirtualBox, VMware, or Hyper-V to simulate servers.
- VMs are large, slow to start, and resource-hungry (each VM has a full OS inside).

- (4) Scaling Applications is Difficult
- Manually configuring and copying app binaries.
- Manually setting up load balancers.


### Docker Architecture
- The docker follows a client-server architecture that includes 3 main components
- 1. Docker Client
- 2. Docker Host
- 3. Docker Registry

### 1. Docker Client:
- **Role:** Interface for users to interact with Docker.
- **What it does:**
- Sends commands (like docker build, docker run) to the Docker daemon using REST APIs.
- Acts as the primary user Interface
- Can communicate with a local or remote Docker daemon.
- **Common Commands:**
- Docker command to build an image:
- **CMD:** docker build 
- Docker command run a container:
- **CMD:** docker run
- Docker command to download an image from registry:
- **CMD:** docker pull
- Docker command to upload an image to Registry:
- **CMD:** docker push

### 2. Docker Host
- **Role:** System where Docker Daemon runs and containers are executed.
- **Components:**
- **Docker Daemon (dockerd):** Core engine that handles:
- Image management
- Container lifecycle
- Network and volume management
- **Containers:** Running instance of Docker images.
- **Images:** Templates used to create containers.
- **Networks & Volumnes:** For container communication and data persistent.
- **Function:**
- Listens to requests from Docker Client and performs the requested action.
- Manages all container-related operations on the host.

### 3. Docker Register
- **Roles:** Storage and distribution system for Docker images.
- **Types:**
- **Public:** Upload image from Docker Host to Registry.
- **Pull:** Download image from Registry to Docker Host.
- **Example:**
- docker pull nginx
- docker push myrepo/myimage

### Architecture Flow Summary:
- [User] → Docker Client → Docker Daemon (on Host) → Registry (if needed)
- Examples:
- **1. docker run nginx**
- Client tells daemon to pull `nginx` from Docker Hub (if not present)
- Daemon runs the container
- **2. docker build + docker push**
- Client builds image, daemon stores it locally
- Client tells daemon to push it to registry

### What is a daemon??
- Daemon are processes that running silently at the background. They are not interactive.

## Difference between docker image and docker container??

### Docker image:
- **Definition:** A read-only template used to create Docker containers.
- **Contains:** Application code, libraries, dependencies, and configuration.
- **Analogy:** Like a blueprint or recipe.
- **Example:** nginx:latest, python:3.11

### Docker container:
- **Definition:** A running instance of a Docker image.
- **Contains:** Everything defined in the image + a writable layer.
- **Analogy:** Like a dish cooked using the recipe.
- **Lifecycle:** Create → Start → Run → Stop → Remove

### Docker hub:
- https://hub.docker.com/

### What is namespace in docker??
- In Docker, a namespace is a Linux kernel feature used to isolate resources for containers.
- A namespace provides separation between containers and the host system (and between containers themselves), so each container thinks it's running on its own isolated system.

### Docker Architecture (By Askok Sir)
- **Dockerfile:** Dockerfile is used to specify where is our app-code and what dependencies are required for our application execution. Dockerfile is required to build docker image.
- **Docker Image:** Docker Image is a package which contains (app_code + dependencies)
- **Docker Registry:** Docker Registry is used to store Docker Images.
- **Docker Container:** When we run docker image then Docker container will be created. Docker container is a linux virtual machine.

### Docker installation in Amazon Linux AMI (By Ashom Sir)
- **Note:** The commands is specific to Amazon Linux. If its ubuntu the commands to install docker is different.
- **Step-1 :** Create EC2 VM (Amazon linux ami) 
- **Step-2 :** Connect with that vm using ssh client (git bash)
- **Step-3 :** Execute below commands
- **Installation of Docker**
sudo yum update -y
sudo yum install docker -y
sudo service docker start
- ** Add ec2-user user to docker group**
sudo usermod -aG docker ec2-user
- **Exit from terminal and Connect again**
exit
- **Verify Docker installation**
docker -v

### Docker commands (By Ashok Sir)
- To display docker images available in our system:
-**CMD:** docker images

- When you run a docker image, a docker container will be created. A docker container is nothing but a linux machine
- To display running docker containers ONLY: 
-**CMD:**docker ps 

- To display BOTH running & stopped containers:
-**CMD:** docker ps -a 

- To download docker image from docker hub we use pull command. 
- We can pull it from docker hub by using image-id or by its name:
-**CMD:** docker pull <image-id/name>
- $ docker pull ashokit/spring-boot-rest-api

- To create/run docker container
-**CMD:** docker run <image-id/name> : 
- $ docker run hello-world

- To delete docker container
-**CMD:** docker rm <container-id> : 

- To stop running docker container
-**CMD:** docker stop <container-id> : 

- To start docker container which is in stopped state
-**CMD:** docker start <container-id>

- To delete docker imageTo delete docker image
-**CMD:** docker rmi <image-id/name>

- To display container logs
-**CMD:** docker logs <container-id> 

- To delete stopped containers + unused images + build cache
-**CMD:** docker system prune -a


### Running java springboot app Real-world applications using docker images (By Ashok Sir)
- public docker image name (java springboot app) : ashokit/spring-boot-rest-api
-**CMD:**
docker pull ashokit/spring-boot-rest-api
docker run ashokit/spring-boot-rest-api
docker run -d ashokit/spring-boot-rest-api

- **USE ME: **
**Syntax :** docker run -d -p <host-port>:<container-port> ashokit/spring-boot-rest-api
- Note: To access application running in the container we will use below URL
- http://host-public-ip:host-port/welcome/{name}

**Example:** 
docker run -d -p 9091:9090 ashokit/spring-boot-rest-api
http://3.89.33.118:9091/welcome/LUKE

### What is Port Mapping???
-**NOTE:** By default, services running inside a Docker container are isolated and not accessible from outside.
- Docker port mapping is the process of linking container port to host machine port.
- It is used to  allow external access to applications running inside the container.
**Syntax :** docker run -p <host_port>:<container_port> image_name
**Note:** host port and container port no need to be same.

### Running python-flask-app Real-world applications using docker images (By Ashok Sir)
docker pull ashokit/python-flask-app
docker run -d -p 5051:5000 ashokit/python-flask-app
http://host-public-ip:host-port/
http://3.89.33.118:5051/

### Dockerfile
- Dockerfile contains set of instructions to build docker image.
- Filename : Dockerfile
- To write dockerfile we will use below keywords

### (1) FROM:
- It is used to specify base image required to run our application.
- **Example:**
FROM openjdk:17
FROM python:3.3
FROM tomcat:9.0
FROM mysql:8.5
FROM node:19.5

### (2) MAINTAINER:
- MAINTAINER is used to specify who is author of this Dockerfile.
- This is Optional in Dockerfile.
- **Example:**
MAINTAINER Ashok <ashok.b@oracle.com>

### (3) RUN
- RUN keyword is used to specify instructions (commands) to execute at the time of docker image creation.
- Note: We can specify multiple RUN instructions in Dockerfile and all those will execute in sequential manner.
- **Example:** 
RUN 'git clone <repo-url>'
RUN 'mvn clean package'

### (4) CMD
- CMD keyword is used to specify instructions (commands) which are required to execute at the time of docker container creation.
- **Note:** If we write multiple CMD instructions in dockerfile, docker will execute only last CMD instruction.
- **Example:** 
CMD 'java -jar app.jar'
CMD 'python app.py'

### (5) COPY
- COPY instruction is used to copy the files from source to destination.
- **Note:** It is used to copy application code from host machine to container machine.
Source : HOST Machine
Destination : Container machine
- **Example:** 
COPY target/app.jar  /usr/app/		
COPY target/app.war /usr/bin/tomcat/webapps/
COPY app.py  /usr/app/

### (6) ADD
- ADD instruction is used to copy the files from source to destination.
- **Example:**
ADD <source> <destination>
ADD <URL> <destination>

### (7) WORKDIR
- WORKDIR instruction is used to set / change working directory in container machine.
- **Example:** 
COPY target/sbapp.jar /usr/app/
WORKDIR /usr/app/
CMD 'java -jar sbapp.jar'

### (8) EXPOSE
- EXPOSE instruction is used to specify application is running on which PORT number.
- **Note:** By using EXPOSE keyword we can't change application port number. It is just to provide information the people who are reading our Dockerfile.
- **Example:** 
EXPOSE 8080

### (9) ENTRYPOINT
- It is used to execute instruction when container is getting created.
- **Note:** ENTRYPOINT is used as alternate for 'CMD' instructions.
CMD "java -jar app.jar"
ENTRYPOINT ["java", "-jar", "app.jar"]

### What is the diff between 'CMD' & 'ENTRYPOINT' ?
- CMD instructions we can override while creating docker container. ENTRYPOINT instructions we can't override.













































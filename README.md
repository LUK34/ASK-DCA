### Life without Docker:
- **(1) "It works on my machine" problems**
- Developers would build and test apps on their laptops. When they move the app to servers, it often breaks because of differences in OS, libraries, versions, configurations, etc.
- Without Docker, teams would spend hours or days debugging these environment differences.

- **(2) Complex Software Installations**
- installing something like a database (e.g., PostgreSQL) required manually:
- download right version of s/w
- install dependencies
- configure services properly

- **(3) Heavy Virtual Machines (VMs)**
- Before Docker, developers used Virtual Machines like VirtualBox, VMware, or Hyper-V to simulate servers.
- VMs are large, slow to start, and resource-hungry (each VM has a full OS inside).

- **(4) Scaling Applications is Difficult**
- Manually configuring and copying app binaries.
- Manually setting up load balancers.

- https://training.mirantis.com/dca-certification-exam/

### Docker ubuntu installation commands: (Currently not going with this. We will be going with AWS Linux)
- https://docs.docker.com/engine/install/ubuntu/

### Product page of Docker Desktop:
- https://www.docker.com/products/docker-desktop


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
- Whenever a docker image runs -> docker container is created. Docker container is assigned a unique identifier called **UUID**.
- **UUID** is used to identify the docker container among others.
- UUID is difficult for humans to understand. So docker also allow us to supply container names.
- By default, if we do not specify the continaer name, docker supplies randomly generated name from 2 words, joined by an underscore.


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

- The docker inspect command is used to view detailed information about Docker objects, such as containers, images, networks, or volumes.
-**CMD:** docker inspect <object_type> <object_name_or_id>

- The command netstat -ntlp is used to display active TCP connections and listening ports along with the process IDs (PIDs) and the program names that are using those ports.
- -n	Show numerical addresses instead of resolving hostnames
- -t	Display TCP connections only
- -l	Show only listening ports
- -p	Show the PID and name of the program using the port
-**CMD:** netstat -ntlp

- This CMD will basically give you the list of IDs associated with the Docker containers which are currently
present within my workstation.
-**CMD:** docker container ls -aq

- The below command. We are frst passing the list of IDs associated with Docker container which are currently 
present in the workstation. We are passing the ids to docker container stop command.
-**CMD:** docker container stop $(docker container ls -aq)

- This command will forcefully delete all Docker images.
-**CMD:** docker rmi -f $(docker images -q)

- This command will forcefully delete a specific docker image.
-**CMD:** docker rmi -f <image_id_or_name>

- **Important. Refer Neal Vohra`s Docker Certified Associate Course -> Video 18**
- `docker container exec` command runs a new command in a running container.
- The command started using docker exec only runs while the container's primary process(PID 1)
- and it is not restarted if the container is restarted.(Its like inception.)
- Remember every container is a linux machine.
- You can run commands inside the container(containers are basically linux machine) while noyt being logically logged in to the container.
-**CMD:** docker container exec


### Running java springboot app Real-world applications using docker images (By Ashok Sir)
- public docker image name (java springboot app) : ashokit/spring-boot-rest-api
- Under security rule edit inbound rule to include port 9091.
- Refer `Output 1` screenshot.
-**CMD:**
docker pull ashokit/spring-boot-rest-api
docker run ashokit/spring-boot-rest-api
docker run -d ashokit/spring-boot-rest-api
docker run -d -p 9091:9090 ashokit/spring-boot-rest-api
http://3.89.33.118:9091/welcome/LUKE

- **USE ME: **
**Syntax :** docker run -d -p <host-port>:<container-port> ashokit/spring-boot-rest-api
- Note: To access application running in the container we will use below URL
- http://host-public-ip:host-port/welcome/{name}

**Example:** 
docker run -d -p 9091:9090 ashokit/spring-boot-rest-api
http://3.89.33.118:9091/welcome/LUKE

### What is Port Mapping??? (AshokIT)
-**NOTE:** By default, services running inside a Docker container are isolated and not accessible from outside.
- Docker port mapping is the process of linking container port to host machine port.
- It is used to  allow external access to applications running inside the container.
**Syntax :** docker run -p <host_port>:<container_port> image_name
**Note:** host port and container port no need to be same.

### Running python-flask-app Real-world applications using docker images (By Ashok Sir)
- Under security rule edit inbound rule to include port 5051.
- Refer `Output 2` screenshot.
**CMDS:**
docker pull ashokit/python-flask-app
docker run -d -p 5051:5000 ashokit/python-flask-app
http://host-public-ip:host-port/
http://3.89.33.118:5051/

### Dockerfile (AshokIT)
- Dockerfile contains set of instructions to build docker image.
- Filename : Dockerfile
- To write dockerfile we will use below keywords

### (1) FROM:
- It is used to specify base image required to run our application.
- `FROM` instruction specifies the Base Image from which you are building.
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
- **RUN** keyword is **used to specify instructions (commands) to execute at the time of docker image creation.**
- **Note:** We can specify multiple RUN instructions in Dockerfile and all those will execute in sequential manner.
- **Example:** 
RUN 'git clone <repo-url>'
RUN 'mvn clean package'

### (4) CMD
- **CMD** keyword is **used to specify instructions (commands) which are required to execute at the time of docker container creation.**
- **Note:** If we write multiple CMD instructions in dockerfile, docker will execute only last CMD instruction.
- **Example:** 
CMD 'java -jar app.jar'
CMD 'python app.py'

### (5) COPY
- **COPY** keyword is **used to copy the files from source to destination.**
- **Note:** It is used to copy application code from host machine to container machine.
Source : HOST Machine
Destination : Container machine
- **Example:** 
COPY target/app.jar  /usr/app/		
COPY target/app.war /usr/bin/tomcat/webapps/
COPY app.py  /usr/app/

### (6) ADD
- **ADD** keyword is **used to copy the files from source to destination.**
- **Example:**
ADD <source> <destination>
ADD <URL> <destination>

### (7) WORKDIR
- **WORKDIR** keyword is **used to set / change working directory in container machine.**
- **Example:** 
COPY target/sbapp.jar /usr/app/
WORKDIR /usr/app/
CMD 'java -jar sbapp.jar'

### (8) EXPOSE
- **EXPOSE** keyword **is used to specify application is running on which PORT number.**
- **Note:** By using EXPOSE keyword we can't change application port number. It is just to provide information the people who are reading our Dockerfile.
- **Example:** 
EXPOSE 8080
- **When you do not include the EXPOSE <port> instruction in the Dockerfile:**
- Nothing breaks.
- The container still runs normally. But Docker doesn't document or inform others (or Docker tools) which port your application uses.
- ** What EXPOSE does not do:**
- It does not actually publish the port to your host machine.
- It’s only metadata: a way to tell others which port the container is expected to use.
- **So which port will be used if EXPOSE is missing?**
- No port is exposed by default.
- When you run a container from such an image, you must manually map the container port to a host port using the -p flag.
- **Example:**
docker run -p 8080:80 my-image
- Even if the Dockerfile didn’t have EXPOSE 80, this still works because you explicitly told Docker to map host port 8080 to container port 80.

### (9) ENTRYPOINT
- **ENTRYPOINT** keyword is used to execute instruction when container is getting created.
- **Note:** ENTRYPOINT is used as alternate for 'CMD' instructions.
- The bst use for `ENTRYPOINT` is to set the image's main command. ENTRYPOINT doesn't allow you overrid the command.
CMD "java -jar app.jar"
ENTRYPOINT ["java", "-jar", "app.jar"]

### Famous Interview Question:  What is the difference between 'RUN' keyword and 'CMD' keyword ?
- **RUN** keyword is **used to specify instructions (commands) to execute at the time of docker image creation.**
- **CMD** keyword is **used to specify instructions (commands) which are required to execute at the time of docker container creation.**

### What is the purpose of 'COPY' keyword and 'ADD' keyword?
- **COPY** keyword is **used to copy the files from source to destination.**. 
- It is used to copy application code from host machine to container machine.
- **Source:** HOST Machine
- **Destination:** Container machine

- **ADD** instruction is **used to copy the files from source to destination.**

- ** The main difference between COPY and ADD keyword is as follows: **
- The COPY and ADD instructions in a Dockerfile are both used to transfer files from the host system into the Docker image, but they have key differences.
- COPY is the simpler and more predictable command, designed solely to copy files and directories from the build context into the image. 
- On the other hand, ADD includes all the functionality of COPY but also supports two additional features:
- it can automatically extract compressed archive files (like .tar, .tar.gz) and can fetch files from remote URLs. 
- Because of its broader functionality, ADD can sometimes introduce unintended behavior, such as unexpectedly unpacking archives. 
- For this reason, it is generally recommended to use COPY unless the extra capabilities of ADD are specifically required.

### What is the diff between 'CMD' & 'ENTRYPOINT' ?
- CMD instructions we can override while creating docker container. ENTRYPOINT instructions we can't override.

### Sample Dockerfile: (AshokIT)
FROM ubuntu

MAINTAINER Ashok <ashok.b@oracle.com>

RUN echo 'hello from run instruction-1'
RUN echo 'hello from run instruction-2'

CMD echo 'hi from cmd-1'
CMD echo 'hi from cmd-2'


### LAB 1: To build image in dockerfile: (AshokIT)
- **Code for `Dockerfile` **
FROM ubuntu

MAINTAINER Luke Rajan Mathew

RUN echo 'run instruction-1'
RUN echo 'run instruction-2'

CMD echo 'cmd instruction-1'
CMD echo 'cmd instruction-2'

- **CMD:**
docker build -t lrm-img-1
docker run lrm-img-1

- The above command will run the docker image and will immediately exit.
- Because we are not running the image in detached mode.

### LAB 2: Dockerfile for non springboot using Dockerfile (AshokIT)
- install git client
- **CMD:**
sudo yum install git -y

- install maven s/w
- **CMD:**
sudo yum install maven -y

- clone project git repo
- **CMD:**
git clone https://github.com/ashokitschool/maven-web-app.git

- build maven project
- **CMD:**
cd maven-web-app
mvn clean package

- check project war file
- **CMD:**
ls -l target

- build docker image
- **CMD:**
- docker build -t <img-name> .
docker build -t app1 .
docker images

- Create Docker Container
- **CMD:**
docker run -d -p 8081:8080 app1
docker ps

- Enable host port number in security group inbound rules and access our application.
- **CMD:**
- http://public-ip:8080/maven-web-app/
http://54.157.12.247:8081/maven-web-app/


### LAB 3: Dockerizing Java Spring Boot Application using Dockerfile (AshokIT)
- Every JAVA SpringBoot application will be packaged as "jar" file only.
- **Note:** To package java application we will use 'Maven' as build tool.
- To run spring boot application we need to execute jar file.
- **Syntax:** java -jar <jar-file-name>
- **Note:** When we run springboot application jar file then springboot will start tomcat server with 8080 port number (embedded tomcat server).
- **Dockerfile to run SpringBoot App:**
FROM openjdk:11
MAINTAINER "Ashok Bollepalli <797979>"
COPY target/spring-boot-docker-app.jar  /usr/app/
WORKDIR /usr/app/
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "spring-boot-docker-app.jar"]


- clone project git repo
- **CMD:**
git clone https://github.com/ashokitschool/spring-boot-docker-app.git

- build maven project
- **CMD:**
cd spring-boot-docker-app
mvn clean package

- check project war file
- **CMD:**
ls -l target

- **Note:** Dockerfile is already created by Ashok sir. You can check by typing the below command
- **CMD:**
ls -ltr

- build docker image
- **CMD:**
- docker build -t <img-name> .
docker build -t app2 .
docker images

- Create Docker Container
- **CMD:**
docker run -d -p 8083:8080 app2
docker ps

http://54.157.12.247:8083/


### LAB 4: Dockerizing Python Application using Dockerfile (AshokIT)
- Python is a general purpose language.
- **Note:** It is also called as scripting language.
- We don't need any build tool for python applications.
- We can run python application code directley like below
- **Syntax :** $ python app.py
- If we need any libraries for python (Ex: Flask) application development then we will mention them in "requirements.txt" file
- **Note:** We will use "python pip" s/w to download libraries configured in requirements.txt file.
- **Dockefile for Python Flask App Dockerfile**
FROM python:3.6
MAINTAINER Ashok Bollepalli "ashokitschool@gmail.com"
COPY . /app
WORKDIR /app
EXPOSE 5000
RUN pip install -r requirements.txt
ENTRYPOINT ["python", "app.py"]

- Clone git repo Python Flask App
- **CMD:**
git clone https://github.com/ashokitschool/python-flask-docker-app.git

- Go inside project directory
- **CMD:**
cd python-flask-docker-app

- Create docker image
- **CMD:**
docker build -t pyapp_flask .
docker images

- Create container
- **CMD:**
docker run -d -p 5052:5000 pyapp_flask
docker ps

- Access application in browser
- URL : http://public-ip:5052/
http://54.157.12.247:5052/


### To prune all stopped containers, unused images, unused networks, unused volumes because of consumption of disk space:
- **CMD:**
docker system prune -a --volumes

### LAB 5: Dockerizing Angular Application (AshokIT)

- **Build Stage (Multi-stage Build with Node.js)**

- **FROM node:18 AS build**
- This starts a new build stage using the Node.js v18 image.
- The alias build allows us to refer to this stage later in the final image.
- Node.js is required here to install dependencies and build the Angular app.

- **WORKDIR /app**
- Sets the working directory inside the container to /app.
- All subsequent commands will run relative to this directory.

- **COPY package*.json ./**
- Copies package.json and package-lock.json (if it exists) from your host to the container.
- These files are essential for installing project dependencies.

- **RUN npm install**
- Installs the npm dependencies listed in package.json.

- **COPY . .**
- Copies the entire Angular project (source code and files) into the container's /app directory.

- **RUN npm run build --prod**
- Runs the Angular CLI build command in production mode, outputting the built app into /app/dist/angular_docker_app.

- **Production Stage (Nginx)**
- **FROM nginx:alpine**
- This starts a second stage using a lightweight Alpine-based Nginx image.
- Nginx is used to serve the static Angular files.

- **EXPOSE 80**
- Informs Docker that the container will listen on port 80 (standard HTTP port).

- **COPY --from=build /app/dist/angular_docker_app /usr/share/nginx/html**
- Copies the built Angular files from the build stage to Nginx's default web root directory.
- These files are now ready to be served by Nginx.

-**Dockerfile:**
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build --prod
FROM nginx:alpine
COPY --from=build /app/dist/angular_docker_app /usr/share/nginx/html
EXPOSE 80

-**CMD:**
git clone https://github.com/ashokitschool/angular_docker_app
cd angular_docker_app
docker build -t ngapp .
docker run -d -p 80:80 ngapp
docker system prune -a --volumes


### Difference between COPY and ADD (Zeal Vohra):
- COPY takes in a src and destination. It omly lets you copy in a local file or directory from your host.
- ADD lets you do that too, but it also supports 2 other source:
- 1. First, you can use a URL instead of a local file/directory.
- 2. Secondly, you can extract a tar file from the source directly.

### LAB 6: Dockerizing React Application (AshokIT)

- **Stage 1: Build the App**
- FROM node:18.13.0 as build
- Uses the official Node.js 18.13.0 image
- This stage is named build for reference in the next stage.

- **WORKDIR /app**
- Sets the working directory inside the container to /app.

- **COPY package*.json ./**
- Copies package.json and package-lock.json (if it exists) to the container. These files contain dependencies.

- **RUN npm install**
- Installs all dependencies listed in package.json.

- **COPY . .**
- Copies the rest of the project files into the container's /app directory.

- **RUN npm run build --prod**
- Builds the project in production mode.
- This will usually generate a build (React) or dist (Angular) folder containing optimized static files (HTML, CSS, JS).

- **Stage 2: Serve with Nginx**
- FROM nginx:latest
- Uses the official Nginx image to serve the static files.

- **COPY --from=build /app/build /usr/share/nginx/html**
- Copies the production-ready build output from the previous stage (build stage) to Nginx’s default public directory.

- **EXPOSE 80**
- Opens port 80 so Nginx can serve traffic on that port.

-**Dockerfile:**
FROM node:18.13.0 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build --prod
FROM nginx:latest
COPY --from=build app/build /usr/share/nginx/html
EXPOSE 80

-**CMD:**
git clone https://github.com/ashokitschool/ReactJS_Docker_App
cd ReactJS_Docker_App
docker build -t ngapp .
docker run -d -p 81:80 ngapp
docker system prune -a --volumes

### LAB 7: Exploring `docker container exec` by using nginx image (Zeal vora) (IMPORTANT)
- `docker container exec` command runs a new command in a running container.
- **IMPORTANT**
- **The command started using docker exec only runs while the container's primary process(PID 1)**
- **and it is not restarted if the container is restarted.(Its like inception.)**
- Remember every container is a linux machine.
- **docker container run -d --name <name to the container> <image to use for the container>**
- **docker container run:** This tells Docker to create and start a new container.
- **-d:** This flag stands for detached mode. It means the container will run in the background (you won't see logs or interact with it in the terminal immediately).
- **--name docker-exec:** This assigns a name (docker-exec) to the container, so you can refer to it easily in other commands (like docker stop docker-exec or docker exec -it docker-exec bash).
- **nginx:** This specifies the image to use for the container. In this case, it uses the official nginx image from Docker Hub, which starts an NGINX web server.

- **netstat -ntlp**
- This command is used to display active network connections and listening ports on your Linux system, with specific filtering. Here's a breakdown of each option:
- **-n:** Show numerical addresses instead of resolving hostnames or service names. (Faster and clearer output.)
- **-t:** Show only TCP connections (not UDP).
- **-l:** Show only listening ports (services waiting for incoming connections).
- **-p:** Show the PID and name of the program using the socket.

- **/etc/init.d/nginx status**
- This command is used to manage the NGINX service on systems that use SysVinit (older Linux service management system).
- Checks the current status of the NGINX service.
- It tells you if NGINX is running or stopped.

- **/etc/init.d/nginx stop**
- This command is used to manage the NGINX service on systems that use SysVinit (older Linux service management system).
- Stops the NGINX web server.
- It shuts down the NGINX process gracefully (if it's running).
- When you execute the above command in the set of below commands. Because you are accessing inside the container, once you execute the command -> 
- this will stop the container and you will come out of the host. When you check all the exiting active running processes, there is nothing.

- **NOTE:** If you try to run commands which are not available inside the container, you will get an error.
- So in this case you need to make sure that the executable file is in the path and available inside the container.
- The error will appear like : **\"bash\":executable file not found in $PATH"**

- You can run commands inside the container(containers are basically linux machine) while not being logically logged in to the container.
- docker container exec -it <container name>
- docker system prune -a --volumes
- docker run --name mynginx -d -p 8000:80 nginx

- **CMDS:**
docker container run -d --name docker-exec nginx
docker ps
docker container exec -it docker-exec bash
/etc/init.d/nginx status
cd /bin
ls -ltr
apt-get update -y
apt-get install -y net-tools
which netstat
exit
docker ps
docker container exec -it docker-exec netstat -ntlp
docker container exec -it docker-exec bash
netstat -ntlp
/etc/init.d/nginx stop
docker ps
docker system prune -a --volumes


### Importance of `it` flag: (Zeal vora) (IMPORTANT)
- Every process that we create in Linux enviroment, has 3 open file descriptors they are stdin,stdout,stderr.

- **1. stdin (Standard Input)**
- **Purpose:** Receives input from the user or another program.
- **Stream Number:** 0
- **Example:** When you type something in the terminal, it's sent to stdin.
- cat              # waits for input from stdin (keyboard)
- hello world      # you type this

- **2. stdout (Standard Output)**
- **Purpose:** Displays normal output of a program.
- **Stream Number:** 1
- **Example:** When you run echo Hello, it prints to stdout.
- echo "Hello" > output.txt  # sends stdout to a file

- **3. stderr (Standard Error)**
- **Purpose:** Displays error messages.
- **Stream Number:** 2
- **Example:** If a command fails, the error goes to stderr.

- The purpose of the it flag (often written as -it) in Docker is to run a container interactively with a terminal session attached.
- **Breakdown of -it:**
- **-i (interactive):** Keeps STDIN (standard input) open even if not attached. This allows you to interact with the container.
- **-t (tty):** Allocates a pseudo-TTY (a terminal). This makes the terminal behave more like a real one, which is needed for commands like bash or sh.

- **CMD:**
docker run -it ubuntu bash

- Starts a new container from the ubuntu image.
- Opens a terminal inside it.
- Launches the bash shell so you can type commands inside the container.

- **Use case:**
- Use -it when you want to:
- Interact with the container's shell
- Debug an issue inside the container
- Explore a container manually

### Default container commands: (Zeal vora) (IMPORTANT)
- Whenever we run a container, a default command executes which typically runs as PID 1.
- This command can be deined while we are defining the container image.

- The default container command refers to the command that a container executes when it starts if no other command is specified at runtime.
- This is defined in the Docker image itself, typically through either:
- the **CMD** instruction in the Dockerfile
- the **ENTRYPOINT** instruction

### Restart policy of the container (Zeal vora)
- Docker provides restart policies to automatically control the behavior of containers when they exit or fail.
- These policies ensure that containers are restarted under certain conditions, which is useful for maintaining availability.
- Here are the main restart policies in Docker:

- **1. no (default)**
- **Description:** Do not automatically restart the container.
- **Use case:** You want full manual control.
- **Example:**
docker run --restart=no my-image

- **2. always**
- **Description:** Always restart the container regardless of the exit status.
- **Use case:** Long-running services that should always be available.
- **Note:** Even restarts after Docker daemon restarts.
- **Example:**
docker run --restart=always my-image

- **3. on-failure[:max-retries]**
- **Description:** Restart only if the container exits with a non-zero exit code (i.e., it failed).
- **Optional:** You can limit the number of retries with :max-retries.
- **Use case:** When you want to retry only on errors, but avoid infinite restart loops.
- **Example:**
docker run --restart=on-failure my-image
docker run --restart=on-failure:5 my-image  # Retry up to 5 times

- **4. unless-stopped**
- **Description:** Restart the container unless it is explicitly stopped.
- **Use case:** Like always, but won't restart the container if you stopped it manually.
- **Example:**
docker run --restart=unless-stopped my-image

### Disk usage metric for Docker component: (Zeal vora)
- The `docker system df` command is used to show the amount of disk space used by:
- **Docker images**
- **Containers**
- **Volumes**
- **Build cache**
- **CMD:**
docker system df
docker system df -Verify

### automatically delete container on Exit:
- To delete a Docker container automatically on exit, use the --rm flag when running the container:

- **What It Does:**
- Runs the container.
- Removes it immediately after it stops (whether it exits normally or crashes).
- Saves you from cleaning up stopped containers manually.

- **Example:**
docker run --rm ubuntu echo "Hello, Docker!"
- This runs the Ubuntu container, prints the message, and deletes the container once it's done.

- **Notes:**
- You cannot use --rm with --restart policies.
- Useful for short-lived or testing containers.


-**CMD:**
docker container run -dt --rm --name testcontainer busybox ping -c10 google.com
docker ps
docker logs testcontainer
docker ps -a

### LAB 8: Dockerfile for nginx (Zeal Vohra):
- **FROM ubuntu**
- **Purpose:** This sets the base image for your Docker image.
- It tells Docker to use the official Ubuntu Linux image from Docker Hub as the starting point.
- **RUN apt-get update**
- **Purpose:** Updates the package lists in the container.
- This ensures you have the latest information about available packages before installing anything.
- **RUN apt-get install -y nginx**
- **Purpose:** Installs the nginx web server in the container.
- **-y** automatically confirms the installation prompts.
- **COPY index.nginx-debian.html /var/www/html**
- **Purpose:** Copies your custom HTML file from your local context (the folder where you run docker build) into the web root directory of nginx inside the container.
- **CMD nginx -g 'daemon off;'**
- **Purpose:** Starts the nginx server when the container runs.
- **'daemon off;'** tells nginx to run in the foreground instead of background (which is required for Docker containers to keep running).

- **index.nginx-debian.html**
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

    <h1>Welcome to Base Nginx Container from Luke Rajan Mathew</h1>

</body>
</html>

- **Dockerfile:**
FROM ubuntu
RUN apt-get update
RUN apt-get install -y nginx
COPY index.nginx-debian.html /var/www/html
CMD nginx -g 'daemon off;'

- **CMD**
docker build .
docker images
docker container run -d -p 8001:80 --name lrm_custom_nginx <Specify image id from images>
curl 13.222.180.142:8001
docker system prune -a --volumes

- **URL:**
13.222.180.142:8001


### HEALTHCHECK Instruction (Zeal Vohra):
- **HEALTHCHECK** 
- This instruction Docker allows us to tell the platform on how to test that our application is healthy.
- When docker starts a container, it monitors the process that the container runs. If the process ends, the container exists.
- That's just a basic check and does not necessarily tell the detail about the application.
- If the exit code of the command is zero, then you will typically see the status as healthy.
- However, if you see the status code of something like one, then it would be marked as unhealthy.
- 0: Success. The container is healthy and ready for use.
- 1: Failure. The container is not working correctly.
- 2: Reserved. Do not use this exit code.

-**Sample Use-Case**
- Check every 5 minute or so that a web-server is able to serve the site's main page within 3 seconds.
- **CMD:** HEALTHCHECK --interval=5m --timeout=3s \ CMD curl -f http://localhost || exit 1

-**Overciew of CURL**
- CURL is used with -f option to fail silently.
- This is mostly done to better enable scripts etc to better deal with failed attempts.
- curl -f http://dexter.kplabs.in/test214.txt
- curl -f http://dexter.kplabs.in/test214.txt | exit 1

- **Dockerfile**
FROM busybox
HEALTHCHECK --interval=5s --timeout=2s --retries=3 CMD ping -c 1 54.144.122.13

- Refer `Output 13_HealthCheck`

- **CMD:**
mkdir mony
chmod 777 mony
cd mony
docker container run -dt --name busybox busybox sh
docker ps
docker build -t monitoring_health .
docker container run -dt --name monitor_cn monitoring_health sh
docker ps
echo $?
ping -c1 <ip addrs>
echo $?
docker inspect monitor_cn

ping -c1 54.144.122.13


### LAB 9: Dockerfile WORKDIR example (Zeal Vohra):
-**Note:**
- The WORKDIR instruction can be used multiple times in a Dockerfile.
- **WORKDIR** keyword is **used to set / change working directory in container machine.**
- **Example:** 
COPY target/sbapp.jar /usr/app/
WORKDIR /usr/app/
CMD 'java -jar sbapp.jar'

-**Sample Snippet:**
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
-**Output:/a/b/c**

-**Dockerfile: Example 1**
FROM busybox
RUN mkdir -p /example/demo
WORKDIR /example/demo
RUN touch file01.txt
CMD ["/bin/sh"]

-**Dockerfile: Example 2**
FROM busybox
RUN mkdir -p /example/demo/context1/context2
WORKDIR /example/demo
WORKDIR context1
WORKDIR context2
RUN touch file01.txt
CMD ["/bin/sh"]

- Refer `Output 14_WORKDIR`

-**CMD: Example 1**
docker build -t workdir-demo .
docker run -dt --name workdir-demo workdir-demo sh
docker exec -it workdir-demo sh

-**CMD: Example 2**
docker build -t workdir-demo2 .
docker run -dt --name workdir-demo2 workdir-demo2 sh
docker exec -it workdir-demo2 sh
docker system prune -a --volumes

### LAB 10: ENV Example (Zeal Vohra):
- The ENV instruction set the enviroment variable <key> to the value <value>

- **Settig Enviroment Variables from CLI**
- You can use the -e,--env and --env-file flags to set simple enviroment variable in the
- container you're running, or overwrite variable that are defined in the Dockerfile of the
- image you 're running.

- **Snippet Code:**
docker run -env VAR1=value1 --env VAR2=value2 ubuntu env | grep VAR

- **Dockefile:**
FROM busybox
ENV NGINX 1.2
RUN touch web-$NGINX.txt
CMD ["/bin/sh"]

- Refer **Output 15_ENV**

**CMDS:**
docker build -t image-env .
docker run -dt --name image_env_1 image-env sh
docker ps
docker exec -it image_env_1 sh
ls -ltr

### Tagging Docker Images (Zeal Vohra):
- create a directory and place a `Dockerfile` in this directory.
- `-t` is used to specify the tag of the docker image.
- Inside the directory execute the below command.
**CMD: First way -> On building the image you can create the tag for the image**
docker build -t demo:v1 .

**CMD: Second way -> Tagging an existing image that was not tagged before**
docker build .
docker images
docker tag <Image id> demo:v2
docker tag 324505958 demo:v2
docker images

**CMD: Third way -> Creating a second tag for an already existing tag**
- This will create an alias for the primary image.
docker tag <specify the image name that you want to tag>
docker tag ubuntu:latest myubuntu:v2

### LAB 11: Docker commit command (Zeal Vohra):
- Whenever you make changes inside the container it can be useful to commit a container file
changes or settings into a new image.
- By default, the container being commited and its processes will be paused while the image is
commited
- **Syntax:**
docker container commit CONTAINER-ID myimage01

- **Dockefile:**
FROM busybox
ENV NGINX 1.2
RUN touch web-$NGINX.txt
CMD ["/bin/sh"]

**Commit CMD:** docker container commit <container name> <new name that you want to appear in docker images list>

** What happens after we commit??**
- we will be commiting the container with a new name. 
- After that we will run the commited container with a new name
- **1. Source Container:** busybox_v1 is the name (or ID) of an existing container that you have customized or modified (e.g., created files, installed packages).
- **2.Commit:** Docker takes a snapshot of that container's filesystem as it is right now.
- **3.New Image:** A new image named busybox_modi_01 is created based on that snapshot.
- **4.Usage:** You can now use this new image to launch new containers with all the changes you made in busybox_v1.

- Refer **Output 16_Commit**

- **CMD:**
docker run -dt --name busybox_v1 busybox
docker ps
docker container exec -it busybox_v1 sh
ls -ltr
cd root
ls -ltr
vi change1.txt
ls -ltr
exit
docker container commit busybox_v1 busybox_modi_01
docker images
docker run -dt --name busybox_custom_v1 busybox_modi_01
docker ps
docker container exec -it busybox_custom_v1 sh
cd /root
ls -ltr
vi change1.txt
exit


### Layers of Docker Image (Zeal Vohra):
- A Docker image built up from a series of layers.
- Each layer represents an instruction in the image's Dockerfile.
- The major difference between a container and an image is the top writeable layer.
- All writes to the container that add new or modify existing data are stored in this writable layer.
**Video 41. Refer Neal Vohras notes.**

### Managing images with CLI (Zeal Vohra):
- **CMD:** docker image --help
- **Video 43:Neal Vohra -> Inspecting Docker images**
- **E.g: docker image inspect nginx --format='{{.ContainerConfig.Hostname}}'**

### Image Pruning Steps (Zeal Vohra):
- docker image prune command allows us to clean up unused images.
- By default, the above command will only clean up dangling images.
- Dangling Images= Image without Tags and Image not referenced by any container.
- **docker image prune** command will remove all dangling images.
- **docker image prune -a** command will remove all images that is not reference by any container.

### LAB 12: Flattening Docker image (Zeal Vohra):
- In a generic scenario, the more layers an image has the more the size of the imahe.
- Some of teh image size goes from 5 GB to 10 GB.
- Flattening an image to single layer can help reduce the overall size of the image.

- The below will show you how to modify an image to a single layer.
- This is called Flattening the image.
- First create a Dockerfile and build a docker image and then do the below step.

- **Dockerfile:**
FROM ubuntu
RUN apt-get update
RUN apt-get install -y nginx
CMD nginx -g 'daemon off;'

- **Export and Import the container:**
docker build -t myubuntu .
docker run -dt --name myubuntu myubuntu sh
docker export myubuntu > myubuntudemo.tar
ls -l myubuntudemo.tar
cat myubuntudemo.tar | docker import - myubuntu:latest
docker image history myubuntu
docker system prune -a --volumes

- Refer `Output 17`

### Docker Registry (Zeal Vohra):
- A registry a stateless, highly scalable server side application that stores and lets you distribute docker images.
- Docker hub is the simplest example that all of use must have used.
- There are various types of registry available, which includes:
- **Docker Registry**, **Docker Trusted Registry**, **Private Repository (AWS ECR)**, **Dockerhub**

- You need to follow a format to push an image to a specific registry:
- **CMDS:**
docker tag ubuntu:latest localhost:5000/myubuntu
docker images
docker push localhost:5000/myubuntu

### LAB 13: Moving images across hosts (Zeal Vohra):
- The docker save command will save one or more images to a tar archieve and then later import in pendrive and import in another persons laptop.
- **CMDS:**
docker save busybox > busybox.tar

- The docker load command will load an image from a tar archieve.
- **CMD:**
docker load < busybox.tar

-**Dockerfile**
FROM busybox
RUN touch custom.txt
CMD ["/bin/sh"]

- Refer `Output 18`

- **CMDS:**
docker build -t myapp .
docker images
docker save myapp > myapp.tar 
docker images
docker rmi myapp
docker images
docker load < myapp.tar
docker images

### LAB 14: Docker cache (Zeal Vohra)
- Docker creates container images using layers
- Each command that is found in a Dockerfile creates a new layer.
- Docker uses a layer cache to optimize the process of building Docker images and make it faster.
- If the cache can't be used for a particular layer, all subsequent layers won't be loaded from cache.

-**Dockerfile**
FROM python:3.7-slim-buster
COPY . .
RUN pip install --quiet -r requirements.txt
ENTRYPOINT ["python", "server.py"]

-**requirements.txt**

certifi==2018.8.24
chardet==3.0.4
Click==7.0
cycler==0.10.0
decorator==4.3.0
defusedxml==0.5.0

- Refer `Output 19`

-**CMDS:**
docker build -t without-cache .
docker build -t with-cache .

### Docker network:
- Docker takes care of the networking aspects so that container can communicate with other containers and also with the Docker Host
- Docker networking subsystem is pluggable, using drivers.
- There are several drivers available by default, and provides core networking functionality.
- **bridge**
- **host**
- **overlay**
- **macvlan**
- **none**

- **CMD:** 
docker network ls
docker network inspect host
docker network inspect bridge

### Bridge Network:
- A bridge network uses a software bridge which allows container connected to the
same bridge network to communicate, while providing isolation from container which are not connected to that 
bridge network.
- A bridge is the default network driver for Docker.
- If we do not specify a driver, this is the type of network you are creating.
- When you start Docker, a default bridge newtork (also called bridge) is created
automatically, and newly-started containers connect to it unless otherwise specified.
- We also can create User-defined bridge Network which are superior to the default bridge.
**CMDS:**
brctl show

### User-Defined Bridge Network:
- There are various difference between default and user-defined bridge. Some of these includes:
- User-defined bridges provide better isolcation and interoperability between containerized applications.
- User-defined bridges providee automatic DNS resolution between container.
- Containers can be attached and detached from user-defined networls on the fly.
- Each user-defined network creats a configurable bridge.
- Linked containers on the default bridge network share environment
- If you do not specify the driver in the network, by default docker will create this specific network by bridge driver.
**CMDS:**
docker network ls
docker network create --driver bridge lrm_bridge
docker networkd ls
brctl show
ifconfig
docker container run -dt --name mybridge01 --network lrm_bridge ubuntu
docker container run -dt --name mybridge02 --network lrm_bridge ubuntu
brctl show
docker network inspect lrm_bridge
docker ps
docker container exec -it mybridge01 bash
apt-get update && apt-get install net-tools && apt-get install iputils-ping

### Host Networks:
- This **driver removes the network isolation** between the **docker host** and the **docker container** 
to use the host's networking directly.
- For instance, if you run a container which binds to port 80 and use host networking, the Container's
application will be available on port 80 on th host's IP address.
-**CMDS:**
docker network ls
docker container run -dt --name lrm_host --network host ubuntu
docker ps
docker container exec -it lrm_host bash
apt-get update && apt-get install net-tools
exit
docker container run -dt --name bridgey ubuntu
docker ps
docker container exec -it bridgey bash
apt-get update && apt-get install net-tools -y
ifconfig
netstat -ntlp
apt-get install nginx
/etc/init.d/nginx Start
netstat -ntlp
exit
netstat -ntlp
docker container exec -it lrm_host bash
netstat -ntlp
ifconfig
apt-get install nginx -y
/etc/init.d/nginx start
netstat -ntlp
exit
netstat -ntlp

### None Networks:
- If you want to completly disable the networking stack on a container, you can use the none network.
- This mode will not configure any IP for the container and doesn't have any access to the external network as
well as for other containers.
-**CMDS:**
docker network ls
docker container run -dt --name mynone --network none alpine
docker ps
docker container exec -it mynone ash
ifconfig
ping google.com

### Publishing exposed ports of containers:
- We were discussing about an approach to publishing container port to host
-**CMD:**
docker container run -dt --name webserver -p 80:80 nginx
- This is also referred as publish list as it publishes only list of port specified.

- There is also a second approach to publish all the exposed ports of the container.
-**CMD:**
docker container run -dt --name webserver -P nginx

- This is also referred as a **publish all**. In this approach, all exposed ports are published to random ports to host.

### Container Orchestration
- During the inital times, users relied on manual container management or basic scripts (such as python and shell scripts)
to handle tasks like deployment, scaling,networking and monitoring.

- ** ---- Scenario: Server -> App Container + Nginx Container ---- **

- ** ---- Challenge 1 - Manual Scaling ---- **
- So let's say that you have a server one where there are two containers that are running now suddenly
- as part of your promotion or maybe some kind of advertisement, you are receiving very high traffic on your website.
- And the issue is that there is only one container running for app, one container running for nginx.
- So in the situation where there is an unexpected high amount of traffic that is coming to your website,you want some form of scaling to happen.
- Now you also have a second server that is available. However, this scaling will not really happen automatically.
- So now what will happen is the user or the developer. He will have to manually create the container on the second server as well.
- And then whatever traffic that comes to your website, it needs to be load balanced between the containers in the server one and server number two.
- So all of this is a manual process and it takes a lot of time.

- ** ---- Challenge 2 - Lack of Fault Tolerance ---- **
- Let's say that you have some containers that are running in the server one. **Now if a container has failed, it can be due to a wide variety of reasons.**
- **It can be a crash, it can be due to resource exhaustion, etc. the container generally would not automatically restart without manual intervention in most of the cases.**
- Now at this stage we are just taking an example where app container has failed. However, what happens if the entire server one has failed?
- How would you automate things in such cases? So when you have a manual based approach, if the server one fails what you might do in the server two again you will launch the app container.
- You will launch the nginx container, redirect the traffic to the server two and so on. So all of this is a manual process and it makes things difficult for production level environments.

- **To overcome Challenge 1 and Challenge 2. The solution is Container Orchestration.**

- ** ---- CONTAINER ORCHESTRATION ---- **
- **Container orchestration automates the deployment, management, scaling and networking of the containers**.
- Let's say a user wants to launch certain containers in a fleet of servers. 

- **Old Approach:** 
- The user will not manually log in to each of the server, manually run the docker run command to create a container, and so on that is the older approach. 

- **New Approach:** 
- The user will communicate directly to the container orchestration tool. 
- The container orchestration tool will in turn take care of the responsibility of launching the containers in the appropriate server, making sure that this containers are always running.
- **If the containers are not running: ** Let's say if a server one has crashed, then container orchestration tool has that logic to migrate that container or relaunch the container in another server.
- To ensure that containers are always running. So most of the common tasks related to deployment scaling, networking, everything. Container orchestration tool takes care of it.

- **Solution to Challenge 1:**
- Let's say that you have in total of three servers that are running as part of the fleet. As of now, the traffic is less.
- So you have 25% of traffic, and the container orchestration tool has launched one container in server number one.

- **Note:** 
- Many of the mature container orchestration tool can automatically scale the containers up or down, based on the defined policies and the real time traffic.This is the important part to understand.
- Let's say as of now the traffic is less. You only have one container, but at a later stage you have increased the amount of traffic.

- Assume that there is 75% traffic approaching server 1. So this single container might not be able to handle all of the requests.
- In this situation, container orchestration will go ahead and launch multiple set of containers in multiple servers. and also the traffic will be routed equally among all of the servers.
- And this is a great benefit for organizations. Along with that, we were discussing about the challenge related to fault tolerance.
- Now when container orchestration is introduced, it can automatically look into the health check of the containers that are running.

- Let's say that the server 1 has crashed due to some reason. Then container orchestration will go ahead and it has the appropriate logic to create one more set of
- containers in another server and redirect all of the traffic to server number two so that the website is always running.
- So all of this important logic, you don't really have to create the manual scripts related to it. The container orchestration tool automatically takes care of it.
- And this is of great benefit and it saves huge amount of time.

- **Container Orchestration: ** 
- It is the process of automating the networking and management of containers so you can deploy application at scale.
- Provisioning and deployment
- Configuration and scheduling
- Resource allocation
- Load balancing and traffic routing
- Monitor Container health

# DOCKER SWARM (IMPORTANT)

### LAB 15: Docker Swarm (Zeal Vohra)
- Create 3 virtual machines (AWS Linux Machines)
- DOCK-SWARM-ND-1
- DOCK-SWARM-ND-2
- DOCK-SWARM-ND-3
 
- **How to verify OS release:**
- **CMD:** cat /etc/os-release
- **NOTE:** 
- Amazon Linux 2023 (AL2023). In AL2023, yum is no longer available.
- It has been replaced by dnf, and Docker installation is different since CentOS repos like docker-ce.repo won’t work directly.
- No need to add external Docker repos — AL2023 includes Docker in its official repo.
- Use dnf instead of yum.
- Run newgrp docker or log out and back in to activate Docker group access without sudo.

-**The below shell script does the following:**
- Update the system
- Install Docker from Amazon Linux 2023 repositories
- Start and enable Docker service
- Add current user to docker group (optional - to avoid using sudo with docker)
- Print Docker version to verify

- **docker-install.sh  --> OLD**
#!/bin/bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum -y install docker-ce
systemctl start docker
systemctl enable docker

- **docker-install.sh  --> NEW* --> USE ME*
#!/bin/bash
sudo dnf update -y
sudo dnf install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
docker --version

- Refer `Output 20`

- **CMD:**
vi docker-install.sh
chmod +x docker-install.sh
./docker-install.sh

### Initializing Docker Swarm (Zeal Vohra)
- **NOTE: Video 64**
- **Watch Neal Vohra video only. Do not practically implement each step present in the video because here we are using AWS Linux machine.**

- A node is an instance of the Docker engine participating in the swarm.
- To deploy your application to a swarm, you submit service definition to a manager node.
- The manager node dispatches units of work called tasks to worker nodes.

- **NOTE:**
- **In AWS Linux 2 or AWS Linux 2023, the traditional ifconfig command is deprecated and not installed by default.**
- **Instead, you should use the ip command from the iproute2 package.**

- curl http://<AWS Linux machine Public IP Addresse assigned by AWS on start>/latest/meta-data/public-ipv4
curl http://52.55.168.144/latest/meta-data/public-ipv4

- **CMDS:**
ifconfig

- ifconfig confirms: (Example)
- **Interface name:** enX0
- **Private IP:** 172.31.24.212
- **Docker bridge IP:** 172.17.0.1

- ** ---- 1. Manager Node Command: (DOCK-SWARM-ND-1) ---- **
- sudo docker swarm init --advertise-addr <MANAGER-IP>
**CMD:** sudo docker swarm init --advertise-addr 172.31.24.212

- Once you've run the command with sudo, you’ll get a docker swarm join token.
**CMD:** sudo docker node ls

- Find the Security Group attached to the Manager Node. Add Inbound Rule.
- Custom TCP	TCP	2377	Custom: 0.0.0.0/0 or your worker subnet

- **EXAMPLE: This is the type of output you must get**
ID                            HOSTNAME                        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
7u57n27yj3xbvhuhft5giqab9     ip-172-31-24-23.ec2.internal    Ready     Active                          25.0.8
jqqxmomx4cnj2vacvlt5qvn5b *   ip-172-31-24-212.ec2.internal   Ready     Active         Leader           25.0.8
ul1zqxk8pwi48ftuch39ccddd     ip-172-31-26-88.ec2.internal    Ready     Active                          25.0.8

- ** ---- 2. Worker Nodes Command: (DOCK-SWARM-ND-2) ---- **
- **EXAMPLE:**
docker swarm join --token SWMTKN-1-1yafj5xnvght5ihyoykpv3uf361rn9bizhfrurk9kvn2t627pg-1pxaf2mhkhb6fsyaisl74e6o9 172.31.24.212:2377


### LAB 16: Services, Tasks and Containers: (Zeal Vohra)
- A service is the definition of the tasks to execute on the manager or worker node.

**CMD:**docker service create--name webserver -replicas 1 nginx

- When you run the above command. Docker swarm will create one nginx container in one of the nodes which are part of the swarm.
- Now, once this container is created and due to some reason this container has stopped working, the swarm will automatically restart that container.
- Whenever the orchestrator detected that the container stopped working, The orchestrator will create the container again in either of the nodes.

**CMDS:**
docker service create --name nginx_webserver --replicas 1 nginx
docker service ls
docker service ps nginx_webserver
docker ps
docker stop <container-id>
docker service ps nginx_webserver
docker service ls
docker service rm nginx_webserver
docker ps


### LAB 17: Scaling service in Swarm
- Once you have deployed a service to a swarm, you are ready to use the Docker CLI to scale the number of containers in the service.
- Containers running in a services are called "task".

- **SCALE UP:** Upscale from 1 to 5 tasks
- Execute the below commands in DOCK-SWARM-ND-1
- **CMDS:**
docker service create --name nginx_webserver --replicas 1 nginx
docker service ps nginx_webserver
docker service scale nginx_webserver=5
docker service ps nginx_webserver

- After executing the above commands in DOCK-SWARM-ND-1, go to DOCK-SWARM-ND-2
- **CMDS:**
docker ps
sudo systemctl stop docker

- Go back to DOCK-SWARM-ND-1
- **CMDS:**
docker service ps nginx_webserver

- **SCALE DOWN: ** Downscale from 5 tasks to 1 task
- DOCK-SWARM-ND-1. Originally there are 5 containers. Here we are going to scale down from 5 to 1.
- **CMDS:**
docker service scale nginx_webserver=1
docker service ps nginx_webserver
docker ps

- Make sure you remove the service
- **CMDS:**
docker service rm nginx_webserver

### LAB 18: Scaling service in swarm
- There are 2 ways in which you can scale service in swarm:

- **Scale service -> example 1:**
docker service scale my_webserver=5

- **Scale service -> example 2:**
docker service update --replicas 5 my_webserver
docker service ls

- **CMDS:**
docker service ls
docker service create --name service01 --replicas nginx
docker service create --name service02 --replicas nginx
docker service ls
docker ps

- We apply 2 scaling:
- **CMDS:**
docker service scale service01=2
docker service update --replicas 2 service02
docker ps

- Refer `Output 21`

- ** ---- CORRECT COMMAND ---- **
- **CMD: The below command you can specify n number of services.**
docker service scale service01=3 service02=3

- ** ---- WRONG COMMAND ---- **
- **CMD: The below command you cannot specify n number of services.**
- docker service update --replicas 4 service02 service01

### LAB 19: Replicated + Global service
- There are 2 types of services deployments replicated and global.

- **Replicated Service:**
- For a replicated service, you specify the number of identical tasks you want to run.
- For example, you decide to deploy an NGINX service with 2 replicas each serving the same content.

-**CMDS:**
docker service ls
docker service create --name myreplica --replicas 1 nginx
docker service ps myreplica

- **Global Service:**
- A global service is a service that runs one task on every node.
- Each time you add a node to the swarm, the orchestrator creates a task and the scheduler assigns the task
- to the new node.

- Since the `--mode global`, docker will create this specific service of name `antivirus` and will launch the task in each and every node.
- `docker service ps antivirus` we can see that this antivirus service is installed in all the nodes because of the `--mode global`

- Refer `Output 22`

-**CMDS:**
docker service create --name antivirus --mode global -dt ubuntu
docker service ps antivirus

### LAB 20: Draining swarm node
- You can migrate all the containers which are running to a active node which you know that it is going
to run for a longer period of time and that can be achieved with the help of draining.
- Setting a node to drain means:
- No new tasks (containers) will be scheduled on this node.
- All existing tasks on this node will be stopped and moved to other nodes in the swarm that are in active state.
- This is useful when you're doing maintenance on a node or want to temporarily remove it from handling workloads.

-**CMD:**
docker node update --availability drain <specify node name>

-**Example:**
- **CMD:** docker node update --availability drain ip-172-31-26-88.ec2.internal
- You’re telling Docker Swarm to evacuate all services from this node.
- Docker Swarm will automatically reschedule those tasks on other nodes.

- **CMD:** docker node update --availability active ip-172-31-26-88.ec2.internal
- This brings the node back to active state, allowing it to receive and run tasks again.
- When you later reactivate the node. Swarm will again consider it a valid target for task scheduling.

- **CMD:** sudo docker node ls
- **OUTPUT: **
6mlkmk3ea984   my_webserver.5   nginx:latest   ip-172-31-24-212.ec2.internal   Running         Running 42 seconds ago
[ec2-user@ip-172-31-24-212 ~]$ docker node ls
ID                            HOSTNAME                        STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
7u57n27yj3xbvhuhft5giqab9     ip-172-31-24-23.ec2.internal    Ready     Active                          25.0.8
jqqxmomx4cnj2vacvlt5qvn5b *   ip-172-31-24-212.ec2.internal   Ready     Active         Leader           25.0.8
ul1zqxk8pwi48ftuch39ccddd     ip-172-31-26-88.ec2.internal    Ready     Active                          25.0.8

- let's say we move it to ten. So even if you move it to ten, Docker orchestrator will not create the new task within a node which
- is marked as drain, it will only move it to the nodes which are part of the availability active.
- So if you quickly do a Docker node ls over here, you should see that the Swarm zero three, the availability here is drained.

- Refer `Output 23`

-**CMDS:**
sudo docker node ls
docker node ls
docker service create --name my_webserver --replicas 5 nginx
docker service ps my_webserver
-- docker node update --availability drain ip-172-31-26-88.ec2.internal 
-- docker node update --availability active ip-172-31-26-88.ec2.internal 

### Inspecting swarm services and nodes
- The docker inspect command is used to view detailed low-level information about Docker objects like:
- It returns detailed JSON output describing the object's configuration and current state. This includes:

- For containers: 
- Environment variables, Mount points (volumes), Network settings (IP address, ports), Resource limits (CPU, memory)
- Start time and status, Command that started the container, Bind mounts and volumes, Host configuration

- For images:
- Image ID, Tags, Layers, Size, Creation time, For networks:, Subnet, Gateway, Connected containers

- For networks:
- Subnet, Gateway, Connected containers

- **CMDS:**
- docker service inspect <name of the service>
- docker service inspect <name of the service> --pretty
- docker node inspect <node id -> this we get from docker node ls> 
- docker node inspect <node id -> this we get from docker node ls> --pretty

### LAB 21: Adding Network and publishing ports to Swarm Tasks
- This is like port mapping.
- docker service rm <service name>

- Refer `Output 24`

- **CMDS:**
docker service ls
docker service create --name mi_webserver --replicas 2 --publish 8080:80 nginx
docker service ls
docker service ps mi_webserver 
netstat -ntlp
hostname -i

### LAB 22: Docker Compose:
- **Compose** is a **tool for defining and running multi-container Docker applications.**
- With compose, you use a YAML file to configure your application`s services.
- We can start all the services with single command - docker compose up
- We can stop all the services with single command - docker compose down

- **Commands to install docker-compose**
- install docker compose
- **CMD:**
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

- **Check docker compose is installed or not**
- **CMD:**
docker-compose --version

- **CMDS:**
mkdir DockerCompose
cd DockerCompose
vi docker-compose.yml

-**docker-compose.yml**
version: '3'
services:
  webserver:
    image: nginx
    ports:
      - "80:80"

  database:
    image: redis

-**CMDS:**
docker-compose config
docker-compose up -d
docker ps
docker-compose down
netstat -ntlp


- **CMDS:**
mkdir wordpress
cd wordpress
vi docker-compose.yml

-**docker-compose.yml**
version: '3.3'
 
services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
 
   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}

- **Summary:**
- This Docker Compose file sets up a WordPress site with a MySQL 5.7 database.
- Services:

- **1. db (MySQL Database)**
- Uses image: mysql:5.7
- Mounts a named volume db_data to persist data (/var/lib/mysql)
- Always restarts on failure (restart: always)
- Sets environment variables for:
- MYSQL_ROOT_PASSWORD: somewordpress
- MYSQL_DATABASE: wordpress
- MYSQL_USER: wordpress
- MYSQL_PASSWORD: wordpress

- **2. wordpress (WordPress Application):**
- Depends on the db service (ensures DB starts first)
- Uses image: wordpress:latest
- Maps port 8000 on host to port 80 on container (access WordPress via http://localhost:8000)
- Always restarts on failure
- Configured with environment variables to connect to the MySQL DB:
- Host: db:3306
- User: wordpress
- Password: wordpress
- Database: wordpress

- **3.Volumes:**
- db_data: A named volume for persisting MySQL data

- Refer `Output 25`

- **CMDS:**
docker-compose config
docker-compose up -d
docker ps
docker-compose down

### Deploying Multi-Service Apps in swarm
- A specific web-application might have multiple containers that are required as part of the build process.
- Whenever we make use of **docker service**, **it is typically for a single container image.**
- The **docker stack** **can be used to manage a multi - service application.**

- A **stack** is a groupd of interrelated service that share dependencies, and can be orchestrated and scaled together.
- A **stack** can compose YAML file like the one that we define during Docker Compose.
- We can define everything within the YAML file that we might define while creaing a docker Service.

- Compose does not use swarm mode to deploy services to multiple nodes in a swarm. All containers will be scheduled
- on the current node. To deploy your application across the swarm, use `docker stack deploy`

- **docker-compose.yml**
version: '3'
services:
  webserver:
    image: nginx
    ports:
      - "80:80"

  database:
    image: redis

- docker stop <container-id>
- docker stack deploy --compose-file docker-compose.yml <name of the stack>
- docker stack ps <name of the stack>

- Refer `Output 26`

- **CMDS:**
cd DockerCompose
docker stack deploy --compose-file docker-compose.yml mydemo
docker stack ps mydemo
docker stack rm mydemo

### Locking Swarm Cluster:
- Swarm cluster contains lot of sensitive information, which includes:
- TLS key used to encrypt communication among swarm node.
- Keys used to encrypt and decrypt the Raft logs on disk.
- If your swarn is compromised and if data is store in plain-text, an attack can get all the sensitive information
- Docker lock allows us to have control over keys.
- **Make sure that docker swarm is present. If present only, execute the below commands**
- **sudo -i**: If you're often accessing root-only folders, using sudo -i to enter an interactive root shell can make it easier.

- **CMDS:**
sudo systemctl status docker
docker node ls
sudo -i
cd /var/lib/docker/swarm/certificates
sudo ls -ltr
cat swarm-node.key
docker swarm update --help
docker swarm update --autolock=true

- e.g: SWMKEY-1-f5YWR0+ZZqx159+D0ESwP25RcgDY6vvpZkddAxoRWTs
- **NOTE: `cat swarm-node.key` **
- if the attacker gets hold of this key as well as the certificate, he will be able to decrypt the
- communication as well as various raft encrypted log files.
- So in order for you to avoid such situation, the Docker suggests you to go for the lock feature of swarm cluster.

- **NOTE: `docker swarm update --autolock=true`**
- Once you execute this command `docker swarm update --autolock=true`. You will get a key.
- This key should be kept inside a password manager and will not be able to restart the MANAGER

- **CMDS:**
systemctl restart docker
systemctl status docker
docker node ls

- **NOTE: Aftre executing `docker swarm update --autolock=true` and saving the key in a text file
- If you execute `docker node ls`. It will say swarm is encrypted to be unlocked before it can be used.
- Please use `docker swarm unlock` to unlock it.

- **CMDS:**
docker swarm unlock

- Need to specify the key here.

- If you want to retrieve the key back again
- **CMDS:**
docker node ls
docker swarm unlock-key

- If you want to rotate the key execute the below command:
- **CMDS:**
docker swarm unlock-key --rotate

- e.g: SWMKEY-1-2gpmiMBkH7A2+RCQIqi4qQfIcQBGiSI/g/tnAD9LSMo

- Refer `Output 27`

### Troubleshooting Swarm Services Deployment:
- A service may be configured in such a way that no node currently in the swarm can run its tasks.
- In this case, the service remains in state **pending.**
- There are multiple reason why service might go into pending state.

- **Reason 1:** If all nodes are drained, and you create a service, it is pending until a node becomes available.
- **Reason 2:** You can reserve a specific amount of memory for a service. If no node in the swarm has the required amount of memory,
- the service remain in a pending state until a node is available which can run its tasks.
- **Reason 3:** You have imposed some kind of placement constraints.

-docker ls
-docker inspect <id value of service>
-docker service rm mi_webserver
- **CMDS:**
docker service ls
docker service ps mi_webserver

### Mounting volumes in CONTAINER
- docker container exec -it <container id> bash
- here /mypath is the mount
- let's say that the container that was created is an important container and something wrong happened and it got terminated.
- So even if it gets terminated, all the data that you might have if you are storing it in the `/mypath` directory, 
- you will still be able to access it irrespective of the container lifecycle.
- And this is the reason why it is important to have volumes specifically for the containers which are going to store some important data.

- **sudo -i**: If you're often accessing root-only folders, using sudo -i to enter an interactive root shell can make it easier.

- **CMDS: for DOCK-ND-1**
docker service create --name my_service --mount type=volume,source=myvolume,target=/mypath nginx
docker service ps my_service
docker volume ls

- **CMDS: for DOCK-ND-2**
docker ps
docker volume ls
docker container exec -it <container id> bash
ls -ltr
cd /mypath
ls -ltr
touch test.txt
exit
docker volume ls
sudo -i
cd /var/lib/docker/volumes/
ls -ltr
cd myvolume
ls -ltr
cd _data/
ls -ltr
docker ps

- **CMDS: for DOCK-ND-1**
docker service ls
docker service rm my_service

- Based on the above example. Once we remove the service from node 1. When we remove the service the task will also be removed.
- On logging in docker node 2, the container is removed. Howeve, if you still look into Docker volumne,
- you will see the volume still persists.

- **CMDS: for DOCK-ND-2**
docker ps
docker volume ls
ls -ltr 

Refer `Output 28`


### Control Service Placement:
- Amazon ECS can place the containers based on various factors like the availability zones or even instance types.
- Now, similarly, Docker Swarm also has the capability to push the task or containers on the worker nodes depending upon the specific constraints that we can specify.

- Swarm services provide a few different ways for you to control scale and placement of services on different nodes.
- 1. Replicated and Global Services
- 2. Resource Constraints [requiremnet of CPU and Memory]
- 3. Placement Constraints [only run on nodes with label pci_compliance=true]
- 4. Placement Preferences

- **CMDS: --> Not working in AWS Linux**
docker node ls
docker service create --name my_constraint --constraint node.labels.region==blr --replicas 3 nginx
docker service ps my_constraint
-docker node inspect <node-id>

- **CMDS: --> Currently the below commands are working in AWS Linux (USE ME)**
docker node ls
docker service ls
-docker node update --label-add region=mumbai <node-id>
-docker node inspect <node-id>

- Using `docker node inspect <node-id>`. We will get all the details present in the JSON structure.
- check for the part having json structure like
"Spec":{
	"Labels":{
		"region": "blr",
		"us-east": ""
	}
	....
}

Refer `Output 29`

### Overview of Overlay Networks
- The overlay network driver creates a distributed network among multiple Docker daemon hosts.
- Overlay network allows containers connected to it to communicate securely.

- So let's say that this is a swarm cluster and there are multiple nodes which are running and the containers are in a different node.
- So let's assume here that you have three containers in three different virtual machine and each virtual machine has its own Docker daemon and they are part of the swarm cluster.
- Now if a web server container of Node one wants to communicate with the web server container of Node two,
- or if a web server container of node one wants to communicate with the web server container of Node zero three,
- then this is not possible with the bridge or this is not possible with the host network.
- So in order for the communication to happen, we need to make use of the overlay network.

### Creating custom overlay network for Swarm
- Create a service and define the network which is associated with that specific service.
- We also learned that if you create a network h- ere and if you create a service defining this specific network, the network will get propagated.

- ** ---- IMPORTANT ---- ** 
- Security Group apply inbound rule for worker nodes (DOCK-ND-2 , DOCK-ND-3) + Manager node. Setup the below ports.
- Without the below ports,
2377	Custom TCP
7946	Custom TCP
7946	Custom UDP
4789	Custom UDP

-**CMDS:DOCK-ND-1**
docker network create --driver overlay my_network
docker network ls
docker service create --name my_overlay --network my_network --replicas 3 nginx
docker node ls

-**CMD:DOCK-ND-2**
docker network ls

-**CMD:DOCK-ND-3**
docker network ls

- **CMD:DOCK-ND-2**
docker ps
docker container inspect <container-id> | grep IPAddress
- Our objective is to find the IP Address of the container.
- Ex: 10.0.2.4

- **CMD:DOCK-ND-3**
docker ps
docker container inspect <container-id> | grep IPAddress
- Our objective is to find the IP Address of the container.
- Ex: 10.0.2.3

- **CMD:DOCK-ND-1**
docker ps
docker container exec -it <container-id of my_overlay> bash
apt update && apt install -y iputils-ping
ping 10.0.4.3
ping 10.0.4.5

- **CMD:DOCK-ND-1 -> to remove**
docker service rm my_overlay
docker network rm my_network

- Refer `Output 30`

### Secure Overlay Network
- The overlay network driver creates a distributed network among multiple Docker daemon host.
- Overlay network allows containers connected to it to communicate securely.
- If containers are communicating with each other, it is recommended to secure the communication.
- To enable encryption, when your create an overlay network pass the --opt encrypted flag:

- **CMDs:**
docker network create --opt encrypted --driver overlay my-overlay-secure-network
docker network ls
docker network rm my-overlay-secure-network

- When you enable overlay encryption, Docker creates IPSEC tunnels between all the nodes where tasks are scheduled
- for services attached to the overlay network.
- These tunnels also use the AES algorithm in GCM mode and manager nodes automatically rotate the keys every 12 hours.
- Overlay network encryption is not supported on Windows. If a window node attempts to connect to an encrypted overlay network,
- no error is detected but the node will not be able to communicate.

### Creating swarm services using Templates
- We can make use of templates while running the service create command in swarm.
- **Example:**
- docker service create --name demoservice --hostname="{{.Node.Hostname}}-{{.Service.Name}}" nginx
- <hostname> <servicename>

- **Supported flags for the placeholder templates:**
- --hostname , --mount , --env

- **CMDS:**
docker service create --name nginx_service --hostname="{{.Node.Hostname}}-{{.Service.Name}}" nginx
docker ps
hostname
docker inspect <container_id> | grep Hostname
<hostname> <servicename>
docker container exec -it <container-id> bash
hostname

### Split brain problem and Quorum -> (Neal Vohra) -> Video 95
- **Split Brain:**
- Split brain refers to a scenario where a network partition causes multiple Swarm manager nodes to lose communication with each other.
- As a result, more than one manager node (or group of nodes) believes it is the leader, leading to inconsistent cluster state and potential data corruption or service conflicts.
- **Example:**
- You have a Swarm with 5 manager nodes.
- A network issue splits the cluster into two parts (e.g., 2 nodes in one partition and 3 in another).
- If both partitions think they can elect a leader, both might attempt to manage the cluster independently.
- This leads to conflicting decisions, which is the split brain problem.

- **Quorum:**
- Quorum is the minimum number of manager nodes that must agree (i.e., be online and able to communicate) to perform operations safely, especially leader election and cluster state updates.
- **Formula for Quorum:**
- Quorum=(𝑁/2)+1
- Where N is the total number of manager nodes.
- **Example:**
- If you have 5 manager nodes, the quorum is 3.
- If fewer than 3 nodes are available or communicating, the remaining nodes cannot elect a leader or make cluster decisions.

- **Why Quorum Matters:**
- Quorum prevents split brain. Docker Swarm will refuse to elect a new leader or process updates unless quorum is met.
- This protects cluster integrity by ensuring only one valid leader is active.

- **Important Points about Qourum**
- Cluster Size = 1 , Majority = 0 , Fault Tolerance = 0
- Cluster Size = 2 , Majority = 2 , Fault Tolerance = 0
- Cluster Size = 3 , Majority = 2 , Fault Tolerance = 1
- Cluster Size = 4 , Majority = 3 , Fault Tolerance = 1
- Cluster Size = 5 , Majority = 3 , Fault Tolerance = 2
- Cluster Size = 6 , Majority = 4 , Fault Tolerance = 2
- Cluster Size = 7 , Majority = 4 , Fault Tolerance = 3

- **Important Points:**
- Always use an odd number of manager nodes (e.g., 3, 5, 7) to avoid ties and optimize quorum.
- For production, keep your manager nodes distributed across failure zones (e.g., different availability zones in AWS) to reduce the chance of a split brain.


### Swarm Manage Node High Availability -> (Neal Vohra) -> Video 96
- Manager nodes are responsible for handling cluster management task.
- Let's say that you have one manager node, it is connected to two worker and this manager node has stopped working.
- And tomorrow you have to scale up your service because a traffic is going to increase a lot.
- So in that case, since the manager node is down, you will not be able to perform the scale up service or scale down service or any cluster management service with the swarm.
- And this is the reason why it is very important that you have multiple manager nodes for high availability.

- Manager Node has many responsibility within swarm, these include:
- **Maintaining cluster state**
- **Scheduling services**
- **Serving swarm model HTTP API endpoints**

- Swarm comes with its own fault tolerance features.
- Docker recommends you implement an odd number of nodes according to your organization high-availability requiremnets.
- Using a Raft implementation, the managers maintain a consistent internal state of the entire swarm and all the services running on it.
- An N manager cluster tolerates the loss of at most (N-1)/2 managers.

- ** For this example you must add 3 seperate nodes. Here all the 3 nodes are Manager nodes.**

- **IMPORTANT NOTE: Token of the MANAGER NODE is different from the token of the WORKER NODE.**
- ** DOCK-MNG-1 , DOCK-MNG-2, DOCK-MNG-3 **
- **docker-install.sh  --> NEW* --> USE ME*
#!/bin/bash
sudo dnf update -y
sudo dnf install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
docker --version

- **CMD:**
vi docker-install.sh
chmod +x docker-install.sh
./docker-install.sh

- ** ---- IMPORTANT ---- ** 
- Security Group apply inbound rule for all manager nodes (DOCK-MNG-1 , DOCK-MNG-2 , DOCK-MNG-3 ) + Manager node. Setup the below ports.
- Without the below ports,
2377	Custom TCP
7946	Custom TCP
7946	Custom UDP
4789	Custom UDP

- **CMDS:**
hostname -I

- **CMD:** docker swarm init --advertise-addr <Public IP Address of the Manager node>
- This will generate a token for the worker nodes.

- **CMD: ---- ( USE ME ) ---- ** docker swarm join-token manager
- This will generate a token for the manager nodes.

- **CMDS: DCK-MNG-1**
docker swarm init
docker node ls
docker swarm join-token manager

- **CMDS: DCK-MNG-2**
- <Copy the token from DCK-MNG-1 that is exclusively for manager nodes>

- **CMDS: DCK-MNG-3**
- <Copy the token from DCK-MNG-1 that is exclusively for manager nodes>

- Refer `Output 31`

### Run Manager only nodes in Swarm - Video 97
- Container should NOT be run on MANAGER NODE. It should be run in WORKER NODE.


### Docker System Commands:
- **docker system info**
- Shows general Docker system info

- **docker system df**
- Displays space used and reclaimable by Docker

- **docker system events**
- Streams real-time event logs

- **docker system prune**
- Cleans up stale resources (containers, networks, images, cache)

- **docker system prune -a**
- Also removes all unused images

- **docker system prune --volumes**
- Also removes anonymous volumes

- **docker system prune -a --volumes**
- Aggressive cleanup of nearly all unused objects


# Kubernetes

### Base:
- Kubernetes (K8s) is an open-source container orchestration developed by google.
- It was originally designed by google, and is now maintained by the Cloud Native Computing Foundation.

### Architecture of Kubernetes:
- A Kubernetes cluster consists of a **control plane** + **set of worker machines called nodes** that run containerized applications.

- **Control plane**
- This is the actual Kubernetes cluster.
- So all of the main components that are part of Kubernetes, they are referred to as the control plane.

- **Worker Nodes:**
- These are the actual servers where the container application run.

- **Basic Workflow:**
- User communicates to Control Plane and provide necessary instructions to run containerized applications.
- For example, user might want to run two containers of nginx  or user might want to run an application container, 
- but not on the smaller server on the largest server that is available here.
- So all of these requirements, the user will submit it to the control plane, which is the main Kubernetes master node.
- Control plane will interpret the requirement, and it will go ahead and launch the appropriate container in the nodes that are available as part of the overall cluster.

- **K8S Cluster Components**

- ** (1) Control Node (Master Node)**
- API Server
- Schedular
- Controller Manager
- ETCD

- ** (2) Worker Nodes**
- Kubelet
- Kube Proxy
- Docker Runtime
- POD
- Container

- To deploy our application using k8s then we need to communicate with control node.	
- We will use KUBECTL (CLI) to communicate with control plane.
- **API Server** will recieve the request given by "kubectl" and it will store that request in "ETCD" with pending status.
- **ETCD** is an internal database of k8s cluster.
- **Schedular** will identify pending requests available in ETCD and it will identify worker node to schedule the task.
- Note: Schedular will identify worker node by using kubelet.
- **Kubelet** is called as Node Agent. It will maintain all the worker node information.
- **Kube Proxy** will provide network for the cluster communication.
- **Controller Manager** will verify all the tasks are working as expected or not.
- In Worker Node, "Docker Engine" will be available to run docker container.
- In K8s, container will be created inside POD.
- **POD** is a smallest building block that we can create in k8s cluster to run our applications.
- Note: In K8s, everything will be represented as POD only.
- Pods are used to deploy applications and manage their lifecycle within a Kubernetes environment.

- **Popular feature of Kubernetes:**
- Pod Auto-Scaling
- Service discouvery and load balancing
- Self-healing of containers
- Secret management
- Automated rollouts and rollbacks

- Ashok IT EKS Cluster video: https://youtu.be/is99tq4Zwsc?si=0h53-S7WFtDNwAMh
- Ashok IT Devops Notes: https://github.com/LUK34/DevOps-Documents/blob/main/05-EKS-Setup.md

### What is kubectl ??
- The kubernetes command-line tool, kubectl, allows you to run commands against kubernetes clusters.
- You can use kubectl to deploy applications, inspect and manage cluster resources and view logs.
- To connect to Kbernetes Master, there are 2 important data which kubectl needs:
- 1. DNS/IP of the cluster
- 2. Authentication Credentials

### What is POD ???
- POD is a smallest building block in k8s cluster.
- Our application will be deployed as a POD only in k8s.
- To create PODS we will use Docker images.
- By using single docker image we can create multiple PODS also.
- If we run our application with multiple PODS then we will get high availability.
- Based on our demand we can increase our PODS count and we can decrease our PODS count.
- To create PODS in k8s we will use manifest YML file.

### What is Kubernetes Objects ???
- Kubernetes Objects is basically a record of intent that you pass on to the kubernetes cluster.
- Once you create the object, the kubernetes system will constantly work to ensure that object exists.

### Approach to configure an Object ???
- There are various ways in which we can configure an Kubernetes Object.
- First approach is through the kubectl commands.
- Second approach is through configuration file written in YAML.

### What is YML ???
- YML stands for YET another markup language.
- It is used to store the data in key-value format.
- YML files are both human and machine readable.
- YML file we can save with .yml or .yaml extension.
- Official website : www.yaml.org


### EKS Installation in AWS Linux Machine
- **Note: Ashok It is using Ubuntu. Here you are using AWS Linux. So commands will vary.**
- Ashok IT EKS Cluster video: https://youtu.be/is99tq4Zwsc?si=0h53-S7WFtDNwAMh
- Ashok IT Devops Notes: https://github.com/LUK34/DevOps-Documents/blob/main/05-EKS-Setup.md
- Here we are using Ubuntu image not AWS Linux Image so be carefull of the installation Commands


- ** ---- FOR AWS LINUX ---- **
- **Username:** ec2-user

- 1. Launch new Ubuntu VM using AWS Ec2 ( t2.micro )
- 2. Connect to machine and install kubectl using below commands.
- **CMDS:**
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client

- 3. Install AWS CLI latest version using below commands
- **CMDS:**
sudo dnf install -y unzip
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

- 4. Launch new Ubuntu VM using AWS Ec2 ( t2.micro )
- **CMDS:**
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

- 5. Create IAM role & attach to EKS Management Host
Create New Role using IAM service ( Select Usecase - ec2 )
Add below permissions for the role
Administrator - acces
Enter Role Name (eksroleec2)
Attach created role to EKS Management Host (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created)

- 6. Create EKS Cluster using eksctl\
- **CMDS:**
- eksctl create cluster --name lrm-eks-cluster --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b


eksctl create cluster --name lrm-eks-clstr --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b

- 7. After your practise, delete Cluster and other resources we have used in AWS Cloud to avoid billing
- **CMDS:**
eksctl delete cluster --name lrm-eks-clstr --region us-east-1


### K8S Manifest YML
- To deploy our application in kubernetes we need MANIFEST YML file.
- In k8s manifest YML we will write 4 sections.
- apiVersion: <resource-version-number>
- kind: <resource-type>
- metadata: <resource-info>
- spec: <container-info>

Kubernetes API Documentation URL Link:
https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/#pod-v1-core

**CMDS:**
kubectl explain pods

### 01-Sample.yml

-**CODE:**
---
id: 1234
name: John
gender: Male
skills:
 - Linux
 - AWS
 - DEVOPS
...

-**CMDS:**
<No need to run this is just an example of the structure of yml file>

### 02-pods.yml

- **CODE:
---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
...

- Note: Save the above content in .yml file.
- **CMDS:**
kubectl get pods
kubectl apply -f 02-pods.yml
kubectl get pods
kubectl logs javawebapppod

### Display which worker node our pod is running

- **CMDS:**
kubectl get pods -o wide

- **Note:** By default PODS can be accessed only with in the cluster. We can't access PODS outside.
- To access PODS outside the cluster we need to expose the PODS by using K8S Services concept.

### K8S Service

- When we deploy our application in k8s then PODS will be created for our application..
- For every POD one IP will be generated.
- PODS are short lived objects. We should not access PODS using POD IP because PODS can be destroyed and re-created at any point of time.

- K8S service is used to group the pods and expose them for outside access.
- In K8S we have 3 types of services

- ** ---- (1) Cluster IP ---- **
- It generates one static IP for all PODS based on POD label.
- Ex: 192.168.10.89
- **Note:** using ClusterIP we can access PODS only with in the cluster.
- **Note:** PODS created , PODs Destroyed, PODS increased or decreased still ClusterIP will not change.
- **Example:** Database PODS we should access only within in the cluster. Outside ppl should not access DB pods. In this scenario we can use CLUSTER IP service.
- **What it does:** 
- Exposes the service internally within the cluster.
- Not accessible from outside the cluster.
- **Use case:**
- Internal microservices (e.g., backend talking to a database).

- ** ---- (2) Node PORT ---- **
- It is used to expose the PODS for outside access. 
- Using NODE PUBLIC IP we can access PODS running in that node outside of the cluster also.
- **What it does:**
- Exposes the service on a static port on each Node’s IP address.
- Enables external access to the application using <NodeIP>:<NodePort>.
- **Use case:**
- Quick way to expose services outside the cluster for development/testing.
- **Key Points:**
- Port range for nodePort is 30000-32767.
- Access via: http://<NodeIP>:<NodePort>
- Can expose only a limited number of ports this way.
- Good for non-production or simple setups.

- ** ---- (3) Load Balancer ---- **
- It is used to expose the PODS for outside access.
- When we can access LOAD BALANCER URL, it will distribute the load to all the nodes and all the PODS available for our application.
- Load balancer will work with provider managed cluster service not with KubeAdm.
- **What it does:**
- Provisions an external load balancer (e.g., from AWS, Azure, GCP).
- Automatically maps the service to a public IP address.
- **Use case:**
- Expose services to the Internet in production environments.
- **Key Points:**
- Requires a cloud provider that supports external load balancers.
- The cloud provider allocates a public IP and configures a load balancer for you.
- You can access your app with: http://<external-ip>:80

### 03-NodePort.yml  -> K8S Service Manifest YML

- **CODE:**
---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080
    nodePort: 30070
...

- **Note:** Save this data with .yml extesion

- **CMDS:**
kubectl get svc
kubectl apply -f 03-NodePort.yml
kubectl get svc

- **Note:** Enable NODE PORT in security group inbound rules.
- **Note:** To access our application use below URL
- **URL:** http://worker-node-public-ip:node-port/java-web-app/

### What is NodePort ?
- When we use service type as NodePort then k8s will use one random port number to expose our application on worker node for public access.
- **Node Port Range** : 30,000 - 32767
- **Note:** If we can also fix nodeport number in service manifest yml.

### 04-Pod-NodePort.yml  ->  POD and Service in Single Manifest YML

- **CODE: **
---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080 
    nodePort: 30070
...


- **Pod Definition**
- **Kind:** Pod — you're deploying a single container (ashokit/javawebapp) that exposes port 8080.
- **Label:** app: javawebapp — used by the service to select this pod.
- **ContainerPort:** 8080 — this is the port inside the container where the app listens.

- **NodePort Service**
- **Kind:** Service (NodePort)
- **Selector: app: javawebapp** — matches the Pod label so that the service can forward traffic to the pod.
- **Port: 80** — the port on the service itself.
- **TargetPort: 8080** — forwards traffic to the pod's container port.
- **NodePort: 30070** — exposes the service on every node at this static port.

- **CMDS:**
kubectl apply -f 04-Pod-NodePort.yml
kubectl get pods
kubectl get svc


### What is the difference between labels and selectors???
- In Kubernetes, labels and selectors are closely related concepts but serve different purposes. Here's the difference between them:

- **Labels:**
- **Definition:** Labels are key-value pairs attached to Kubernetes objects (like Pods, Services, Deployments, etc.).
- **Purpose:** They help you organize, group, and identify resources.
- **Example:**
metadata:
  labels:
    app: myapp
    environment: production
- **Use Cases:**
- Identifying which app or component a resource belongs to.
- Filtering objects in kubectl get commands.
- Matching objects using selectors.

- **Selectors:**
- **Definition:** Selectors are queries used to find resources based on their labels.(Used for filteration of pod. E.g: dev and prod pods to filter)
- **Purpose:** They enable controllers (like Services, ReplicaSets) to select and act on specific sets of resources.
- **Types:**
- Equality-based selectors (key=value)
- Set-based selectors (key in (value1, value2))
- **Example (used in a Service):**
selector:
  app: myapp
  environment: production
- This matches any Pod with both labels: app=myapp and environment=production.


### Example: Implementing Labels and Selectors
- **CMDS:**
kubectl run pod-1 --image=nginx
kubectl run pod-2 --image=nginx
kubectl run pod-3 --image=nginx
kubectl get pods
kubectl get pods --show-labels

- Commands to assign labels are as follows:
- **CMDS:**
kubectl label pod pod-1 env=dev
kubectl label pod pod-2 env=stage
kubectl label pod pod-3 env=prod
kubectl get pods --show-labels

- Selectors
- **CMDS:**
kubectl get pods -l env=dev
kubectl get pods -l env=prod
kubectl get pods -l env=stage
kubectl get pods -l env!=dev


- **CMDS: This removes the label named env from the Pod named pod-1.**
kubectl label pod pod-1 env-
kubectl get pods --show-labels
kubectl label pods --all status=running

- **CMDS: Pods are not meant to be stopped and restarted directly like traditional services. However, you can delete a Pod to stop it**
- kubectl delete pod <pod-name>
kubectl delete pod pod-1
kubectl delete pod pod-2
kubectl delete pod pod-3
- kubectl delete pods -all


### What is `ReplicaSet` in Kubernetes???
- A ReplicaSet is a Kubernetes controller that ensures a specified number of identical Pod replicas are always running and available.
- **High availability:** Keeps a consistent number of Pods running.
- **Self-healing:** If a Pod crashes or a node fails, ReplicaSet automatically creates a replacement.
- **Scaling:** Easily increase or decrease the number of Pods by changing the replica count.















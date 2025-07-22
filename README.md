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

- **CODE: replicaset.yml**
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: kplabs-replicaset
spec:
  replicas: 5
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: nginx


- **CMDS:**
kubectl apply -f replicaset.yml
kubectl get replicaset
kubectl get pods
kubectl delete pod <first pod>
kubectl get pods
kubectl get pods --show-labels
kubectl delete pods --all

### Deployments in Kubernetes
- **Challenges with ReplicasSets**
- ReplicaSets works well in basic functionality like managing pods, sclaing pods and similar.

- Deployments provides replication functionality with the help of ReplicaSets, along with various
- additional capability like rolling out of changes, rollback chnages if required.

- Kubernetes Deployment is a resource object used to manage stateless applications by providing features such as:
- Rolling updates
- Rollbacks
- Scaling
- Self-healing

- deployment makes use of replica sets to make sure that there are always a specific number of replicas which are running

- Deployment will create a new replica set. It will launch all the pods within that replica set and that replica set will be connected to the deployment.
- Now once this pods within the replica set is running perfectly, then deployment will go ahead and it
- will remove the pods associated with the previous application.



- **CODE: deployment.yml**
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kplabs-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: nginx

- **CMDS:
vi deployment.yml
kubectl apply -f deployment.yml
kubectl get replicaset
kubectl get pods

- Deployment will create a new replica set. It will launch all the pods within that replica set and that replica set will be connected to the deployment.
- Now once this pods within the replica set is running perfectly, then deployment will go ahead and it
- will remove the pods associated with the previous application.

- **CODE: deployment.yml**
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kplabs-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: nginx:1.17.3

- **CMDS:
kubectl apply -f deployment.yml
kubectl get replicaset

- After adding a specific version for nginx, you will see that there are two replica sets which are created here.
- Now this is very similar to the architecture where we were discussing that in order for a new roll out change to happen,
- deployment will create a second replica set and this second replica set will have the new version of your pods running.
- Now, along with that, it will go ahead and it will also start to remove the pods which are present within the older replica set in a roll out fashion.

- **CMDS:
kubectl describe deployments kplabs-deployment
kubectl get replicaset
kubectl rollout history deployment.v1.apps/kplabs-deployment
kubectl rollout history deployment.v1.apps/kplabs-deployment --revision 1
kubectl rollout history deployment.v1.apps/kplabs-deployment --revision 2

- The exact change is present in revision 2.

- ** Important pointers for deployment**
- Deployment ensure that only a certain number of pods are down while they are being updated.
- By default, it ensures that at least 25% of the desired number of Pods are up (25% max unavailable).
- Deployments keep the history of revision which had been made.

### Deployment Configuration:
- While performing a roll update, there are 2 important configuration to know.
- **maxSurge: ** Maximum number of PODS that can be scheduled above original number of pods.
- **maxUnavailable: **  Maximum number of PODS that can be unavailable during the update.

**Example Use-Case**
- Total PODS in deployment is 8.
- maxSurge = 25%, maxUnavailable = 25% 
- maxSurge = 2 PODS
- maxUnavailable= 2 PODS
- At Most of 10 PODS (8 current pods + 2 maxSurge pods)
- At Least of 6 PODS (6 current pods - 2 maxUnavailable pods)

- **CMDS:**
kubectl create deployment demo-deployment --image=nginx
kubectl set image deployment demo-deployment nginx=httpd
kubectl get pods
kubectl get pods

- **CMDS:**
kubectl edit deployment demo-deployment

- Set the maxSurge from 25% to 0 or 10%. It can take % as well as count of pods, Follow the vi editor mechanism to update and save values.

- **CMDS:**
kubectl set image deployment demo-deployment nginx=nginx
kubectl get pods

### Kubernetes Secrets
- A secret is an object that contains a small amunt of sensitive data such as password, a token or a key.
- Allows customers to store secrets centrally to reduce risk of exposure.
- Store in ETCD database.

- **Multiple risks of hard coding credentials:**
- 1. Anyone having access to the container repository can easily fetch the credentials.
- 2. Developer needs to have credentials of production systems.
- 3. Update of credentials will lead to new docker image being built.

- **CMD:**
kubectl create secret [TYPE][NAME][DATA]

- Elaborating Type:
- (1) Generic:
- File (--from file)
- directory
- literal value
- (2) Docker registry
- (3) TLS

-**CMDS:**
kubectl get secret

- ** ---- 1. Creating secret using generic type ---- **
-**CMDS:**
kubectl create secret generic firstsecret --from-literal=dbpass=mypassword123
kubectl get secret
kubectl describe secret firstsecret
kubectl get secret firstsecret -o yaml

- The password will be in a base 64 encoded format.
- To decode the password
-**CMDS:**
- echo bXlwYXNzd29yZDEyMw== | base64 -d

- ** ---- 2. To load the secret from a file ---- **
- **CMDS:**
vi credentials.txt
kubectl create secret generic secondsecret --from-file=./credentials.txt
kubectl get secret
kubectl get secret secondsecret -o yaml

- ** ---- 3. To load secret in YAML file PART 1 ---- **
- Need to encode the username and password in base64 encoded version.
- Fetch the decoded password. Open a new command prompt and type the following command to decode it.
-**CMD:**
echo <decoded-password>== | base64 -d
echo -n 'dbadmin' | base64
echo -n 'password123' | base64
**CODE: secret-data.yml**
apiVersion: v1
kind: Secret
metadata:
  name: thirdsecret
type: Opaque
data:
  username: ZGJhZG1pbg==
  password: cGFzc3dvcmQxMjM=


- To get the secret type the below command:
-**CMD:**
vi secret-data.yml
kubectl apply -f secret-data.yml
kubectl get secret thirdsecret -o yaml

- ** ---- 4. To load secret in YAML file PART 2 ---- **
- The below code will automatically encode the string. You dont have to encode it seperately
**CODE: secret-stringdata.yml**
apiVersion: v1
kind: Secret
metadata:
	name: stringdata
type: Opaque
stringData:
 config.yaml: |-
	username: dbadmin
	password: password123

-**CMD:**
vi secret-stringdata.yml
kubectl apply -f secret-stringdata.yml
kubectl get secret
kubectl get secret stringdata -o yaml

eksctl create cluster --name lrm-eks-clstr --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
eksctl delete cluster --name lrm-eks-clstr --region us-east-1


### Mounting Secrets in Containers
- Once a secret is created, it is necessary to make it available to containers in a pod.
- There are 2 approaches to achieve this:
- 1. volumes
- 2. Environment variables

- Now in the environment variable type of approach, you have an environment variable which would have been created called as SECRET_USERNAME.
- And this environment variable will basically have the value associated with the secret. So this is one of the approaches where your secret can be associated with a specific environment variable
- and your application can basically fetch the value associated with a given environment variable to get the secret back.
- Now, do remember that in both the approaches that we were discussing in Volume Mount as well as within the environment variable,
- the secret which is being made available to the container is not encoded, it is within the decoded format.
- So you don't really have to worry about your application having to decode the secret before using it.


- **CODE: secretpod.yml**
apiVersion: v1
kind: Pod
metadata:
  name: secretmount
spec:
  containers:
  - name: secretmount
    image: nginx
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    secret:
      secretName: firstsecret

- **CMDS:**
kubectl get secret
kubectl get secret firstsecret -o yaml
kubectl apply -f secretpod.yml
kubectl get pods
kubectl exec -it secretmount -- bash
cd /etc/foo
ls -ltr
cat dbpass
cd ..
ls -ltr

	    
- **CODE: secret-env.yml**
apiVersion: v1
kind: Pod
metadata:
  name: secret-env
spec:
  containers:
  - name: secret-env
    image: nginx
    env:
      - name: SECRET_USERNAME
        valueFrom:
          secretKeyRef:
            name: firstsecret
            key: dbpass
  restartPolicy: Never	  
	  

- **CMDS:**
kubectl apply -f secret-env.yml
kubectl get pods
kubectl exec -it secret-env --bash
echo $SECRET_USERNAME



eksctl create cluster --name lrm-eks-clstr --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
eksctl delete cluster --name lrm-eks-clstr --region us-east-1

### ConfigMaps: (Important Topic)
- This concept is like your application.properties file in springboot app.
- We have an AppA container and depending on the environment, its settings needs to be differed.
- Example Properties:
**Dev Enviroment:** app.env=dev app.mem=2048m app.properties=dev.env.url
**Prod Enviroment:** app.env=prod app.mem=8096m app.properties=prod.env.url

- config maps allows us to do is it allows us to centrally store data.
- So what happens now is that instead of hard coding these things within the container image, what you do is you centrally store this data.
- So currently you're centrally storing the data associated with the dev properties and you're centrally storing the data associated with the prod properties.

- The second advantages of centrally storing data is that you can dynamically change the values. So let's say that tomorrow you want to add one more parameter within your configuration file.
- If your configurations are stored centrally, then you can easily do that.
- However, if your configuration files are hard coded within the container image, then if you want to change even a certain parameter, you will have to completely rebuild that specific image.
- All right, so this method of centrally storing the data is achieved with the help of config maps within the Kubernetes.

- In Kubernetes, a ConfigMap is an API object used to store non-confidential configuration data in key-value pairs. 
- It allows you to decouple configuration artifacts from application code, making your applications more portable and easier to manage.

- **CLI Syntax for creating ConfigMap**
- **CMD:** kubectl create configmap [NAME][DATA-SOURCE]
- the [DATA-SOURCE] can be File, directory, literal value.


- **Use Case:**
- ConfigMaps are used to:
- Inject configuration into Pods
- Store command-line arguments, environment variables, or configuration files
- Share common configuration across multiple Pods

- **Note:**
- Do not use ConfigMaps for sensitive data like passwords or API keys — use Secrets instead.
- Changes to a ConfigMap do not automatically reload in running Pods unless explicitly reloaded or restarted.

- ** ---- 1. Creating ConfigMap using literal value approach ---- **
- **CMDS:**
kubectl get configmap
kubectl create configmap dev-config --from-literal=app.mem=2048m
kubectl get configmap
kubectl get configmap -o yaml

- ** ---- 2. Creating ConfigMap using File ---- **
- create a simple file called `dev.properties` and `prod.properties`

- **dev.properties**
app.env=dev
app.mem=2048m
app.properties=dev.env.url

- **dev-configmap.yml**
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
    - name: test-container
      image: nginx
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: dev-properties
  restartPolicy: Never

- **CMDS: for dev.properties**
vi dev.properties
kubectl create configmap dev-properties --from-file=dev.properties
kubectl get configmap dev-properties -o YAML


- **Explaination:**
- In the Manifest file, So what we are doing, we are creating a simple pod.
- The name of the pod is `configmap-pod` and we are launching it from the nginx container over here.So this is the first part. 
- The second part is also something that you should already know by now where we are basically using the
- volumes, where you have the `volumeMounts` and you have the `volumes`.
- All right. Now within the `volumeMounts`, what we are doing is we are mount the mount path here is the `etc-config`
- and within the volumes here we are referencing the `configMap` here.
- So if you see the name of the volume is `config-volume`, the reference here is the `configMap` and this property is basically the name of the `configMap`.
- So in our case the name of the `configMap` is `dev-properties`.
- So what will happen is whatever data which was there within the `dev-properties` will be mounted inside the `etc-config` directory of your pod.


- **prod.properties**
app.env=prod 
app.mem=8096m 
app.properties=prod.env.url



- **CMDS: for prod.properties**
vi prod.properties
kubectl create configmap prod-properties --from-file=prod.properties
kubectl get configmap prod-properties -o YAML

- **pod-configmap.yml**
apiVersion: v1
kind: Pod
metadata:
  name: configmap-pod
spec:
  containers:
    - name: test-container
      image: nginx
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: dev-properties
  restartPolicy: Never
  
 **CMDS:**
kubectl apply -f pod-configmap.yml
kubectl get pods 
kubectl exec -it configmap-pod bash
cd /etc/config
ls
cat dev.properties

  
eksctl create cluster --name lrm-eks-clstr --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
eksctl delete cluster --name lrm-eks-clstr --region us-east-1


### Services in Kubernetes

- Pods are ephemeral in Kubernetes — they can be terminated, rescheduled, or recreated at any time. Because of this, Services play a vital role in ensuring consistent, reliable access to Pods.
- ** What is a Service in Kubernetes?**
- A Service is an abstraction that defines a logical set of Pods and a policy by which to access them — often called a microservice.
- Think of it as a stable network endpoint that sits in front of one or more Pods and routes traffic to them.

- ** Why Do We Need Services?**
- **Pods:**
- Get destroyed and recreated during updates, scaling, or failures.
- Get new IP addresses every time they are recreated.
- Without a stable way to reference them, clients wouldn't know how to reach the Pods. That’s where Services help.

- **Key Functions of a Service:**
- **Stable Network Identity:** Services get a static IP and DNS name inside the cluster.
- **Load Balancing:** Distributes traffic among the Pods behind the service.
- **Discovery:** Other components in the cluster can discover the service via DNS (my-service.my-namespace.svc.cluster.local).
- **Decoupling:** Clients don’t need to know the dynamic IPs of Pods. They just talk to the service.

- **Types of Services**
- **ClusterIP:**	(Default) Accessible only inside the cluster.
- **NodePort:**	Exposes the service on a static port on each Node’s IP.
- **LoadBalancer:**	Provisions an external IP through cloud provider’s load balancer.
- **ExternalName:**	Maps the service to an external DNS name (used for external services).

- **CMDS:**
```
kubectl get pods
```


- To get the IP Address associated with all the pods.


### Creating First Service and endpoint
- **Service** is a gateway that distributes the incoming traffic between its endpoints**
- **Endpoints** are the underlying PODS to which the traffic will be routed to.


- **Step 1: Create backend PODS**
- **CMDS:**
```
kubectl run backend-pod-1 --image=nginx
kubectl run backend-pod-2 --imae=nginx
```


- **Step2: Create Frontend PODS**
- **CMDS:**
```
kubectl run frontend-pod --image=ubuntu --command --sleep 3600
```

-  **Step 3:** 
- what we'll be doing for it will be connecting to the frontend pod and  from the front end pod we will be making a request or to one of the backend pod that
- we have created and we'll see if we're able to get the response back properly.
- **CMDS:**
```
kubectl get pods -o wide
kubectl exec -it frontend-pod -- bash
```

- **Step 4:**
- Need to install `curl`
- **CMDS:**
```
sudo dnf update && sudo dnf -y install curl
curl <Ip-address of the backend pod 1>
exit
```

- **Step 5:**
-  we create a service and then we'll be associating our back in board with this specific service.
- **CMDS:**
```
vi service.yml
```

**CODE: service.yml**
```
apiVersion: v1
kind: Service
metadata:
	name: kplabs-service
spec:
	ports:
	- port: 8080
	targetPort: 80
```	
	
**CMD:**
```
kubectl apply -f service.yml
kubectl get service
kubectl describe service kplabs-service
kubectl exec -it frontend-pod -- bash
curl <Ip-address of the backend pod 1>
vi endpoint.yml	
```

**CMDS:**
```
kubectl get pods -o wide
```

**CODE: endpoint.yml**
```
apiVersion: v1
kind: Endpoints
metadata:
  name: kplabs-service
subsets:
  - addresses:
      - ip: <ip address of backend pod1>
    ports:
      - port: 80
```

**CMDS:**
```
kubectl apply -f endpoint.yml
clstr
kubectl describe service kplabs-service
- You will get the IP address of backend pod 1
kubectl exec -it frontend-pod -- bash
curl <backend pod 1 ip address>:80
```

**CMDS: To delete the created resources**
```
kubectl delete service kplabs-service
kubectl delete endpoints kplabs-service
kubectl delete pod backend-pod-1
kubectl delete pod backend-pod-2
kubectl delete pod frontend-pod
```

### Understanding Cluster IP:
- Whenever service type is ClusterIP, an internal cluster IP address is assigned to the service.
- Since an internal cluster IP is assigned, it can be reachable from withing the cluster.
- This is a default Service Type.

### Challenges with Manual Approach for endpoints
- At this stage, we were manually defining the IP Address of the PODS in endpoints
- If there are 500 PODS, we will have to manually define each IP in ednpoint.

### Intergration of Service with Selectors
- Kubernetes allows us to define the list of labels of PODS that needs to be added as part of endpoints.
- All PODS that matches that label will be added.

- Whenever a client creates a service they can define to add all the pods with a label of app=nginx
- So now whenever a service will be created, the end point of that service will be associated with the
- IP Address associated with all the ports that has this specific label

- **CMDS:**
```
vi demo-deployment.yml
```

- **CODE: demo-deployment.yml**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

- **CMDS:**
```
kubectl apply -f demo-deployment.yml
kubectl get pods --show-labels
vi service-selector.yml
```

- **CODE: service-selector.yml**
```
apiVersion: v1
kind: Service
metadata:
  name: kplabs-service-selector
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```

**CMDS:**
```
kubectl apply -f service-selector.yml
```

**CMDS: Verify endpoints in Service**
```
kubectl describe service kplabs-service-selector
```

**CMDS: Scale Deployments**
```
kubectl scale deployment/nginx-deployment --replicas=10
kubectl get pods
kubectl get pods --show-labels
kubectl get svc
kubectl describe service kplabs-service-selector
```


**CMDS: To find the exact list of IP Addresses**
```
kubectl get endpoints
kubectl describe endpoints kplabs-service-selector
```

- Lets say you create a new manual pod. And you add the same label `app-nginx` in this pod.
- Whenever Kubernetes sees that a new pod is created with a specific label that is part of the `selector` of that `service`.
- The service end point will automatically add this pod as well.

**CMDS:**
```
kubectl run manual-pod --image=NGINX
kubectl label pods manual-pod app=nginx
kubectl get pods --show-labels
kubectl describe service kplabs-service-selector
```

**CMDS: Delete Created resources**
```
kubectl delete service kplabs-service-selector
kubectl delete -f demo-deployment.yml
kubectl delete pod manual-pod
```

### Service Type: NodePort
- NodePort exposes the Service on each Node's IP at a static port(NodePort)
- You will be able to contact the NodePort Service, from outside the cluster, by requesting
- <WorkerIP>:<NodePort>
- If servicetype is NodePort, then Kubernetes will allocate a port(default:30000-32767) on every worker node.

- Lets say that you have created a service of type `NodePort`. That is lets say for example
- `kplabs-service` is of type `NodePort`
- On the worker node say `Worker Node 1` a new node port will be created, lets say node port:31514
- If anyone wants to connect to this service -> `kplabs-service`. So they will now have to connect to the worker node Public IP, 
- followed by this specific node port and the request will be routed to the node port service.

- **CMDS:**
- **Step 1:** Create Sample POD with Label
```
kubectl run nodeport-pod --labels="type=publicpod" --image=nginx
kubectl get pods --show-labels
```

- **Step 2:** Create NodePort service
```
vi nodeport.yml
```

- **CODE:**
```
appVersion: v1
kind: service
metadata:
	name: kplabs-nodePort
spec:
	selector:
		type: publicpod
	type: NodePort
	ports:
	- port: 80
	  targetPort: 80
```
	  
- **CMDS:**
```
kubectl apply -f nodeport.yml
```

### Overview of the Networking model
- Kubernetes imposes the following fundamental requirements on any networking implementation.
- 1. pods on a node can communicate with all pods on all nodes without NAT.
- 2. All nodes can communicate with all Pods without NAT.
- 3. The IP that a pods sees itself as is the same IP that others see its as.

- Challenges and Solutions:
- Based on the constraints set, there are 4 different networking challenges that needs to be solved:
- 1. Container to Container Networking
- 2. Pod to Pod Networking
- 3. Pod to Service Networking
- 4. Internet to Service Networking

- 1. Container to Container Networking:
- 2. Container to Container networking primarily happens inside a pod.
- 3. Pods can contain group of container with the same IP address.
- 4. Communication between the containers inside pods happens via localhost

-**CMDS:**
```
kubectl get pods
kubectl get pods -o wide
kubectl exec -it nginx --bash
ifconfig
```

- Kuberenetes Ingress is a collection of routing rules which governs how external users access the Services
- running within the Kuberenetes cluster.

### Understanding Liveness Probe Health Checks
- Many application running for long periods of time eventually transition to broken states,
- and cannot recover except by being restarted.
- Kubernetes provides liveness probes to detect and remedy such situations.

-**CODE: livenessprobe.yml**
```
apiVersion: v1
kind: Pod
metadata:
  name: liveness
spec:
  containers:
  - name: liveness
    image: ubuntu
    tty: true
    livenessProbe:
      exec:
        command:
        - service
        - nginx
        - status
      initialDelaySeconds: 20
      periodSeconds: 5
```

- **CMDS:**
```
kubectl run -it ubuntu --image=ubuntu
kubectl get pods
kubectl exec -it <ubuntu pod id> service nginx status
kubectl exec -it <ubuntu pod id> ls /tmp
kubectl exec -it <ubuntu pod id> bash
sudo dnf update && sudo dnf -y install nginx
```

- Initally it will say that nginx is not running.
- If you execute the echo command and get a non 0 status, it means that nginx is not up and running 
- and the previous command errored out.
- `echo` command is primarily specific to linux.

- **CMD:
```
service nginx status
echo $?
service nginx start
echo $?
service nginx status
echo $?
exit
kubectl delete deployment ubuntu
kubectl apply -f livenessprobe.yml
kubectl get pods
kubectl get pods
```

- when you exec -it into livenessprobe and check the nginx status.
- This command will fail because there is no service nginx that is currently running.
- When the service nginx is not running, which means that there is error.
- The liveness probe will restart the overall container. Basically, it will kill and start the container.

- **CMDS:**
```
kubectl get pods
```

- There are 3 types of probes which can be used with liveness:
- 1. HTTP
- 2. Command
- 3. TCP


### Overview of Readiness probe
- It can happen that an application is running but temporarily unavailable to serve traffic
- **Example:** 
- Application is running but it is loading its large configuration files from external vendor.
- It is like an Anti virus software which is running and receiving its patch updates from another server.
- In such-case, we dont't want kill the container however we also do not want it to serve the traffic.

- **CODE:**
```
apiVersion: v1
kind: Pod
metadata:
  name: readiness
spec:
  containers:
  - name: readiness
    image: ubuntu
    tty: true
    readinessProbe:
     exec:
       command:
       - cat
       - /tmp/healthy
     initialDelaySeconds: 5
     periodSeconds: 5
```	 
	 
- For the readinessProbe code that is present above. 
- The command `cat /tmp/healthy` will be successfull if the file `/tmp/healthy` is present.
- and if the file `/tmp/healthy` is not present, the command will fail + readiness probe will fail.

- **CMDS:**
```
kubectl apply -f readinessprobe.yml
kubectl get pods
```

- The pod will be in non ready state. which mean that the command will fail -> readiness probe will also fail.

- **CMDS:**
kubectl exec -it readiness touch /tmp/healthy

- This will explicitly connect to the pod and will create this `healthy` file under the `tmp` directory
- using the command `touch /tmp/healthy`

- **CMDS:**
```
kubectl get pods
```

- If you see the status is running then the readiness probe is running.

- **CMDS:**
```
kubectl exec -it readiness rm /tmp/healthy
kubectl get pods
kubectl exec -it readiness cat /tmp/healthy
```

- The readiness probe will fail.
- So in summary, if the application is running it doesnt mean that it is ready to serve the traffic.
- It can mean that it is doing lot of thing at the background, which is why the readiness probe is used to 
- determine if the application is ready to use or not.

### Understanding DaemonSets
- Let's say that we have 3 nodes and we want to run a single copy of a pod in every node.
So let's say that we have three worker nodes which are available and we want to run a single copy of a pod in every worker node.

- So this can be better explained with a diagram where you have three worker node over here and you want to run a single copy of a pod.
- Let's assume that its is a web server in every node over here. Now, if I have to ask you this question, what would be your outright answer?
- So **the common answer that you will see is create a deployment with a replica set of three, which will go ahead and launch three pods, mostly in each and every node.**
- So **this is the ideal case, but this can not always be true. It can happen in replica set and deployments that two copy or even three copy of a pod can be launched in a single node.**
- So it all depends upon a lot of factors based on which the scheduling happens. **So with the typical use case, we cannot directly make use of replica sets and assume that scheduler will launch a single copy of pod in every node.**
- So in order to achieve that you can make use of daemon set.

- A DaemonSet can ensures that all Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them.
- So one of the simple use case that I can share here is the antivirus. 

- ** FIRST USE CASE: **
- **So let's say that you have three worker nodes and what you want is you have an antivirus pod which regularly scans for all the files within the node to verify if it has some malicious data or not.**
- So in such kind of a use case, ** you want to run your antivirus pod in each and every node and you don't want to run multiple copies of your antivirus pod.**
- ** You just want to run a single copy of your antivirus pod in every node. So this is the first use case.**

- ** SECOND USE CASE: **
- ** Second is whenever a new node comes up, then that new node should also have a single copy of your antivirus pod.**


- ** So the FIRST and SECOND USE CASE can be achieved with the help of daemon set.**


- ** CODE: daemonset.yml **
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: kplabs-daemonset
spec:
  selector:
    matchLabels:
      name: kplabs-all-pods
  template:
    metadata:
      labels:
        name: kplabs-all-pods
    spec:
      containers:
      - name: kplabs-pods
        image: nginx
```

- ** CODE EXPALINATION: **
- The name of the DaemonSet is kplabs-daemonset.
- This defines the selector used to match the pods that this DaemonSet will manage.
- It looks for pods with the label name: kplabs-all-pods.
- This section defines the Pod template that the DaemonSet uses to create pods.
- The pod will have the label name: kplabs-all-pods to match the selector.
- The pod will run a single container named kplabs-pods.
- It uses the nginx image, which means each node will have an nginx container running.

- ** What Happens When You Apply This YAML: **
- Kubernetes will schedule one nginx container on every node in the cluster.
- If a new node is added later, Kubernetes will automatically run the same pod on the new node as well.

- ** CMDS: **
```
kubectl get nodes
kubectl apply -f daemonset.yml
kubectl get pods
kubectl get nodes -o wide
```

- ** Note: **
- So anytime a new worker node gets created, your daemonset will go ahead and make sure that a pod is
- created in that specific worker node.

### Taints and Toleration:
- Taints are used to repel the pods from a specific nodes.

- **Example:**
- So let's understand this with a simple example. Now, on the left hand side, you have a **worker node one** and on the right hand side you have the **worker node two**.
- And along with that you have pod one and you have pod two. Lets say that you apply a taint on worker node one, so you can consider this to be a boundary. 
- So there is a **taint** applied to the **worker node one**.
- So now whenever there is a taint applied to a specific node and if a pod is scheduled within that specific node by default, it will be blocked.
- So if a **scheduler** tries to **schedule a pod on a node which has been tainted, then the effect would be blocked**.
- However, if you look into the worker node two, Node two does not have that taint.
- And this is the reason why **if the same pod is being scheduled to the worker node two, it will be scheduled in that specific node.**
- So this pod will be running in the worker node two. Now the question that comes is **how can you go ahead and schedule a pod in a worker node one which has a taint applied to it?**
- So the **answer** to that is **tolerations**. ** So in order to enter the taint worker node, you need a special pass **.
- So I call it as special pass. And this special pass is called as the Toleration.
- So ** let's take the same scenario where you have a taint applied to a worker node 1.**
- ** The pod is trying to schedule itself to this specific worker node 1, and it has also brought its special pass.**
- ** And since it has this special pass, this pod will be allowed to be schedule within this worker Node one.**
- So now this pod can go ahead and run in the worker node one along with that.
- One important part to remember is that pod can also run in worker node two if it is scheduled over there, because worker node two does not really have the taint.

- **CMDS:**
```
kubectl get nodes
kubectl describe node <place worker node 1 name>
kubectl taint nodes <worker node 1 name> key=value:NoSchedule
kubectl describe node <worker node name>
kubectl run nginx --image=nginx --replicas=5
kubectl get pods -o wide
```


- **CMDS:**
```
vi toleration.yml
kubectl apply -f toleration.yml
kubectl get pods -o wide
kubectl describe node
```

- **CMDS: To taint a particular node **
```
kubectl taint nodes <worker node 1 name> key=value:NoSchedule
```

- **CMDS: To untaint a particular node **
```
kubectl taint nodes <worker node 1 name> key=value:NoSchedule-
```

### Resource Requests and limits
- If you schedule a large application in a node which has limited resource, 
- then it will lead to `out of memory` exception or others and will lead to downtime.

- Requests and Limits are 2 ways in which we can control the amount of resource that can be assigned to a pod (resource like CPU and Memory)
- **Requests:** Guarenteed to get.
- **Limits:** Makes sure that container does not take node resources above a specific value.

- **CMD: requests-limits.yml**
```
apiVersion: v1
kind: Pod
metadata:
  name: kplabs-pod
spec:
  containers:
  - name: kplabs-container
    image: nginx
    resources:
      requests:
        memory: "640Mi"
        cpu: "0.5"
      limits:
        memory: "12800Mi"
        cpu: "1"
```

- ** First scenario: resources: requests: memory: "64Mi" **
- ** First scenario: limits: memory: "128Mi" **

- **CMDS:**
```
kubectl apply -f requests-limits.yml
kubectl get pods -o wide
kubectl get nodes
kubectl describe node <worker node name>
```

- Kubernetes Scheduler decides the ideal node to run the pod depending on the requests and limits
- If your POD requires 8GB of RAM, however there are no nodes within your cluster which has 8BG RAM.
- then your pod will never get scheduled.

- ** Pod Quality of Service (Qos)**
- In Kubernetes, Guaranteed, Burstable, and BestEffort are Pod Quality of Service (QoS) classes that determine how CPU and memory resources are allocated and prioritized during contention.
- These QoS classes affect scheduling and eviction behavior — especially when nodes run low on resources.

- ** 1.Guarenteed **
- Highest priority and most reliable.
- **Requirements:**
- Every container in the Pod must have requests = limits for both CPU and memory.
- **Behavior:**
- Guaranteed to get the resources it requests.
- Last to be evicted during memory pressure.

- ** 2.Burstable **
- Medium priority.
- ** Requirements: **
- At least one container has requests and limits, but they are not equal.
- Or, not all containers have both CPU and memory limits set.
 -** Behavior: **
- Gets minimum requested resources, but can burst up to the limit if available.
- Evicted after BestEffort but before Guaranteed during pressure.

- ** 3. BestEffort **
- Lowest priority and least reliable.
- **Requirements:**
- No requests or limits specified for any container.
- **Behavior:**
- Gets leftover resources.
- First to be evicted when node is under resource pressure.

- **CMDS: delete the old pod first**
```
kubectl delete -f request-limits.yml
```


- **CMD:**
```
vi requests-limits.yml
```

- ** Second scenario: resources: requests: memory: "6400Mi" **
- ** Second scenario: limits: memory: "128Mi" **

- **CMD:**
```
kubectl apply -f requests-limits.yml
```

- You will expect an error saying that Invalid value: "6400Mi" must be less than or equal to memory limit.

- ** Third scenario: resources: requests: memory: "6400Mi" **
- ** Third scenario: limits: memory: "12800Mi" **

- **CMD:**
```
kubectl apply -f requests-limits.yml
kubectl get pods
kubectl describe pod
```

### Network Policies in Kubernetes:
- Pods are non-isolated, they accept traffic from any source.
- **Example:**
- Pod 1 can communicate with Pod 2
- Pod 1 in namespace DEV can communicate with POD 3 in namespace Staging.


- Sceanrio 1: Pod Compromise
**CODE: nsp-deny-pod.yml**
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-pod
  namespace: default
spec:
  podSelector:
    matchLabels:
        run: pod01
  policyTypes:
  - Ingress
  - Egress
```

- **CMDS:**
```
kubectl run -it pod01 --image=bus
exit
```

- **CMDS:**
```
hostname -I
- Obtain the ip address and open a new mobaxterm cmd and run ping <Ip address>
kubectl apply -f nsp-deny-pod.yml
```

- **CMDS:**
```
kubectl run -it pod02 --image=bus
exit
```



### Are Pods Containers?
- Not exactly. A Pod is the smallest deployable unit in Kubernetes. It can contain one or more containers.
- Think of a Pod as a wrapper around containers that shares the same network namespace and storage volumes.
- Most Pods typically run one container, but multiple containers can be grouped in a Pod if they need to work closely together (like a helper process).

### Are Containers Linux Machines?
- No. Containers are isolated environments running on the same OS kernel of the host machine (usually Linux, but can be Windows too).
- They are not full Linux machines or VMs. They are lightweight, using cgroups and namespaces for resource isolation.
- You can run Linux-based applications inside them, but they share the host OS kernel.

### What is a Node in Kubernetes?
- A Node is a worker machine in Kubernetes. It can be:
-  A physical server or a virtual machine
- Each Node runs:
- ** Kubelet: **  agent that talks to the Kubernetes control plane.
- ** Container runtime: ** like Docker, containerd, etc., to run containers.
- **Kube-proxy: ** handles networking for the Pods on that Node.
- ** Nodes are where pods actually run.**



# ---- Installation and Configuration of Docker EE ---- 

- watch Zeal Vohra videos. Docker EE is enterprise edition and is valid for 1 month only so if you are practicing you may have to pay for it.


** CMDS: to install DockerEE in AWS Linux**
```
export DOCKERURL="INSERT-YOUR-URL-HERE"
 
echo "7" > /etc/yum/vars/dockerosversion
 
sudo -E sh -c 'echo "$DOCKERURL/centos" > /etc/yum/vars/dockerurl'
 
yum-config-manager --add-repo $(cat /etc/yum/vars/dockerurl)/docker-ee.repo;
 
yum -y install docker-ee docker-ee-cli containerd.io
 
systemctl start docker
```

- ** Step by Step docker installation in AWS Linux**
- ** CMDS: **
- Step 1: Update the system
```
sudo yum update -y
```

- Step 2: Install Docker (Amazon Linux 2 uses its own repo with Docker package)
```
sudo amazon-linux-extras enable docker
sudo yum install -y docker
```

- Step 3: Start the Docker service
```
sudo systemctl start docker
```

- Step 4: Enable Docker to start on boot
```
sudo systemctl enable docker
```

- Step 5: (Optional) Add current user to the docker group to run Docker as non-root
```
sudo usermod -aG docker $USER
```

- Step 6: Verify Docker installation
```
docker version
```

- **Installing Docker UCP**
docker container run --rm -it --name ucp -v /var/run/docker.sock:/var/run/docker.sock docker/ucp:3.1.4 install --host-address 139.59.82.72 --force-minimums

- In Docker Universal Control Plane (UCP), Access Control refers to managing who can do what within a Docker Enterprise cluster.
- UCP implements Role-Based Access Control (RBAC) to securely manage user permissions for resources like services, containers, volumes, and secrets.

- ** Key Concepts of UCP Access Control**

- ** 1. Users and Teams **
- **Users:** Can be created in UCP or integrated via LDAP/AD.
- **Teams:** Groups of users. Access control is usually applied to teams rather than individual users for easier management.

- ** 2. Roles **
- Roles define what actions can be performed. There are default roles, and you can also create custom roles.
- **Role: ** View Only
- **Permissions: ** Can view resources, but cannot modify them
- **Role: ** Restricted Control
- **Permissions: **	Can perform limited management (e.g., restart container)
- **Role: ** Full Control
- **Permissions: **	Full admin rights over resources
- **Role: ** None
- **Permissions: ** No access

- ** 3. Collections **
- Collections are logical groupings of resources (services, nodes, volumes, etc.). Think of it as a namespace. Access control is applied per collection.

- ** 4. Grants **
- Grants assign roles to teams or users over specific collections.

- ** How Access Control Works **
- ** Example:**

- You have:
- **Team:** DevOpsTeam
- **Collection:** /devops-app
- **Role:** Full Control

- You grant Full Control of /devops-app to DevOpsTeam. All members of that team can now fully manage containers, secrets, and services in that collection.

- ** Access Control Flow**
- User logs in to UCP
- UCP checks the user's teams
- UCP looks at the collection/resource being accessed
- UCP checks if a team (or user) has a role for that collection
- Access is granted or denied based on the role

- **Common Resource Types Controlled**
- Nodes
- Stacks
- Containers
- Secrets
- Volumes
- Networks

- ** Tips for Managing Access **
- Use teams instead of managing access per user.
- Use least privilege principle — only give users the permissions they need.
- Regularly review access grants.
- Create custom roles for specific workflows if default roles don't fit.

- ** What is a Subject**
- In Docker Universal Control Plane (UCP), a subject refers to an entity (user or team) to which access control policies (grants) are applied.

- ** In a Grant Statement: **
- Each grant in UCP consists of:
- **Subject:** Who (User or Team)
- **Role:**  What permissions
- **Collection:** Where (resource group)

- ** General:**
- Subject -> Organisation -> Teams -> Users

- ** Docker Trusted registry Installation **

- ** 1. What is DTR (Docker Trusted Registry)?**
- DTR stands for Docker Trusted Registry. It is an enterprise-grade image storage solution that integrates with Docker Enterprise and is used to store, 
- manage, and secure Docker images in a private, on-premises registry. DTR is typically deployed alongside Docker Universal Control Plane (UCP) and Docker Engine in Docker Enterprise.

- ** 2. What are the Features of DTR? **
- Here are the key features of Docker Trusted Registry:
- **Private Registry:**	Host Docker images in a secure, private environment.
- **Role-Based Access Control:**	Integrates with UCP to allow fine-grained access to repositories.
- **Image Signing (Content Trust):**	Enforces image integrity and authenticity using Docker Content Trust.
- **Image Scanning:**	Automatically scans images for vulnerabilities (when used with Docker EE).
- **LDAP/AD Integration:**	Supports enterprise directory services for authentication and access control.
- **High Availability:**	Can be deployed in multi-node setups with replication for fault tolerance.
- **Repository Mirroring:**	Mirrors upstream registries for caching and availability.
- **Web UI & API:**	Includes a browser-based interface and REST API for managing images.
- **Helm Chart Support:**	Supports Kubernetes deployments via Helm charts.
- **Image Promotion:**	Enables controlled promotion of images from dev to production.


- **Architecture of DTR:**
- The architecture of Docker Trusted Registry involves several components working together within a Docker Enterprise environment:
+-----------------------+         +-----------------------+
|  Developers / CI/CD   |  <--->  |   Docker Client (CLI) |
+-----------------------+         +-----------------------+
                                          |
                                          v
+------------------------------------------------------------+
|                  Docker Universal Control Plane (UCP)      |
|     (Authentication, RBAC, Security Policies)              |
+------------------------------------------------------------+
                           |
                           v
+------------------------------------------------------------+
|                   Docker Trusted Registry (DTR)            |
|    - Web UI                                                |
|    - REST API                                              |
|    - Image Storage Backend (S3 / NFS / Local Volume)       |
|    - Metadata DB (typically internal to DTR)               |
|    - Scanning Service (if enabled)                         |
+------------------------------------------------------------+
                           |
                           v
                +----------------------------+
                |  Storage (S3/NFS/Volume)   |
                +----------------------------+



- **DTR Node Roles:**
- **Replica Nodes:** Each DTR replica handles image storage, API requests, and web interface.
- **Storage Backend:** DTR can use a shared storage backend such as Amazon S3, NFS, or a local volume.
- **UCP Integration:** DTR is tightly integrated with UCP for authentication, access control, and RBAC.
- **Scanning Services (optional):** Integrated with security scanners to find vulnerabilities in images.


- **CMD:** 
```
docker run -it --rm docker/dtr install --ucp-node node01 --ucp-username admin --ucp-url https://159.89.168.70 --ucp-insecure-tls
```

- **Uninstalling DTR:**
```
docker run -it --rm docker/dtr:2.6.3 destroy --ucp-insecure-tls
```

- **Blog to remove DTR entry from UCP:**
- https://success.docker.com/article/extra-dtr-listed-in-ucp-31x-requiring-removal

- **TLS Output:**
- INFO[0006] Generated TLS certificate. dnsNames="[*.com *.*.com example.com *.dtr *.*.dtr]" domains="[*.com *.*.com 172.17.0.1 example.com *.dtr *.*.dtr]" ipAddresses="[172.17.0.1]"


- ** Configuring Trusted CA and pushing images to DTR **
- **CMDS:**
```
wget https://159.65.151.132/ca --no-check-certificate
cp ca /etc/pki/ca-trust/source/anchors/example.com.crt
update-ca-trust
systemctl restart docker
docker tag busybox:latest example.com/admin/webserver:v1
docker push exam
docker push example.com/admin/webserver:v1
```

- ** DTR Backup **
- **CMDS:**
```
docker run --log-driver none -i --rm docker/dtr backup --ucp-url https://172.31.40.237 -ucp-insecure-tls --ucp-username admin --ucp-password YOUR-PASSWORD-HERE > backup.tar
```

- **What is DTR Webhooks??**
- DTR Webhooks (Docker Trusted Registry)
- **Definition:**
- Webhooks in Docker Trusted Registry (DTR) are automated callbacks that notify external systems when certain events happen in the registry.
- **Key Points:**
- Used to integrate with CI/CD pipelines, security scanners, or monitoring tools.
- Webhooks can be triggered on events such as:
- 1. Image push
- 2. Image pull
- 3. Delete events
- Configured via the DTR UI or API.
- Payload typically includes metadata about the event: repository, tag, actor, etc.

# ---- Section 6: Security: ---- 

### What is UCP Client??
- UCP Client (Universal Control Plane)
- ** Definition:**
- The UCP client is used to interact securely with a Docker Enterprise cluster managed by Docker Universal Control Plane (UCP).
- **Key Points:**
- UCP client bundle provides TLS certificates and the correct DOCKER_HOST endpoint for secure access.
- Allows secure use of docker CLI and kubectl to interact with UCP-managed resources.
- Downloaded from the UCP web UI:
- 1. Admin UI → My Profile → Client Bundle
- Extract the bundle and source the env.sh to set up the environment:
- **CMDS:**
source env.sh
docker info  # To test connection

### What is Linux Namespace
- ** Definition:**
- Linux namespaces provide isolation of system resources between containers.
- Types of Namespaces (important for exam):
- pid – Process ID namespace
- net – Network stack (interfaces, routing)
- mnt – Mount points
- ipc – Inter-process communication
- uts – Hostname and domain
- user – User and group IDs
- **Purpose:**
- Each container gets its own isolated set of namespaces, giving it the illusion of a dedicated system.


### What is Control Groups (cgroups)
- **Definition:**
- Control Groups are a Linux kernel feature used by Docker to limit, prioritize, and isolate resource usage (CPU, memory, I/O, etc.) of containers.
- **Key Points:**
- Prevent one container from starving others of resources.
- Examples of resource limits:
- **--memory=512m**
- **--cpus="1.5"**
- Docker applies cgroups to containers to enforce these limits.
- Essential for multi-tenant environments and resource control.


### Limiting CPU for Containers
-  --cpus=<value>
- **Purpose:**
- Limits the total CPU time a container can use across all cores.
- **Example:**
docker run --cpus="1.5" ubuntu
- **Explanation:**
- The container can use up to 1.5 CPU cores (150% of a single core).
- This is a fractional and flexible CPU limit across all available CPUs.
- Good for soft CPU limits in shared environments.

 **--cpuset-cpus=<cpu_list>**
- **Purpose:**
- Restricts the container to specific CPU cores (by CPU ID).
- **Example:**
- docker run --cpuset-cpus="0,2" ubuntu
- **Explanation:**
- The container is pinned to CPU 0 and CPU 2 only.
- Useful for performance tuning and CPU affinity.
- Prevents context-switching between cores, improving efficiency for high-performance apps.

### Reservation vs limits
- ** ---- Reservation ---- **
- **Purpose**
- Minimum guaranteed resource allocation
- **Enforced By**
- Docker Swarm scheduler
- **Effect**
- Ensures enough resources are available on a node before placing the container
- **Overcommitment**
- No — scheduler avoids overloading
- **Use Case**
- Ensures service placement on nodes with required resources


- ** ---- limits ---- **
- **Purpose**
- Maximum resource usage allowed
- **Enforced By**
- Linux kernel (via cgroups)
- **Effect**
- Prevents a container from overusing resources
- **Overcommitment**
- Yes — limit is enforced during runtime
- **Use Case**
- Protects host from resource exhaustion

### Swarm MTLS (Mutual Transport Layer Security)
- The nodes, which are part of a swarm, makes use of the **mutual transport layer** security,
- also referred as the TLS to authenticate, authorize and encrypt the communications with the other nodes which are part of the swarm.
- Now, by default, **whenever you initialize a swarm, the node in which you initialize the swarm becomes the manager node**,
- and the manager node generates a new root **certificate authority (CA)** along with a key pair, which is used to secure the communication with the other nodes which joins the swarm.
- You can specify your own externally-generated root CA, using the **--external-ca** flag of the docker swarm init command.

- So each time a node joins the swarm, the manager issues a certificate to that node.
- basically when you initialize a swarm, there is a root certificate authority.
- the communication that happens between the manager node and the worker node and the communication
- that happens between the manager node and the manager node is completely encrypted. This will prevent any outside attack.

- To find the root certificate authority.
- **Note:** You need 2 nodes (servers) -> DOCK-MNG-1, DOCK-WRK-1

- **CMD: DOCK-MNG-1**
docker node ls
docker swarm init

- Remember, by default the manager node generates a new root Certificate Authority (CA) along with key pair,
- which are used to secure communication with other nodes that join the swarm.

- **CMD: DOCK-MNG-1**
```
cd /var/lib/docker/swarm/certificates
```

- Here you will find a `swarm-root-ca.crt` and `swarm-node.key`. This is the certificate that will be created automatically.

- The manager node also generates 2 tokens to use when you join additional nodes to the swarm: one worker token and one manager token.
- **Each token includes the digest of the root CA's certificate and a randomly generated secret. (IMPORTANT) **
- When a node joins the swarm, the joining node uses the digest to validate the root CA certificate from the remote manager.

- Rotating the CA Certificate
- Run `docker swarm ca --rotate` to generate a new CA certificate and key.

- Now one of the question that comes is do I have to update the joint tokens of the existing worker nodes?
- In the above command, docker generated a cross-signed certificate signed by previous old CA.
- After every node in the swarn has a new TLS certificate signed by the new CA Docker forgets about the old CA certificate
- and key material and tells all the nodes to trust the new CA certificate only.
- Join tokens also changes accordingly.

- **CMD: DOCK-MNG-1**
```
docker swarm joint-token worker
```

- This will give you the worker token which you can paste in worker node (DOCK-WRK-1) to join the swarm.
- After pasting in worker node, under the manager node. execute the below comamnds

- **CMD: DOCK-MNG-1**
docker node ls

- Now we need to find the TLS in worker node. so...

- **CMD: DOCK-WRK-1**
```
cd /var/lib/docker/swarm/certificates
ls -ltr
```

- Here under worker node, you can see the node certificate and the node key.
- Now here in the second point we were discussing that each token includes the digest of the root CA certificate and a randomly generated secret.
- So basically, whatever join token that you run in the worker node, it has the digest of the certificate authority.
- So if a certificate authority has been changed, if there are any modification that has happened to the CA, then the join token will not work.
- And this is very important because the worker node can verify with the help of join token that whatever certificate authority which was present while a token was generated that has not been modified.

-**CMD: DOCK-WRK-1**
```
docker swarm leave
ls -ltr
```

- All the certificates in the worker node will be removed.
- Lets come to the manager node and rotate our certificate.

-**CMD: DOCK-MNG-1**
```
docker node ls
```

- remove the worker node from the manager Node. Remove using the node id.

-**CMD: DOCK-MNG-1**
```
docker node rm <node id of worker node>
docker node ls
docker swarm ca --rotate
```

- Now the certificate has been rotated

-**CMD: DOCK-MNG-1**
```
docker swarm joint-token worker
```

- Take the joint token and paste in worker node.

- Now in case the certificate authority has been rotated, then the TLS certificates of all the worker nodes and all the other nodes are also changed.

-**CMD: DOCK-WRK-1**
```
cd /var/lib/docker/swarm/certificates
ls -ltr
md5sum swarm-node.crt
```

- On executing the md5sum command. So this digest basically means that if there is any modification that might happen to this specific node, then the digest value would change.

-**CMD: DOCK-WRK-1**
```
md5sum swarm-node.key
```

- Now go to the manager node and rotate the certificate.

-**CMD: DOCK-MNG-1**
```
docker swarm ca --rotate
docker node ls
```

-**CMD: DOCK-WRK-1**
```
md5sum swarm-node.crt
```

- If you look at the value of the swarm node certificate of the current and previous value.
- So basically whenever you rotate the certificate, the swarm will also rotate all the relevant certificates

- ** Certificate Details:**
- By default, each node in the swarm renews its certificate every 3 months.
- You can configure this interval by running the docker swarm update --cert expiry <TIME PERIOD>
- In the event that a cluster CA key or a manager node is compromised, you can rotate the swarm root CA
- so that none of the nodes trust certificates signed by the old root CA anymore.


### Docker Secrets
- Docker secrets to centrally manage this data and securely transmit it to only those containers that need access to it.
- Secrets are encrypted during transit and at rest in a docker SWARM
- A given secret is only accessible to those services which have been granted explicit access to it, and only while those 
- service tasks are running.

- **CMDS: Only one node is needed**
```
docker swarm init
docker swarm init --advertise-addr <public ip of the server>
docker secret ls
vi db_creds.txt
```

- **CODE: db_creds.txt**
```
username: dbadmin
pass: dbadmin@123$#
```

- Inorder to create a secret do the following:

- **CMDS:**
```
docker secret create dbcreds db_creds.txt
docker secret ls
```

- Remember this secret is encrypted and if you execute `docker secret inspect dbcreds` you will not get the secret.

- **CMDS:**
```
docker service create --name webserver --secret
docker secret ls
docker service create --name webserver --secret dbcreds nginx
docker node ls
docker ps
```

- **So since the service is associated with the secret, all the containers which are created within that specific service will have access to the secret.**

- **CMDS:**
```
docker container exec -it <container id> bash
```
- secerts will be stored in run directory.

- **CMDS:**
```
cd /run/
ls -ltr
cd secrets/
ls -ltr
cat dbcreds
```

### Docker Content Trust:

- When we are downloading image from network or from internet, integrity becomes a true concern.
- Now if there is a middleman who is sitting in between and he has modified the image, then the integrity changes.
- Let's say if he has added some kind of a malicious thing within the image, then if you are running
- that image in production, it will hamper your security and hence integrity becomes a true concern.

- Now, the Docker Content Trust gives us the ability to verify both the integrity and the publisher. Of all the data received from the registry over any channel.
- **Watch Video 195 of Zeal Vohra**
- To enable Docker content trust, you have to enable a specific method:

- **CMD:**
```
export DOCKER_CONTENT_TRUST=1
```


### Overview of Docker group

**CMDS:**
```
useradd newuser
su - newuser
id
docker ps
vi /etc/group
```

- here u need to go to the line `docker:x:993`
- docker:x:993: newuser

- **CMDS:**
```
su - newuser
docker ps
sudo vi /root/test.txt
```

- It will ask for password. The issue if you enter any random password, it will not work
- because this user does not have any sudo priveleges

- **CMDS:**
```
logout
useradd dockeruser
usermod -aG docker dockeruser
vi /etc/group
```

- Here you will see that `dockeruser` is automatically added.
- Among both the approaches -> `usermod -aG docker dockeruser` is the recommended way.

- **CMDS:**
```
su - dockeruser
docker ps
```

- here u will see that the docker user is able to perform the docker commands.

### Linux capabilities for DOCKER

- **CMDS:**
```
docker run -dt --name cap-demo amazonlinux
docker exec -it cap-demo bash
whoami
touch test.txt
yum install -y e2fsprogs
chattr +i test.txt
```


- It should immediately say that the Operation not permitted while setting flags on test.txt

- **CMDS:**
```
whoami
exit
docker run -dt --name cap02 --cap-add LINUX_IMMUTABLE amazonlinux
docker exec -it cap02 bash
yum install -y e2fsprogs
touch test.txt
chattr +i test.txt
```

- The above command will work perfectly well.

- **CMD:**
```rm test.txt```

- This operation is not permitted because this has an immutable bit set.

- **CMD:**
```echo "Hi" > test.txt```

- Operation not permited because there is an immutable bit set.
- To remove the immutable bit.

- **CMD:**
```
chattr -i text.txt
echo "Hi" > test.txt
rm test.txt
```

### Priveleged containers
- By default, docker container does not have many capabilities assigned to it.
- Docker containers are also not allowed to access any devices.
- Hence, by default, docker container cannot perform various use-case like run docker container inside a docker container.

- Priveleged Container can access all the device on the host as well as have configuration in AppArmor or SELinux
- to allow the container nearly all the same access to the hosta as process running outside container on the host.
- 1. Limits that you set for priveleged containers will not be followed.
- 2. Running a priveleged flag gives all the capabilities to the container.

- **CMDS:**
```
docker run -dt --name amazonlinux amazonlinux bash
docker ps
cd /dev
ls -ltr
```

- **CMDS:**
```
docker exec -it amazonlinux bash
whoami
cd /dev
ls -ltr
```

- So within here, if we go into the dev directory, you will see the amount of devices which are associated
- here is much more less than what was actually present within the host.
- And this is the reason why if you do not really run a privileged container, it is considered to be
- much more safer because many of the devices are not directly connected with your Docker container.
- Now we will see the difference between running a normal container and priveleged container

- **CMDS:**
```
exit
docker run -dt --privileged amazonlinux bash
```

- So now what we have done is we have specified a privileged flag. So whatever container that will get started will be of type privileged.

- **CMDS:**
```
docker ps
docker exec -it <container-id> bash
cd /dev
ls -ltr
```
- You will see all the devices which were present within the host are also available within the container.
- So if you want to perform certain operations over these devices, you can certainly do that. So this is one benefit.
- Now the second benefit of privileged container is that the capabilities are not restricted here.

- **CMDs:**
```
yum install -y e2fsprogs
touch test.txt
chattr -i test.txt
```

- So now if I quickly set an immutable bit to the test.txt, you see, it works perfectly well over here.
- Now, the reason why it is working perfectly well is because the Linux capabilities are not restricted 
- within the privileged containers as opposed to the capabilities of that of the Docker container.
- **this is the reason why the usage of privileged containers is something which should be restricted.**


# Section 7: Storage and volumes

### Overview of Docker Storage drivers
- A docker image is built up from a series of layers.
- Storage Driver piece all these things together for you.
- There are multiple storage drivers available with different trade-off.

- Now whenever we create a container from an image, let's say that this is a Ubuntu image and we create a container from this specific image.
- Now after creating a container, when we log in to the container and we run a `ls` command, 
- you will be able to see that multiple files which are spanned across the layers are merged together and they are shown to you as a single output.

- **CoW strategy: Copy on Write strategy**
- Copy on Write means everyobe has a single shared copy of the same data until its written and then a copy is made.

- let's understand this with an example.
- So let's say you have a common file over here and there are two programs which needs to access the common file.
- So in such cases, what will happen is both of these programs will make use of a single common file. You will not have two files which will be created.
- Both the programs will use a single common file. Now let's say you have a program three Again, that program three wants to have access to the common file.
- Again, it will use that same common file. So in this way you have resources which will be shared and disk space will be saved.

- Now the question is what happens when a write is being made to this specific file?
- And in this case there is a little different strategy.
- So when a write operation happens within the cow strategy, what happens is that common file is being copied to the layer where the write is happening.
- So let's assume that program one is running on layer three of the Docker image, and Layer three is making some write changes to the common file.
- So now the common file which was shared over here, it will be copied to the layer three and then whatever write which was supposed to happen, that would happen at the layer three approach.

- **Watch video 200 of Zeal Vohra course. IMPORTANT**

- Let's say this file in layer three is of 500 MB and you are making a change to this specific file. So this file will be copied to your container layer completely.
- So the entire 500 MB gets copied to this container layer and the change would be made at this specific container layer.
- Now whenever a read operation happens, Docker storage driver will ensure that if the file exists in this container layer, it will not read the file in the below layer altogether.
- It will read the file from the container layer, which is the read and write layer over here.

- **CMDS:**
```docker INFO```

- The storage driver details will be present over here.

- **CMDS:**
```
cd /var/lib/docker
ls -ltr
```

- You will see a name with directory storage driver.

- **CMDS:**
```
cd <overlay driver>
ls -ltr
cd l
ls -ltr
docker pull ubuntu
docker images
ls -ltr
```
- On executing `ls -ltr` you will see multiple layers created over here. These directories are associated with layers.
- Typically the lowest layer will have the highest size.

- `Higher layer` is basically your `Container layer`
- `Lower layer` is basically your `Image layer`

- **Watch the video. Need to watch `video 200` multiple times**


### Block and Object storage
- **Block Storage**
- In block storage, the data is stored in terms of blocks.
- Data stored in blocks are normally read or written entirely a whole block at the same time.
- Most of the files systems are based on block devices.
- Every block has an address and application can be called via SCSI call via its address.
- There is no storage side meta-data associated with the block except the address.
- This block has no desciption, no owner.

- **Object Storage**
- Object storage is data storage architecture that manages data as objects as opposed to blocks of storage.
- An **object is defined as a data (file)** along with all its meta-data which is combined together as an object.
- This object is given an ID which is calculated from the content of the object (from the data and metadata).
- Application can then call object with the unique object ID.

- ** Difference between Object and Block Storage **
- **Object Storage: **
- Store virtually unlimited files.
- Maintain file revisions.
- HTTP(s) based inerface.
- Files are distributed in different physical nodes.

- **Block Storage;**
- Files is split and stored in fixed sized block.
- Capacity can be increased by adding more nodes.
- Suitable for application which require high OPS, database, transactional data.

### Changing Storage drivers
- The default storage driver for docker is `overlay2`. Use `docker info`

- one very important part to remember is that if we are going to change the storage driver, then
- whatever data that we might have for the previous storage driver would be inaccessible and whatever containers which were launched would be inaccessible.

- **CMDS:**
```
docker info
docker container run -dt --name overlay2 nginx
docker ps
cd /var/lib/docker
ls
cd overlay2
ls
docker ps
systemctl stop Docker
cd /etc/docker/
ls
vi daemon.json
```

- **CODE: daemon.json**
{
  "storage-driver": "aufs"
}

```
systemctl start docker`
docker info


```

- when you check the `docker info`. Under the `Storage Driver` -> aufs will be present.
- What we did was to change the default storage driver `overlay2` -> `aufs` in docker.
- so when we execute commands `docker ps` and `docker ps -a` we will not be able to see the containers that were cretaed.
- because they are made to be inaccessible.

- **CMDS:**
```
cd /var/lib/docker
ls -ltr

```
- there will be a directory of tupe `aufs`

- **CMDS:**
```
docker container run -dt --name aufs nginx

```
- Here when we run the above command, the image will be pulled because of the storage driver change.

- **CMDS:**
```
docker ps
cd aufs
ls -ltr


```

- this time the directory would be under the `aufs`.
- So you see there is a different approach in which stores the data.
- So all of the data that you might store in the container readwrite layer as well as the images would be part of the `aufs` directory here.


### Docker volumes
- **Challenges with files in Container Writeable Layer:**
- By default all files created inside a container are stored on a writable container layer. This means that:
- The data does not persist when that container no longer exist and it can be difficult
- to get the data out of the container if another process needs it.

- So docker has 2 options for container to store files in the host machine,
- so that the files are persisted after the container stops: volumes and bind mounts.
- If you are running Docker on Linux you can also use a tmpfs mount.

- You need to make sure that a container is running.
- **CMDS:**
```
docker ps
cd /var/lib/Docker
cd overlay2
ls -ltr
```
- So this specific container, whatever data that you might be writing inside the container would be stored in a specific directory.
- Now this directory, whatever container read and write layer that we were discussing.
- So this read and write layer, whatever things that you will be writing here.
- It depends upon the container lifecycle.
- As we discussed, if the container is deleted, then whatever data that you might have written through
- the read and write layer would also cease to exist.
- So let's try it out.

- **CMDS:**
```
ls -ltr
docker ps
docker stop mynginx
docker rm mynginx
ls -ltr

```

- Now it has been deleted. And if you want to recover data, there is no way to do that.
- And this is the reason why for the applications which are stateless.
- This type of architecture would not really matter.
- But if you have a stateful application, so let's say that many organizations, they host their databases
- like MySQL inside the container.
- Now they do not really want that, that the container, if it gets deleted or if there are any issues
- with the container, they would not be able to access the data.
- ** So you have to keep the data of the container separately and the container separately.That is the recommended approach.**
- So that can be achieved with the help of bind mounts as well as volumes.

- **CMDS:**
```
docker volume ls
docker volume create myvolume
docker volume ls
```

- So the volume is created.
- Now whatever container that we might launch, we can associate that containers directory with the volumes directory.

- **CMDS:**
```
docker container run -dt --name busybox -v myvolume:/etc busybox sh
docker ps
docker volume ls
docker volume inspect myvolume
cd /var/lib/docker/volumes/myvolume/_data/
ls -ltr
docker container exec -it busybox sh
cd /etc
ls -ltr
df -h
```

- So if you basically do a `df -h`, you will see that the root file system is based on overlay.
- And then you have the `etc` directory here. `etc` directory is within the volume of `/dev/vda1`.
- So this is a different disk altogether when compared to the root volume.

- **CMDS:**
```
exit
cd /var/lib/docker/volumes/myvolume/_data/
docker ps
docker stop busybox
docker rm busybox
docker ps
docker ps -a
```

- So the docker container will cease to exist. But remeber we are usig volumes,
- so our read/write data will be saved in the volume.

- So the container has been deleted. However, if you do an `ls -ltr` here you can see that the data continues to be present.
- So this data, which is present within the volume, is independent about the container lifecycle.
- So even if the container is deleted, the data still remains.

- **CMDS:**
```
ls -ltr
docker volume ls
cd 
docker volume rm myvolume
docker volume ls
```

** Important Pointers:**
- A given volume cane be mounted into multiple containers simultaneoulsy.
- When no running container is using a volume, the volume is still available to Docker and is not removed automatically.
- When you mount a volume, it may be named or anonymous. Anonymous Volumes are not given an explicit name when they are first
- mounted into a container, so docker gives them a random name that is guarenteed to be unique within a given Docker host.

### Bind Mounts
- When you use a `bind mount`, file or directory on the host machine is mounted into a container.
- The file or directory is referenced by its full or relative path on the host machine.

- Let's say you already have a directory and there are certain contents inside that directory.
- Now you want your container to be able to access the contents within the directory inside the host.
- So in such cases you can make use of bind mounts.

- ** CMDS: **
```
docker container run -dt --name nginx nginx
docker ps
docker inspect nginx
docker container exec -t nginx bash
cd /usr/share/nginx/html/
ls -ltr
```

- You will see a default `index.html`. This is the default nginx html image.

- ** CMDS: **
```
exit
mkdir index
cd index
ls -ltr
echo "Hello, this is bind mount file" > index.html
docker container run -dt --name nginxbind01 -v /root/index/:/usr/share/nginx/html nginx
```

- Here we are running a container that specifies the directory under the host (`/root/index`)
- and i also need to specify the directory under the container which is `/usr/share/nginx/html`

- ** CMDS: **
```
docker ps
docker inspect nginxbind01
curl <IP Address of the container>
docker container exec -it nginxbind01
cd /usr/share/nginx/html/
ls -ltr
```

- This is the index.html file which came from the host directory.
- The host directory is basically the root index file.

- ** CMDS: **
```
exit
pwd
```

- Whenever you are creating a bind mount there are 2 ways to do bind mount.
- **First way**
- **CMDS:**
```

docker container run -dt --name nginxbind02 --mount type=bind,source=/root/index,target=/usr/share/nginx/html nginx
docker ps
docker inspect nginxbind02
curl <Ip address of the container present inside `Networks`>
docker container exec -it nginxbind02 bash
cd /usr/share/nginx/html
ls -ltr
echo "This is my custom changes" >> index.html
cat index.html

```

- **CMDS: Read Only bind mount**

```
docker container run -dt --name nginxbind03 --mount type=bind,source=/root/index,target=/usr/share/nginx/html nginx
docker ps
docker container exec -it nginxbind03 bash
cd /usr/share/nginx/html/
ls -ltr
cat index.html
echo "This is my 3rd change" >> index.html

```

- You will get a bash prompt saying that this is read only.

### Automatically remove columen on container exist
- Whenever a container is created with -dt flag and if the main process completes/stops, then it goes into the exited stage.
- We can see list of all containers (Running/Exited) with docker ps -a command.
- Many a times, container performs a job and we want container to automatically get deleted once it exits.
- This can be achieved with the `--rm` option.

- **CMDS:**
```
docker container run -dt --name container01 busybox ping -c10 google.com
docker ps
docker ps -a
```

- So now what we want is after the command or after the script has completed its execution, we don't
- really want it to be in the exited stage because it will unnecessarily lead to disk usage.
- So what we want is that the container should be removed automatically when the job is completed.
- So in such cases you can make use of the `--rm` option.

- **CMDS:**
```
docker container run --rm -dt --name container02 busybox ping -c10 google.com
docker ps
docker ps
docker ps -a
```

- You need to wait for few seconds for the container to finish its 10 ping execution.
- so when you add `--rm` into the container creation. Once the command finish execution, the container will be deleted thus preventing disk usage.

- **CMDS:**
```
docker container run -dt --name container02 -v /testvolume busybox ping -c10 google.com
docker volume ls
docker ps -a
```
- The problem with the above approach is that the container will cease to exit after 10 pings but still the volume will persist.

- **CMDS:**
```
docker container prune -y
docker ps -a
docker volume ls

```

- In our scenario, when we go for the volume +`--rm` approach, when the container is deleted, the volume will also to be deleted.
- **CMDS:**
```
docker volume rm <volume id>
docker container run --rm -dt --name container02 -v /testvolume busybox ping -c10 google.com
docker ps
docker volume ls
docker ps -a
docker volume ls
```
- So when we use `--rm`, when the container cease to exist, because we are using `--rm` option -> the container volume will also be deleted.


### Device Mapper
- The **device mapper is a framework** provided by the **Linux kernel** for **mapping physical block devices onto higher-level virtual block devices.**
- Device mapper works by passing data from a virtual block device, which is provided by the device mapper itself, to another block.
- Device mapper creates logical devices on top of physical block device and provide features like: RAID, Encryption, Cache and various others.

- There are 2 modes for devicemapper storage driver:
- **loop-lvm mode:**
- Should only be used in testing enviroment
- Makes use of loopback mechanism which is generally on the slower side.

- **direct-lvm mode:**
- Production hosts using the devicemapper storage driver must use `direct-lvm` mode.
- This is much more faster then the loop-lvm mode.

- **CMDS:**
```
vi /etc/docker/daemon.json
```

- **CODE:**
```
{
 "storage-driver": "devicemapper"
}
```

- **CMDS:**
```
docker info | grep -i storage
docker images
docker pull busybox
docker container run -dt --name busybox busybox sh
docker ps
```
- This container and this image is associated with storage driver.
- The storage driver that we are usings is `overlay2` and we can identify it by using the command -> `docker info | grep -i storage`

- **CMDS:**
```
systemctl restart Docker
systemctl status docker
docker info
```

- You will see a warning saying that `loop-lvm` will not be used. 
- Remember `loop-lvm` is used in testing only. It is recommended to use `direct-lvm` mode.

### Docker logging drivers
- UNIX and Linux commands typically open 3 I/O streams when they run, called `STDIN`, `STDOUT` and `STDERR`
- Similary to the above statement, docker also has similar commands.

- **STDIN (Standard Input)**
- **What it is:** The input stream, usually from the keyboard or piped input.
- **Docker use:** If you want to interactively send input to a container (like entering commands or data), you can enable STDIN.
- **Example:**
```
docker run -i ubuntu cat
```

- **STDOUT (Standard Output)**
- **What it is:** The default output stream for displaying results.
- **Docker use:** Everything printed to the terminal by the container (e.g., from echo, print, console.log) goes to STDOUT.
- **Example:**
```
docker run ubuntu echo "Hello from container"
```

- **STDERR (Standard Error)**
- **What it is:** The stream used for error messages and diagnostics.
- **Docker use:** Any error messages from the container's application are sent to STDERR.
- **Example:**
```
docker run ubuntu bash -c "ls /nonexistent"
```

**CMDS:**
```
docker ps
docker container run -dt --name mybusybox busybox ping gooogle.com
docker ps
docker logs mybusybox

```

- you will able to see the output of the commands that are running.

- **CMDS:**
```
docker info
```
- Under `Log`, you have various log drivers such as `awslogs`, `fluentd`, `gcplogs`, `gelf`, `json-file` ,`none` , `syslog` , `local` ,` journald` , `splunk`.
- These are the different type of logging drivers present in docker.

- **CMDS:**
```
docker info | grep -i "logging driver"
```
- So anytime when you create a container and you do not specify a logging driver by default, 
- Docker will associate the `json-file` logging driver with your container so we can even find it out.

- **CMDS:**
```
docker ps
docker inspect mybusybox

```
- Here you will find `LogConfig: { "Type": "json-file" ...}`
- All of your log paths of the container are stored in a specific folder. Refer `LogPath`.
- This will give you the default path in which all the logs will be stored.

- **CMDS: to see all the logs present inside the log path**
```
less <copy the log path>`
docker ps
```

- **CMDS:**
```
docker container run -dt --name mycustom --log-driver none busybox ping google.com
docker ps
docker logs mycutom

```

- The docker logs command is not available for drivers other than `json-file` and `journald`.

### Creating volumes in Kubernetes
- On-disk files in a Container are ephmeral. 
- Ephemeral here means that if the container goes down, the data becomes inaccessible or if the container gets terminated the data gets lost.
- When there are multiple containers who wants to share same data, it becomes a challenge.
- One of the benefit is that it supports multiple types of volumes


- **CMDS:**
```
eksctl create cluster --name lrm-eks-clstr --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
eksctl delete cluster --name lrm-eks-clstr --region us-east-1
```

- **CODE: pod-volume.yml **
```
apiVersion: v1
kind: Pod
metadata:
  name: demopod-volume
spec:
  containers:
  - name: test-container
    image: nginx
    volumeMounts:
    - name: first-volume
      mountPath: /data
  volumes:
  - name: first-volume
    hostPath:
      path: /mydata
      type: Directory
```

- since we know that host path basically mounts a specific file or directory within the host inside a container.
- So we specify via path and the `path` is `/mydata`. And we also tell a type the `type` is `Directory`.
- So this path that we want to mount inside the container is of a directory type.
- This is the path which is present within the host where pods would be running.

- `volumeMounts` does is that it says that you take this specific volume, which is first volume.
- So you see within the volume you have `first-volume`.
- So this `volumeMounts` is associated with the `first-volume` and then you specify the mount path.
- So where exactly this directory from, the host should be mounted inside the container.

-**CMDS:**
```
mkdir /mydata
kubectl apply -f pod-volume.yml
kubectl get pods
kubectl exec -it demopod-volume bash
cd /data
touch kplabs.txt
echo "Hi" > kplabs.txt
ls
df -h
exit

```

- This directory `/data` is mounted from the host inside the container.
- when you execute `df -h` , you will see a `Mounted on` -> you will see a `/data` partition.
- So whatever files that you store inside `/data`, those files will underlyingly be stored in the `/mydata` of the host.

-**CMDS:**
```
cd /mydata
ls -ltr
cat kplabs.txt

```

### Persistent volume and Persistent volume claim:
- A **persistentVolume(PV)** is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
- Every volume which is created can be of different type.
- This can be taken care by the **storage Administrator/Ops Team**.

- A **PersistentVolumeClaim** is a request for the storage by a user.
- Within the clain, user need to specify the size of the volumen along with access mode.
- **Developer:** I want a volume of size 10 GB which is has speed of Fast for my pod.

- Storage Administrator takes care of creating PV.
- Developer can raise a `Claim` (I want a specific type of PV).
- Reference that claim within the PodSpec file.

**CMDS:**
```
vi pv.yml
```

**CODE: pv.yml -> PersistentVolume -> Storage Administrator**
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: block-pv
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /tmp/data

```

- **CMDS:**
```
mkdir /tmp/data
kubectl apply -f pv.yaml
```
- You will see the output as `persistentvolume....`

- **CMDS:**
```
kubectl get pv
```

- **CODE: pvc.yml -> PersistentVolumeClaim.yml -> Developer**
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc
spec:
  storageClassName: manual  
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

- **CMDS:**
```
kubectl apply -f pvc.yml
kubectl get pvc
kubectl get pv
```

- **CODE: pod-pvc.yml**
```
apiVersion: v1
kind: Pod
metadata:
  name: kplabs-pvc
spec:
  containers:
    - name: my-frontend
      image: nginx
      volumeMounts:
        - mountPath: "/data"
          name: my-volume
  volumes:
    - name: my-volume
      persistentVolumeClaim:
        claimName: pvc	
```

- The developer will reference the claim `pvc`

- **CMDS:**
```
kubectl apply -f pod-pvc.yml
kubectl get pods
kubectl exec -it kplabs-pvc
cd /data
ls -ltr
touch kplabs.txt
exit

```

### Volume expansion in K8s:
- It can happen that your persistent volume has become full and you need to expand the storage for additional capacity.

- Lets say you have a PV and the current capacity of your PV is 10 GB and it is completely full.
- So at the later stage you might need to provision an additional capacity on top of this.
- So this is what is referred as the `volume expansion`.

- **1. Enabling Volume Expansion in the Storage Class.**
- **Step 1:**
- If you want to go ahead and expand the volume, the first step is to enable the volume expansion.
- Now you have to make sure that a parameter of allow volume expansion is set to true Associate it with
- the storage class where your PV is associated with.

- **Step 2:**
- Now once you have this parameter configured, the second step is to go ahead and perform the resizing
- of your PVC, which is the persistent volume claim.
- So within the PVC you go ahead and resize it.

- **Step 3:**
- Once PVC object is modified, you will have to restart the POD.
- kubectl delete pod [POD-NAME]
- kubectl apply -f pod-manifest.yml

- **CODE: pod-pv.yml**
```
kind: Pod
apiVersion: v1
metadata:
  name: storage-pod
spec:
  containers:
    - name: my-frontend
      image: busybox
      command:
        - sleep
        - "36000"
      volumeMounts:
      - mountPath: "/data"
        name: my-do-volume
  volumes:
    - name: my-do-volume
      persistentVolumeClaim:
        claimName: kplabs-pvc
```
- **CODE: pvc.yml**
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: kplabs-pvc
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: do-block-storage
```
- ** External Commands Used:**
```
kubectl edit pvc kplabs-pvc
kubectl exec storage-pod -- sh
df -h
```

- **CMDS:**
```
















































































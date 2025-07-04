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

### Docker cache:
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

-**CMDS:**
docker build -t without-cache .
docker build -t with-cache .




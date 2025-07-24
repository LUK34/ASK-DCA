
### Section 2: Image Creation, Management and Registry
- **2.1**
- 1. Have thorough understanding about various Dockerfile instructions.
```
ADD, COPY, RUN, ENTRYPOINT, WORKDIR, ENV, VOLUMES, CMD, HEALTHCHECK 
```

- **2.2**
- 2. Undestand difference between ADD and COPY
- COPY allows us to copy files from local source to destination.
- ADD allows the same in addition to using URL & extraction capabilities of TAR files.

- 3. Know difference between CMD and ENTRYPOINT
- CMD can be overridden.
- ENTRYPOINT cannot be overriden.

- **2.3**
- 4. Use-cases of using ADD vs wget/curl
- Because image size matters, using ADD to fetch packages from remote URLs is strongly discouraged;
- you should use curl or wget instead.

```
ADD http://example.con/big.tar.xz/usr/src/things/
RUN tar-xlf/usr/src/things/big.tar.xz-C/usr/src/things/
RUN make-C/usr/src/things allows

RUN mkdir-p/usr/src/things\
	&&curl-SL http://example.com/big.tar.xz\
	|tar-xJC/usr/src/things\
	&&make-C/usr/src/things allows
```

- **2.4: WORKDIR **
- 5. The WORKDIR instruction sets the working directory for any RUN, CMD,ENTRYPOINT,COPY and ADD
instructions that follow it in the Dockerfile.
- The WORKDIR instruction can be used multiple times in a Dockerfile

**Sample Snippet:**
```
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```
- **Output:**
```
/a/b/c
```

- **2.5**
- 6. Understanding the format option
- Format option allows us to format the output based on various criteria that we have defined 
- with the command.

```
# docker images --format "{{.ID}}: {{.Repository}}"
778988788: aml-ssh
765543435: ubuntu
e65765787: registry.aquasec.com/enforcer
2huhi6576: nginx
6575ftfty: busybox
76tyfytf7: amazonlinux
```

- **2.6**
- 7. Understanding the filter option
- Filter option allows us to filter outpur based on condition provided.
- Sample Use-Case:
- 7.1 Show all the dangling images.
- 7.2 Show all the images created after N image.
- 7.3 Show all the container which are in the exited stage.

- **2.7**
- 8. Accessing in-secure docker registries
- By default, docker will not allow you to perform operation with an insecure registry.
- You can override by adding following stanza within the /etc/docker/daemon.json file

```
{
	"insecure-registries": ["myregistrydomain.com:5000"]
}
```

- **2.8**
- 9. Pushing an image to a private repository.
- In order to push an image to private repository, you will have to tag it with the DNS name.
- For example:
```
Registry Name:example.com
docker tag ubuntu:latest example.com/myrepo:ubuntu
```
- **2.9**
- 10. Login to a private Repository
```
docker login example.com
```

- 11. Searchability aspect of images
- docker search nginx
- docker search nginx --filter "is-official=true"

- **2.10 - Moving images across multiple hosts**
- 12. The docker save command will save one or more images to a tar archieve.
```
docker save busybox > busybox.tar
```
- 13. The docker load command will load an image from a tar archieve.
```
docker load < busybox.tar
```

- **2.11 - Container Management**
- 14. The docker commit can be used to save the current state of a container to an image.
```
docker commit c3f279s17e0a container-image:v2
```

- 15. You can use docker export command to export a container to another system as an image tar file.
```
docker export my-container > container.tar
```

- 16. When we export a container, all the resultant imported images will be flattened.

- **2.12: Dockerfile ENV**
- 17. The ENV instruction sets the enviroment variable <key> to the value <value>
```
ENV NGINX 1.2
RUN curl -SL http://example.com/web-$NGINX.tar.xz
RUN tar -xzvf web-$NGINX.tar.xz
```
- 18. You can use the -e, --env and --env-file flags to set simple enviroment variable 
- in the container you're running or overwrite variables that are defined in the Dockerfile of the image you're running.
```
docker run --env VAR1=value1 --env VAR=value2 ubuntu env | grep VAR
```

- ** 2.13: EXPOSE Instruction **
- The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime.
- The EXPOSE instruction does not actually publish the port.
- It functions as a type of documentation between the person who builds the image and the person who runs the container,
- about which ports are intended to be published.

- ** 2.14: HEALTHCHECK Instruction **
- HEALTHCHECK instruction in Docker allows us to tell the platform on how to test that our application is healthy.
- HEALTHCHECK CMD curl --fail http://localhost||exit 1
- That uses the curl command to make an HTTP request inside the container, which checks that the web app in the container does respond.
- It exits with a 0 if the response is good, or a 1 if not - which tells Docker the container is unhealthy.

- ** 2.15: Tagging Docker Images **
- Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
- **Syntax:**
docker tag SOURCE_IMAGE[:TAG]TARGET_IMAGE[:TAG]
- **Example Snippet:**
docker tag httpd fedora/httpd:version1.0

- ** 2.16: Pruning Docker Images **
- Docker image prune command allows us to clean up unused images.
- By default, the above command will only clean up dangling images.
- Dangling Images= Image without Tags and Image not referenced by any container.

- ** 2.17: Downloading Images from Private Registry **
- If your image is available on a private registry which requires login, use the --with-registry-auth flags
- with docker service create, after logging in.
```
docker login registry.example.com
docker service create --with-registry-auth\--name my_service registry.example.com/acme/my_image:latest
```
- This passes the login token from your local client to the swarm nodes where the service is deployed using the 
- encrypted WAL logs. With this information, the nodes are able to log into the registry and pull the image.

- ** 2.18: Build Cache**
- Docker creates container images using layers.
- Each command that is found in a Dockerfile creates a new layer.
- Docker uses a layer cache to optimize the process of building Docker images and make it faster.


### Section 3: Installation and Configuration
- **3.1: DTR Backup Process**
- When we backup DTR, there are certain things that are not backed up.
- User/organization backup should be taken from UCP.

- **3.2: Namespaces**
- Know what namespace are all about.
- These namespaces provide a layer of isolation. Each aspect of a container runs in a seperate
- namespace and its access is limited to that namespace.
- User namespace is not enabled by default.

- **3.3: Cgroups**
- Have a clear understanding about cgroups.
- Control Groups (cgroups) is a Linux kernel feature that limits accounts for, and isolates 
- the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes.

- **--cpus=<value>**
- This option limits the amount of CPU time a container can use, expressed as a number of cores.
- **Example:**
```
docker run --cpus="1.5" nginx
```
- This limits the container to use at most 1.5 CPU cores.
- If your system has 4 cores. Docker will allow the container to use any core, 
- but not more than 1.5 cores woth of processing time.
- **Use Case:**
- When you want to throttle CPU usage but dont care which specific cores are used.

- **--cpuset-cpus="<list>"**
- This option restricts the container to run only on specific CPU cores.
- **Example:**
```
docker run --cpuset-cpus="0,1" nginx
```
- The restricts the container to **only use CPU cores 0 and `**
- It will not be schedules on other CPU cores.
- **Use case:**
- When you want to **pin containers to specific cores** for performance tuning or CPU isolation.

- **3.4 Reservation and Limits**
- Limit is a hard limit.
- Reservation is a soft limit.

- **CMDS:**
```
docker container run -dt --name container01 --memory-reservation 250m ubuntu
docker container run -dt --name container02 -m 500m ubuntu
```
--memory and --memory-reservation

** 3.5 Immutable Tags in DTR**
- By default, users with read and write access can overwrite tags.
- To prevent tags from being overwritten, we can configure repository to be immutable.

- ** 3.6 DTR Cache**
- To decrease the time to pull an image, you can deploy DTR caches geographically closer to users.

- Caches are transparent to users, since users still log in and pull images using the DTR URL address.
- DTR checks if users are authorized to pull the image, and redirects the request to the cache.

- **3.7 DTR Garbage Collection**
- You can configure the Docker Trusted Registry(DTR) to automatically delet unused image layers, thus saving you disk space.
- This process is also known as garbage collection.

- **3.8 DTR High Availability**
- Docker Trusted Registry is designed to scale horizontally as your usage increases.
- You can add more replicas to make DTR scale to your demand and for high availability.
- If your DTR deployment has multiple replicas, for high availability, you need to ensure all replicas are using the same storage backend.

- **3.9 High Availability of UCP and DTR**
- To have high-availability on UCP and DTR, you need a minimum of:
- 3 dedicated nodes to install UCP with high availability.
- 3 decicated nodes to install DTR with high availability.
- You can monitor the status of UCP by using the web UI or the CLI. You can also use the
- `_ping` endpoint to build monitoring automation.

-**3.10 Orchestrator types in UCP**
- Docker UCP supports both Swarm and Kubernetes.
- When you install Docker Enterprise, new nodes are managed by Docker Swarm, but you can change the 
- default orchestrator to Kuberenetes in the administrator settings/
- To changes the orchestrator type for a node from Swarm to Kubernetes:
```
docker node update --label-add com.docker.ucp.orchestrator.kubernetes=true<node-id>
```

- **3.11 Storage Driver in DTR:**
- By default, Docker Trusted Registry stores images on the filesystem of the node where it is running, 
- but you should configure it to use a centralized storage backend.

- You can configure DTR to use an external storage backend, for improved performance or high availability.

- **3.12 Supported Storage Systems:**
- Some of the supported storage systems in DTR are:
- **Local:** NFS, Bind Mount, Volume
- **Cloud Storage Provide:** AWS S3, Azure, Google Cloud

- **3.13 DTR Webhooks**
- You can configure DTR to automatically post event notification to webhook URL of your choosing.

### Section 4: Networking
- You should be aware of the primary networking drivers
- ** 4.1 Bridge Network Driver: **
- Bridge is the default network driver for Docker.

- ** 4.2. Host Network:**
- This driver removes the networkd isolation between the docker host and the docker containers to use the host's networking directly.

- ** 4.3. User defined bridge and Legacy Link Approach:**
- Remember that --link is a legacy approach and should not be used.

- ** 4.4 Overlay Network:**
- Default swarm
- Allows conatiners across host to communicate with each other.
- Communication can be encrypted with --opt encrypted option.
- Do not confuse, -o is same as --opt

- You dont need to create the overlay network on the other nodes, because it will be
- automatically created when one of those nodes start running a service task which requires it.

- ** 4.5 Know the differnece between publish list (-p) and publish all (-P)**
-  Publish List (-p) will publish list of ports that you define. [-p 80:80]
- Publish all(-P) will assign random ports for all exposed ports of the container.
- -P will map the container port to randome port above 32768.

- ** 4.6 None Network:**
- If you want to completely disable the networking stack on a container, you can use the none network.
- This mode will not configure any IP for the container and doesn't have any access to the external network as well as for other containers.

- ** 4.7 Configuring docker for external DNS:**
- Docker Conatiner's DNS configuration is taken from the host's /etc/resolv.conf
- DNS settings for containers be customized via daemon.json.file.
- **CMDs:**
```
{
	"dns":["8.8.8.8", "172.31.0.2"]
}
```

- ** 4.8 Inspecting Network Details:**
- To fetch information about a specific network, you can run the following command:
- **CMDS:**
```
docker network inspect <network-name>
```

### Section 5: Security
- **5.1**
- Client bundles are group of certificate and files downloaded from UCP.
- Depending on the permission associated with the user, you can now execute
- docker swarm commands from your remote machine that take effect on the remote cluster.

- **5.2: Docker Content Trust**
- Used for interacting with only trusted images (signed images)
- Enabled with **export DOCKER_CONTENT_TRUST=1**
- **Example Dockerfile:**
```
FROM myubuntu:latest
RUN apt-get install net-tools
CMD["bash"]
```
- Engine Signature Verification prevents the following:
- docker container run of an unsigned or altered image.
- docker pull of an unsigned or altered image.
- docker build where the FROM image is not signed or is not scratch.

- **5.3 Docker Secrets**
- To create a new secret, docker secret create command can be used.
- This is a cluster management command, and must be execute on a swarm manager node.
- Remember that you annot update or rename secret.


























































### Section 4:

































































 


























### Section 3: Networking



### Section 4: Orchestration



### Section 5: Installation and Configuration of Docker EE



### Section 6: Security



### Section 7: Storage and Volumes




---
title: "Docker Containers"
excerpt: "Introduction and basic of docker containers."
collection: docker
permalink: /docker/docker-containers/
date: 2024-11-08
---

### Understanding containers
#### What is a container? 
A container is an instance of [Docker Images](https://afloresep.github.io/docker/docker-images/) running as a process. We can have many containers running off the same image. Containers leverage features like namespaces, cgroups, and UFS to provide consistent, and efficient runtime environment. 


### **Working with Containers**
#### Creating and running containers.
> 3 mayor ways to run containers
> - Locally (Docker Desktop, RD) `docker run -d -p 8080:80 nginx`
> - Servers (Docker Engine, K8s) `docker run -d -p 8080:80 --name my_nginx nginx`
> - PaaS (Cloud Run, Fargate). 

#### Detached vs. foreground modes.
- **Detached Mode (-d):** The container runs in the background, allowing the terminal to be used for other commands.
```bash
docker run -d nginx
```

- **Foreground Mode**: The container process runs attached to the terminal session, showing its logs and output.
```bash
docker run nginx
```

#### **Executing Commands Inside Containers**
To interact with a running container and execute commands:
```bash
docker exec -it <container_name> <command>
```
For example:
```bash
docker exec -it my_nginx bash
```
This command opens a shell inside the `my_nginx` container.

```bash
docker exec -it my_db mysql
```

### **Container Lifecycle**

#### **Starting, Stopping, and Restarting Containers**
- **Start a container**:
  ```bash
  docker start <container_name>
  ```
- **Stop a container**:
  ```bash
  docker stop <container_name>
  ```
- **Restart a container**:
  ```bash
  docker restart <container_name>
  ```

#### **Pausing and Unpausing Containers**
- **Pause a container**:
  ```bash
  docker pause <container_name>
  ```
  This command suspends all processes within the container.
- **Unpause a container**:
  ```bash
  docker unpause <container_name>
  ```

#### **Removing Containers**
To remove a container after it has been stopped:
```bash
docker rm <container_name>
```
To remove all stopped containers:
```bash
docker container prune
```

### **Container Isolation and Security**

#### **Namespaces**
Namespaces provide process and resource isolation for containers, ensuring that containers operate in their own environments. Types of namespaces include:
- **PID Namespace**: Isolates process IDs.
- **Network Namespace**: Isolates network interfaces.
- **Mount Namespace**: Isolates the filesystem.

#### **Control Groups (cgroups)**
Cgroups limit and allocate resources like CPU, memory, and disk I/O to ensure that a single container cannot overwhelm the host system. You can define limits using flags like:
```bash
docker run -m 512m --cpus="1.0" nginx
```
This command limits the container to 512MB of memory and 1 CPU core.

#### **Capabilities and Security Best Practices**
- **Capabilities**: Docker provides a set of capabilities that can be restricted to limit the privileges of a container. By default, Docker drops many capabilities to reduce the attack surface. You can drop additional capabilities using:
  ```bash
  docker run --cap-drop NET_RAW nginx
  ```
- **Security Best Practices**:
  - Run containers as non-root users.
  - Use read-only file systems for containers.
  - Avoid using `--privileged` mode unless necessary.
  - Use secure images from trusted sources.


---
### Docker Container Commands

#### **Running a Container**
To understand what happens in the background when you run a container, see [[docker container run]].

**Example 1: Usage**
```bash
docker container run -d -p 3306:3306 --network my_app_net --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```

- `-d`: Detached mode, runs in the background so you can continue using the terminal.
- `-p HOST:CONTAINER`: Publishes container port (3306) on the host machine port (3306).
- `--name <NAME>`: Assigns a custom name to the container (`db`). If omitted, a random name is assigned.
- `-e MYSQL_RANDOM_ROOT_PASSWORD=yes`: Passes an environment variable (`MYSQL_RANDOM_ROOT_PASSWORD`).
- `--network` will add the container to a specific network 
- `mysql`: The image to use for the container.

**Example 2: Running a Web Server**
```bash
docker container run --publish 80:80 nginx
```

- Downloads the `nginx` image from Docker Hub (if not already available locally).
- Starts a new container from the `nginx` image.
- Opens port 80 on the host machine and routes traffic to port 80 inside the container.
- Adding `-d` runs the container in the background and returns control to the shell prompt.

#### **Interactive Shell in Containers**
- **Start a Container with an Interactive Shell:**
  ```bash
  docker container run -it <IMAGE> bash
  ```
  - `-i`: Interactive mode (keeps STDIN open).
  - `-t`: Allocates a pseudo-TTY.
  - `-it`: Combines interactive and TTY, creating a terminal session inside the container (similar to SSH).

- **Execute a Command in an Existing Container:**
  ```bash
  docker container exec -it <CONTAINER_NAME> bash
  ```
  - Runs an additional command (e.g., `bash`) inside an already running container.
#### DNS Naming
`docker container run -d --net <NETWORK> --net-alias <ALIAS>`
- Creates an alias. Then if we do `nslookup <ALIAS>` we would be able to find all of the containers with alias `<ALIAS>`. 
#### **Listing Containers**
- **List Active Containers:**
  ```bash
  docker container ls
  ```
- **List All Containers (Including Stopped):**
  ```bash
  docker container ls -a
  ```

#### **Stopping and Removing Containers**
- **Stop a Running Container:**
  ```bash
  docker container stop <CONTAINER_NAME>
  ```
  - Use tab completion for available container names.

- **Remove a Stopped Container:**
  ```bash
  docker container rm <CONTAINER_NAME>
  ```
  Containers must be stopped before removing. Use `-f` to force removal:
    ```bash
    docker container rm -f <CONTAINER_NAME>
    ```

#### **Inspecting Containers**
- **View Running Processes Inside a Container:**
  ```bash
  docker container top <CONTAINER_NAME>
  ```

- **Inspect Container Metadata:**
  ```bash
  docker container inspect <CONTAINER_NAME>
  ```
  - Displays a JSON file with detailed information about the container (startup configuration, volumes, networking, etc.).

- **Monitor Resource Usage:**
  ```bash
  docker container stats
  ```
  - Provides a real-time view of container resource usage (CPU, memory, network I/O), similar to `htop`.

#### **COPYING FILES TO AND FROM CONTAINERS**
- **Copy a file from the host to the container**:
  ```bash
  docker cp <file_path> <container_name>:<container_path>
  ```
- **Copy a file from the container to the host**:
  ```bash
  docker cp <container_name>:<container_path> <host_path>
  ```

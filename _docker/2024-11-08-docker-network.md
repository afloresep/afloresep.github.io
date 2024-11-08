---
title: "Docker Networks"
excerpt: "Introduction and basic of docker networks."
collection: docker
permalink: /docker/docker-network/
date: 2024-11-08
---

### What is a Docker Network?
 A **Docker network** is a virtual network that allows containers to communicate with each other, the host machine, and external networks. Docker creates several types of networks to provide different levels of isolation and connectivity. By default (**Bridge Network**) allows containers on the same host to communicate with each other. Containers get their own private IP addresses. 

### How does `docker container run -p HOST:CONTAINER`work?
The `-p`option in the `docker run`is ussed for **port mapping**, allowing to expose a container's port on the hots machine. 
HOST port: The port on your host machine that will be accessible externally
CONTAINER: The port inside the container where the application is listening. 

### How Network Packets Move Around Docker

When a packet is sent to a containerized application:
1. The packet arrives at the host machine’s network interface.
2. If it matches a port mapping rule (e.g., `-p 8080:80`), the packet is forwarded to the container’s IP address and port (`80` in this case).
3. The container’s application processes the packet and sends a response back through the same route.

Docker uses **network namespaces** to provide isolation, ensuring that containers have their own separate network stack, including IP addresses, routes, and firewall rules.

### How Are IP Addresses Related to This?
Each container in Docker is assigned a unique **IP address** within its network (e.g., bridge network). This IP address is used for communication between containers or with the host. For example:

- In a **bridge network**, containers are assigned private IP addresses (e.g., `172.17.0.2`).
- In a **host network**, containers share the IP address of the host machine.

Docker uses internal DNS to resolve container names to IP addresses, allowing containers to communicate using names instead of IPs. This makes networking more flexible and robust, as IP addresses can change when containers are restarted.

### DNS Naming
> Containers shouldn't rely on IP's for inter-communication as IP addresses are very dynamic and we simply could not find them.

Docker uses the container names as the equivalent of a host name for containers talking to each other. 

Let's say I create a new bridge network that is named 'my_app_net' and has a container 'nginx_cont' in it.  If I were to create a second container on that virtual network, they'll be able to find each other, regardless of what the IP address is, with the container names. This is thanks to automatic DNS resolution.

**EXAMPLE**
`docker container run -d --name my_example --network my_app_net nginx`
Now we have 'my_example' and 'nginx_cont' containers in this network that can interact with each other by just using the name
`docker container exec -it my_nginx ping nginx_cont` will work. 

This **DOES NOT WORK ON DEFAULT NETWORK**. In order for this to work we would have to use `--link` to enable DNS on **default** bridge network. That is why it is much easier to just create a new network.


### Docker Network Commands

####  **Listing Networks**
- **Show All Networks:**
  ```bash
  docker network ls
  ```
  - Lists all existing Docker networks (default and user-created).

#### **Inspecting Networks**
- **View Network Details:**
  ```bash
  docker network inspect <NETWORK_NAME>
  ```
  - Displays metadata (JSON format) about the specified network, including connected containers, IP address configuration, and driver information.

#### **Creating a Network**
- **Create a New Network:**
  ```bash
  docker network create --driver <DRIVER_NAME> <NETWORK_NAME>
  ```
  - `--driver`: Specifies the network driver (e.g., `bridge`, `overlay`).
  - Creates a new virtual network that containers can connect to.

#### **Connecting and Disconnecting Containers**
- **Connect a Container to a Network:**
  ```bash
  docker network connect <NETWORK_NAME> <CONTAINER_NAME>
  ```
  - Attaches an existing virtual network to a specified container.

  **Example:**
  ```bash
  docker network connect 01ce8cb64a32 d78dd2666975
  ```
  - Connects container `d78dd2666975` to network `01ce8cb64a32`.

- **Disconnect a Container from a Network:**
  ```bash
  docker network disconnect <NETWORK_NAME> <CONTAINER_NAME>
  ```
  - Detaches the specified network from a container.

  **Example:**
  ```bash
  docker network disconnect 01ce8cb64a32 d78dd2666975
  ```
  - Disconnects container `d78dd2666975` from network `01ce8cb64a32`.

#### **Default Network Creation**
- **Create a Default Bridge Network:**
  ```bash
  docker network create <NETWORK_NAME>
  ```
  - Spawns a new virtual network using the default `bridge` driver.
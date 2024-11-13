---
title: "Docker Swarm mode"
excerpt: "Swarm mode, manager and worker nodes introduction."
collection: docker
permalink: /docker/swarm-mode/
date: 2024-11-08
---

Docker Swarm Mode introduces a powerful way to orchestrate containerized applications using a cluster of Docker hosts, abstracting them as a single "swarm." This mode enables developers to deploy, manage, and scale services efficiently across multiple hosts with minimal configuration.

At its core Swarm is actually a server clustering solution that brings together different operating systems or hosts, or nodes etc, into a single manageable unit that you can then orchestrate the life cycle of your containers in. 

### Manager Nodes and Raft Consensus
In Swarm Mode, **Manager Nodes** orchestrate and manage the entire cluster. They contain a special database called the **Raft Database**, which stores the cluster state, configurations, and information about services and tasks. This Raft Database is replicated across all manager nodes, providing high availability and fault tolerance. The manager nodes achieve consensus using the **Raft Protocol**, ensuring that: 
- Data consistency is maintained across nodes
- All managers have the same view of the cluster state
- Decisions made by the managers are reliable, even if some node fail. 

<img width="520" alt="image" src="https://github.com/user-attachments/assets/5b66bc73-3eb2-46f2-a966-90a0a6a1162e">

<img width="509" alt="image" src="https://github.com/user-attachments/assets/6641f3a3-9b6d-4031-8b9a-fb37a7d10898">

### **Worker Nodes and Control Plane Communication**
In the image, the blue boxes represent the manager nodes, which coordinate tasks and maintain the swarm's state. 
The green boxes represent the **Worker Nodes**, which are responsible for executing tasks assigned by the managers. They don’t hold a copy of the Raft Database and rely on managers for receiving tasks and updates.

- The communication between managers and workers happens over the **Control Plane**. This plane handles all instructions, scheduling, and state updates.
- Workers continuously poll the managers for tasks and provide feedback on the current state of their running containers.

A node can function as both a manager and a worker. You can promote or demote nodes between these roles depending on your needs. For example, if you need more management capacity, you can promote a worker to a manager role.

#### 3. **Services, Tasks, and Containers**

In traditional Docker usage, running containers was typically handled with the `docker run` command, which launched a single container instance. Swarm Mode introduces the concept of **Services**, which replace the `docker run` command with `docker service create`. This new command allows us to manage and scale our containers effectively using:

- **Services:** A service defines the desired state, such as the image to use, the number of replicas, and network configurations.
- **Tasks:** Each replica in a service is represented as a task. A task is the atomic scheduling unit in Swarm Mode.
- **Containers:** A task, when executed, results in the creation of a container on a worker node.

In the example shown in the image, we created a service for Nginx with three replicas using `docker service create`. The Swarm Manager determines where to place each replica based on the current load and availability of nodes, striving for balanced distribution.

#### 4. **Swarm API and Orchestration Components**
<img width="507" alt="image" src="https://github.com/user-attachments/assets/e2ad7d09-2380-4043-8cb5-df60e0d4f3c5">

Swarm Mode isn’t just a simple wrapper around existing Docker APIs. It introduces a **new Swarm API** with a collection of background services that facilitate robust orchestration:

- **Scheduler:** Decides which nodes should run which tasks based on resource availability.
- **Dispatcher:** Assigns tasks to worker nodes and tracks their status.
- **Allocator:** Manages resource allocation, including networking and storage, for tasks.
- **Orchestrator:** Ensures that the cluster state matches the desired state specified in your service definitions. It handles reconciling differences by adding, removing, or updating tasks as needed.d

### Commands
If we want to check whether the swarm is active or not -by default is not- we can use the command `docker info`. 
`docker swarm init` Initializes the swarm.

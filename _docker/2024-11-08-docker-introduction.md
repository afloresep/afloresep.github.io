---
title: "Introduction to Docker"
excerpt: "Introduction and basic of docker."
collection: docker
permalink: /docker/docker-introduction/
date: 2024-11-08
---

### **What is Docker?**
#### Overview of containerization.
Containerization is a lightweight form of virtualization that allows applications to run in isolated environments called **containers**. Unlike traditional virtual machines (VMs), containers share the host system's [[kernel]] but have their own filesystem, network interface, and process space. This makes them much more efficient in terms of resource usage, startup time, and scalability. 

Containers are built from **images**, which are templates that include everything needed to run an application: code, runtime, system tools, libraries, and configurations. They ensure that the application will behave the same way regardless of where it is deployed.

#### Benefits of using Docker.
> 1. Better isolation in a single OS
> 2. Reduced environment variances 
> 3. Increasing speed of change (develop faster, deploy server faster...)
> It help us **reduce** the amount of **infrastructure** to manage while also **ensuring** **isolation** of our workload so that things don't become hard to manage. 

#### **Comparison with virtual machines.**
Instead of spreading out all VMs with unnecessary numbers of OSs, what containers do is they give us the isolation, like a VM. It's not quite the same, but we'll just argue for now that it's an isolation layer, where they get **their own IP address**, they get their **own file system**, they get their own process space.And so it seems like almost a full VM, but you don't need all the different operating systems so we can reduce our operating system count. Our physical host count. You no longer have to use VMs as the layer of isolation. Instead of using SSH, you would use theDocker CLI . Or if you move on to Kubernetes, you could use the Kubernetes CLI and your applications are all isolated in their own containers.This allows you many benefits, including running multiple versions of the same app on the same system, without any conflict.

### **Docker Architecture**
#### Client-server model.
- The Docker Client is the main user interface for Docker. It communicates with the Docker Daemon using commands like `docker build`, `docker pull`, and `docker run`. 
- Users interact with the Docker Client via the [[Docker CLI]] or API to manage containers and images.

#### Interaction between[[ Docker CLI]] and [[Docker Daemon]]
The interaction between the Docker CLI and Docker Daemon follows a client-server model, where the CLI sends requests and the Daemon processes them.


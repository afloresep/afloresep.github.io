---
title: "Docker Images"
excerpt: "Introduction and basic of docker iamges."
collection: docker
permalink: /docker/docker-images/
date: 2024-11-08
---
### Understanding Docker Images

#### What are Docker images?
> An image is the application binaries (files containing pre-copiled binary code that is machine readable and ready to run without the compiler) and dependencies for your app and the metadata on how to run it. 

Inside this image, there's NOT actually a complete OS. There's NO kernel. It's really just the binaries that your application needs because the hots provides the kernel. 

In other words, an Image is the application we want to run. In contrast to a [[Docker Containers]] which is an instance of that image running as a process. 
Docker's default image "registry" is called *Docker Hub*

An image is the **application binaries and dependencies**
#### Image layers and the Union File System.
#### Base images vs. derived images.

### **Managing Docker Images**
- Pulling images from Docker Hub.
- Listing and removing local images.
- Tagging images.

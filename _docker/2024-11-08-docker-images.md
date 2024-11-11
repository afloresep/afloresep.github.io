---
title: "Docker Images"
excerpt: "Introduction and basic of docker iamges."
collection: docker
permalink: /docker/docker-images/
date: 2024-11-08
---

### **Understanding Docker Images**

#### What are Docker images?
An image is the **application binaries and dependencies** and metadata associated with that

> An image is the application binaries (files containing pre-compiled binary code that is machine readable and ready to run without the compiler) and dependencies for your app and the **metadata** on how to run it. 

Inside this image, there's NOT actually a complete OS. There's NO kernel. It's really just the binaries that your application needs because the hots provides the kernel. 

In other words, an Image is the application we want to run. In contrast to a [Docker Containers](https://afloresep.github.io/docker/docker-containers/) which is an instance of that image running as a process. 
Docker's default image "registry" is called *Docker Hub*

#### Image layers and the Union File System.
**What is a Layer?**
In Docker, an image is built up from a series of **layers**, each representing a snapshot of the files system after the execution of a command in the Dockerfile (like `FROM`, `RUN` etc) each resulting in a new layer. These layers are stacked on top of each other to form the final image. 

**Union File System**
Docker leverages a **Union File System** to manage these layers efficiently. A UFS allows layers to be overlaid in such a way that they appear as a single filesystem. This provides two benefits:
1. **Layering and Caching**. Because each layer is a separate filesystem, Docker can cache these layers and reuse them across different images and containers, saving time and space. 
2. **Copy-on-Write**: When a container modifies a file from a lower (read-only) layer, the file is copied to the topmost writable layer. 

> Each layer is uniquely identified and only stored once on a host which saves storage space on host and transfer time on push/pull

**How layers save space**
Using layers saves space because: 
1. **Reusability**: Common layers can be shared between multiple images. For example layer of Apt (to install dependencies) or Debian Jessie can be used in different images. Docker stores only one copy of the base layer
2. **Efficiency**. Since layers are immutable and stored only once, even if multiple images or containers use them, they occupy disk space only once. 
3. **Caching**. When rebuilding images, Docker caches unchanged layers, speeding up the build process and avoiding unnecessary duplication. 

**Layers and Containers: How do they use them**

> When you run a container from an image, Docker adds a thin, writable layer on top of the read-only image layers. This is known as the **container layer**

The relationship is as follows:

- **Image Layers**: Form the read-only base of the container's filesystem.
- **Container Layer**: Captures any changes made during the container's runtime, such as modified files, installed applications, or generated data.

#### Base images vs. derived images.

**What is a Base Image?**
A **base image** is the starting point for building Docker images. It is an image that does not have a parent image and is often specified in the `FROM` instruction at the beginning of a Dockerfile. Base images provide the foundational layers upon which other images are built.

**What is a Derived Image?**
A **derived image** is any Docker image that builds upon a base image by adding additional layers. These layers could include application code, dependencies, configurations, or any other modifications required for your application to run

### **Managing Docker Images**
#### Pulling images from Docker Hub.
To pull an image from Docker Hub, use the following command: 

```bash
docker pull <repository>/<image_name>:<tag>
```

> Note: Only official images will not have `repository/image_name` and will have just `image_name`. For example `nginx` is the official repository while `afloresep/nginx` is not

#### Listing and removing local images.

- **List all Docker images:**

```bash
docker images
```
This command will show a list of all images presented locally, including their repository, tag and image ID

- **Remove a Docker image**

```bash
docker image rm <image_id>
```

#### Image Tags 
An image tag is a pointer to a specific image commit (Image ID) that helps identify the version of the image or other metadata. If we pull two different image tags that both point to the same Image ID it will not download the images twice as they're the same. Also, note that 'latest' tag is the default and not necessarily the last version.  
Basic format for a Docker image tag is: `repository/image_name:tag`

### Dockerfile
A **dockerfile** is a text file that contains a set of instructions used to create a Docker image. Each instruction in a Dockerfile builds up the image layer by layer, defining its structure and behaviour. 

#### Basic Dockerfile Structure

```dockerfile
# 1. Base Image. Defines base image to use. 
FROM debian:jessie

# 2. Maintainer (Optional). Provides metadata about the image (e.g. author)
LABEL maintainer="your_email@example.com"

# 3. Set Working Directory. Thisis where subsequent commands like COPY are executed
WORKDIR /usr/src/app

# 4. Copy Files from host machine to the container
COPY package.json ./
COPY . .

# 5. Install Dependencies. Each instruction creates a new layer
RUN apt-get update && apt-get install -y\
	curl \
	vim \
	&& rm -rf /var/lib/apt/lists/*

RUN pip install -r requirements.txt

# 6. Expose Ports
EXPOSE 3000 8800 

# 7. Set Environment Variables
ENV NODE_ENV=production

# 8. Run the Application. Always last line. Specifies default command to run when a container is started. Only one CMD is allowed per Dockerfile. 
CMD ["npm", "start"]
```

#### Build 

```bash
docker image build -t <NAME <PATH> 
```

#### Extending Official Images
Example with nginx. 

```dockerfile
FROM nginx:latest

# chagen working directory to root of nginx webhost
WORKDIR /usr/share/nhinx/html 

COPY my_new_index.html index.html
```

`COPY` is the stanza we will be using to copy our source code from our local machine, or build servers, into our container images. In this case we're just taking our simple `my_index.html` and we're overwriting the file in the nginx default directory so that it's **our custom home page** for the web server. 

> Note: We're missing required stanzas like CMD, but that's okay because it's inheriting from the `FROM` command 

### **Commands**

 **INSPECTING LAYERS**
- **View Image History**:

  ```bash
  docker image history <image>
  ```

  This command displays the history of an image, showing the sequence of layers and the commands that created them.

- **Inspect Image Metadata**:

  ```bash
  docker image inspect <image>
  ```

  This command shows detailed metadata of an image in JSON format, including information like image size, configuration, and layers.

 **TAGS**
- **Tag an Image**:

  ```bash
  docker image tag <source_image[:tag]> <new_image[:tag]>
  ```

  For example:

  ```bash
  docker image tag nginx afloresep/nginx
  ```

  This command tags the `nginx` image with a new tag, allowing you to push it to a Docker Hub repository.

**PUSHING IMAGES TO DOCKER HUB**
To push an image to Docker Hub, ensure you are logged in using `docker login`, then use:

```bash
docker push <repository>/<image_name>:<tag>
```

For example:

```bash
docker push afloresep/nginx:latest
```

**PULLING AN IMAGE**
To pull a specific image version:
```bash
docker pull ubuntu:22.04
```

**REMOVING UNUSED IMAGES**
To clean up dangling images (those not tagged and not referenced by any container):
```bash
docker image prune
```

**SHOWING IMAGE SIZE**
To display the disk space usage of your images:
```bash
docker image ls --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

 **EXPORTING AND IMPORTING IMAGES**
- **Export an image to a tar file**:
  ```bash
  docker save -o <filename>.tar <image>
  ```
- **Import an image from a tar file**:
  ```bash
  docker load -i <filename>.tar
  ```

**RUNNING A CONTAINER FROM AN IMAGE**
To start a container using an image:
```bash
docker run -d -p 80:80 --name my_nginx nginx
```

**SEARCHING FOR IMAGES ON DOCKER HUB**
To search for an image on Docker Hub:
```bash
docker search <image_name>
```

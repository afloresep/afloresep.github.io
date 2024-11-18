---
title: "Docker Registry"
excerpt: "Docker Registry."
collection: docker
permalink: /docker/docker-registry/
date: 2024-11-18
---

## **What is a Docker Registry?**
A **Docker registry** is a server-side application that stores and distributes Docker images. It acts as a centralized location where Docker clients (like Docker Engine or Docker CLI) can **push**, **pull**, and **manage** container images.

### **Key Concepts:**
- **Docker Hub**: The default public registry for Docker images.
- **Private Registry**: A custom, self-hosted registry for internal use, allowing secure storage and distribution of images within your organization.

## **How to Use Docker Registries**
Using a Docker registry involves **tagging**, **pushing**, and **pulling** images:

1. **Tagging**: Assign a tag to an image that includes the registry URL (e.g., `127.0.0.1:5000/hello-world`).
2. **Pushing**: Upload the tagged image to the specified registry.
3. **Pulling**: Download the image from the registry to your local Docker environment.

**Example Flow:**
- Create a local registry.
- Tag an existing image with the local registry URL.
- Push the image to the local registry.
- Pull the image back from the registry to verify the process.

---

## **Security: Need for TLS and Authentication**
Securing your Docker registry is crucial, especially if it’s exposed to the internet or used in a production environment.

1. **TLS (Transport Layer Security)**:
   - By default, Docker requires the registry to use **HTTPS**. This ensures that the communication between the client and the registry is encrypted, preventing eavesdropping and man-in-the-middle attacks.
   - For a **self-hosted registry**, you can use **self-signed certificates** or obtain certificates from a trusted Certificate Authority (CA).

2. **Authentication and Authorization**:
   - Enable **basic authentication** to restrict access to the registry using a username and password.
   - Use a tool like **htpasswd** to generate user credentials.
   - You can integrate the registry with an **OAuth provider** or use **Docker's authentication plugins** for advanced setups.

**Configuration Tip**: Set up an `auth` section in the registry's configuration file (`config.yml`) to enable authentication and authorization.

---

## **Webhooks: Trigger Automatic Builds**
Docker registries can send **webhooks** to external services like **Jenkins** or **GitHub Actions** when certain events occur (e.g., an image is pushed). This can trigger automatic CI/CD pipelines to:
- **Test** the new image.
- **Deploy** the updated image to a staging or production environment.

**Example Use Case**:
- A new image is pushed to the registry.
- The registry sends a webhook to a Jenkins server.
- Jenkins pulls the new image and runs automated tests or triggers a deployment.

**Configuration**: Define webhooks in the registry's configuration file or use the **UI of services** like Docker Hub, GitHub, or GitLab to set them up.

---

## **Docker Commands for Working with a Local Registry**

### **1. Run the Registry Container**
Start a local Docker registry using the official `registry` image:
```bash
docker container run -d -p 5000:5000 --name registry registry
```
- **`-d`**: Run the container in detached mode.
- **`-p 5000:5000`**: Expose the registry on port 5000.
- **`--name registry`**: Name the container "registry".

### **2. Tag and Push an Image to the Registry**
Re-tag an existing image (e.g., `hello-world`) to include the registry URL and push it to your local registry:
```bash
docker tag hello-world 127.0.0.1:5000/hello-world
docker push 127.0.0.1:5000/hello-world
```
- **`docker tag`**: Assigns a new tag to the image, specifying the registry URL.
- **`docker push`**: Uploads the image to the specified registry.

### **3. Remove the Image Locally and Pull It from the Registry**
To verify that the image is stored in the registry, remove it from your local Docker cache and then pull it back:
```bash
docker image remove hello-world
docker image remove 127.0.0.1:5000/hello-world
docker pull 127.0.0.1:5000/hello-world
```
- **`docker image remove`**: Deletes the specified image from your local system.
- **`docker pull`**: Downloads the image from the registry.

### **4. Re-create the Registry Using a Bind Mount for Data Persistence**
To ensure your registry's data is persistent across restarts, use a **bind mount** to store the image data on your local filesystem:
```bash
docker container run -d -p 5000:5000 --name registry -v $(pwd)/registry-data:/var/lib/registry registry
```
- **`-v $(pwd)/registry-data:/var/lib/registry`**: Mounts a local directory (`registry-data`) to the registry's data storage path (`/var/lib/registry`).
- This setup allows the registry to persist images even if the container is stopped or removed.

### **5. List Images Stored in the Registry**
You can use the **Docker Registry HTTP API** to list the images stored in your local registry:
```bash
curl http://127.0.0.1:5000/v2/_catalog
```
- This command returns a list of repositories stored in the registry.

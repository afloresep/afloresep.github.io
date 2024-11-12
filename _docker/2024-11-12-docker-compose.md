---
title: "Docker Compose"
excerpt: "Docker compose and docker-compose.yml file introduction."
collection: docker
permalink: /docker/docker-compose/
date: 2024-11-08
---

## Docker compose: A Tool for Multi-Container Applications
Docker Compose is a powerful tool that combines a command-line interface (CLI) and a configuration file (`docker-compose.yml`). It allows developers to define, configure, and manage multi-container applications that are often composed of several services (e.g., frontend, backend, databases) in a way that’s easy to reproduce and control.

**Why Docker Compose?** Most software services rely on multiple components (e.g., a web server, database, cache) that need to interact seamlessly. Instead of manually creating each container with numerous `docker run` options, Docker Compose lets you define all container configurations and relationships in one file. With a single command, you can spin up (start) or tear down (stop) your entire multi-container application environment.

> Note: Older versions of Docker use `docker-compose <command>`, but since 2022 `docker compose <command>` works. 

## `docker-compose.yml`

The `docker-compose.yml` file is a configuration file written in YAML syntax. This file describes the containers, networks, and volumes that make up your application.

A simple template for a `yml` file could be: 
```yml
version: '3.1' # Specifies the Compose file format version (use the latest for newer features)

services: # Main section for defining your application’s containers (equivalent to individual docker run commands)
  servicename: # Service names act as the DNS within the network for inter-container communication
    image: # (Optional) The image to pull or build
    build: # (Optional) Specify a build context to build an image from a Dockerfile
    command: # (Optional) Override the default CMD specified by the image
    environment: # (Optional) Pass environment variables to the container
    ports: # (Optional) Map host to container ports
    volumes: # (Optional) Mount host directories or Docker volumes

volumes: # (Optional) Define Docker volumes to share data between containers or persist data

networks: # (Optional) Define custom networks for service isolation or inter-service communication

```
- **services**: Defines individual containers (e.g., `drupal`, `postgres`) and their configurations.
- **image**: The Docker image for the container (can also be replaced with `build` to build an image locally).
- **environment**: Passes environment variables (e.g., `POSTGRES_PASSWORD`) to the container for configuration.
- **volumes**: Allows data to persist beyond the lifecycle of containers by mapping to host directories or Docker-managed volumes.
- **ports**: Exposes container ports to the host machine. Here, `8080:80` means users can access Drupal by visiting `localhost:8080`.

**EXAMPLE OF A DOCKER COMPOSE FILE**
```yml

version: '3.1'

services:
 drupal: 
  image: drupal
  ports: 
	 - "8080:80"
  volumes: 
	 - drupal-modules:/var/www/html/modules
	 - drupal-profiles:/var/www/html/profiles
	 - drupal-themes:/var/www/html/themes

 postgres:
  image: postgres
  environment:
	- POSTGRES_PASSWORD=mypassw

volumes:
 drupal-modules:
 drupal-profiles:
 drupal-themes
```
Let’s enhance these notes with some added clarity, definitions, and examples to make Docker Compose and its components easier to understand. I’ll add explanations for terms, structure, and potential usage scenarios.

## **Docker Compose CLI Commands**

The Docker Compose CLI tool, which comes pre-installed with Docker for Windows and Mac (and available as a separate installation on Linux), provides commands to manage your multi-container environments.

### Common Commands

- **`docker compose up`**: Creates and starts containers, networks, and volumes as defined in the `docker-compose.yml` file. Use `-d` to run in detached mode (background).
- **`docker compose down`**: Stops and removes containers, networks, and volumes that were created by `up`. This makes it easy to clean up your environment after development or testing.

### Example Workflow with Docker Compose CLI

1. **Start Services**:
   ```bash
   docker compose up -d
   ```
   This command will start all services in detached mode, allowing the terminal to remain free for other commands.

2. **Stop Services**:
   ```bash
   docker compose down
   ```
   This stops and removes the containers, networks, and volumes defined in the `docker-compose.yml` file, keeping the environment clean.

---

## **Building Images with Docker Compose**

In addition to pulling pre-built images, Docker Compose allows you to build images directly from a Dockerfile. This is useful if you need a custom configuration or wish to install additional dependencies.

### Example of Adding Image Building to Compose

```yml
version: '3.1'

services:
  custom-app:
    build: # Specifies build configuration instead of pulling an image
      context: . # Path to the directory containing the Dockerfile
      dockerfile: Dockerfile # Optional if the Dockerfile is named `Dockerfile`
    ports:
      - "5000:5000"
    environment:
      - APP_ENV=development
```

In this example:
- **`build`**: Specifies the context where Docker can find the Dockerfile and any dependencies it needs.
- The `custom-app` service will be built from the Dockerfile found **in the current directory** and then started.

---

## **When to Use Docker Compose**

Docker Compose is particularly useful in the following scenarios:
- **Local Development**: Quickly spin up complex environments with multiple services.
- **Testing and CI/CD**: Set up reproducible environments for continuous integration and testing.
- **Service Dependencies**: Link services that rely on each other, such as a frontend, backend, and database, without manually configuring networks and volumes.

However, Docker Compose is not intended for production use. Tools like Kubernetes or Docker Swarm are more suitable for scaling and managing containers in production environments.

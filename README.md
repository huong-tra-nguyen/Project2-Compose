# Project2-Compose

This repository contains the Docker Compose file and setup instructions for creating a Docker-In-Docker (DinD) container and a Jenkins container that utilises the DinD container to run Jenkins pipelines.

## Requirements

To set up this environment, the following prerequisites are required:

- Docker: Installed on the system.
- Docker Compose: Installed on the system.
- A working Docker network named 'jenkins' to facilitate communication between containers.

## Docker Compose File

The Docker Compose file `docker-compose.yml` is included in this repository. It defines the services for creating a Docker-In-Docker (DinD) container and a Jenkins container.

Here's a summary of the services defined in the Compose file:

1. `dind` Service (Docker-In-Docker Container):
   - Name: dind
   - Network: jenkins
   - Network Alias: docker
   - Privileged mode: Enabled
   - User: root
   - Docker TLS Configuration: Enabled
   - Docker TLS Certificates Volume: docker-certs-ca, docker-certs-client
   - Jenkins Home Volume: jenkins-data
   - Docker Engine Image: docker:dind

2. `jenkins` Service (Jenkins Container):
   - Name: jenkins
   - Network: jenkins
   - User: root
   - Docker TLS Certificates Volume: docker-certs-client
   - Jenkins Home Volume: jenkins-data
   - Published Ports: 8080 (Jenkins Web UI), 50000 (Jenkins Agent Port)
   - Base Jenkins Image: jenkins/jenkins:lts-jdk11

## Usage

To use this Docker Compose setup, follow these steps:

1. Ensure that Docker and Docker Compose installed on the system.

2. Create a Docker network named 'jenkins':
   ```sh
   docker network create jenkins
   ```

3. Start Docker-In-Docker (DinD) container:
   ```sh
   docker-compose up -d
   ```

4. After running the above command, Jenkins will be accessible via the web interface at `http://localhost:8080`.

5. To set up Jenkins, obtain the initial admin password from the container logs:
   ```sh
   docker logs jenkins
   ```

6. Follow the Jenkins setup wizard to complete the installation.

7. Jenkins pipelines that utilise the DinD container now can be created to build and run tasks.

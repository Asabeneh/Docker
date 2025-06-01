# Docker Cheatsheet with Examples

This cheatsheet provides essential Docker commands and concepts with practical examples and code snippets for effective container management. Docker is a platform for developing, shipping, and running applications in containers—lightweight, portable, and isolated environments.

---

## Key Concepts

- **Container**: A runnable instance of an image, bundling an app and its dependencies.
- **Image**: A read-only template for creating containers, built from a `Dockerfile` or pulled from a registry (e.g., Docker Hub).
- **Dockerfile**: A script with instructions to build a Docker image.
- **Docker Hub**: A public registry for sharing/pulling Docker images.
- **Volume**: Persistent storage for containers, preserving data across container lifecycles.
- **Network**: Defines how containers communicate with each other or the host.

---

## Basic Commands with Examples

### 1. **Image Management**

- `docker pull <image>:<tag>`  
  Pulls an image from a registry like Docker Hub.  
  _Example_:  

  ```bash
  docker pull redis:latest
  ```  

  Downloads the latest Redis image.

- `docker images`  
  Lists all local images with details (repository, tag, size).  
  _Example_:  

  ```bash
  docker images
  ```  

  Output:  

  ```
  REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
  redis        latest    7614ae9453d1   2 weeks ago   117MB
  ```

- `docker build -t <name>:<tag> .`  
  Builds an image from a `Dockerfile` in the current directory.  
  _Example_:  

  ```bash
  docker build -t myapp:1.0 .
  ```  

  Builds an image named `myapp` with tag `1.0`.

- `docker rmi <image>`  
  Removes an image. Use `-f` to force.  
  _Example_:  

  ```bash
  docker rmi redis:latest
  ```  

  Deletes the `redis:latest` image.

### 2. **Container Management**

- `docker run <image>`  
  Runs a container from an image. Common flags: `-d` (detached), `--name` (custom name), `-p` (port mapping).  
  _Example_:  

  ```bash
  docker run -d --name webserver -p 8080:80 nginx
  ```  

  Runs an Nginx container in the background, mapping host port 8080 to container port 80.

- `docker ps`  
  Lists running containers. Use `-a` for all (including stopped).  
  _Example_:  

  ```bash
  docker ps -a
  ```  

  Output:  

  ```
  CONTAINER ID   IMAGE     COMMAND                  STATUS
  a1b2c3d4e5f6   nginx     "/docker-entrypoint.…"   Up 5 minutes
  ```

- `docker stop <container>`  
  Stops a running container.  
  _Example_:  

  ```bash
  docker stop webserver
  ```  

  Stops the `webserver` container.

- `docker rm <container>`  
  Removes a stopped container. Use `-f` to force.  
  _Example_:  

  ```bash
  docker rm webserver
  ```  

  Deletes the `webserver` container.

- `docker exec -it <container> <command>`  
  Runs a command in a running container interactively.  
  _Example_:  

  ```bash
  docker exec -it webserver bash
  ```  

  Opens a bash shell inside the `webserver` container.

### 3. **Volume Management**

- `docker volume create <name>`  
  Creates a named volume for persistent data.  
  _Example_:  

  ```bash
  docker volume create mydata
  ```  

  Creates a volume named `mydata`.

- `docker run -v <volume>:<path> <image>`  
  Mounts a volume to a container path.  
  _Example_:  

  ```bash
  docker run -v mydata:/app/data -d myapp:1.0
  ```  

  Mounts `mydata` to `/app/data` in the container.

### 4. **Network Management**

- `docker network ls`  
  Lists all Docker networks.  
  _Example_:  

  ```bash
  docker network ls
  ```  

  Output:  

  ```
  NETWORK ID     NAME      DRIVER    SCOPE
  1234567890ab   bridge    bridge    local
  ```

- `docker network create <name>`  
  Creates a custom network.  
  _Example_:  

  ```bash
  docker network create mynetwork
  ```  

  Creates a network named `mynetwork`.

- `docker run --network <network> <image>`  
  Runs a container on a specific network.  
  _Example_:  

  ```bash
  docker run --network mynetwork -d myapp:1.0
  ```  

  Runs `myapp` on `mynetwork`.

### 5. **System Management**

- `docker info`  
  Shows system-wide info (e.g., container/image count).  
  _Example_:  

  ```bash
  docker info --format '{{.Containers}}'
  ```  

  Outputs the number of containers (e.g., `5`).

- `docker system prune`  
  Removes unused containers, networks, and images.  
  _Example_:  

  ```bash
  docker system prune -a
  ```  

  Cleans up all unused resources.

---

## Example Workflows with Code Snippets

### 1. **Building and Running a Node.js App**

Create a `Dockerfile` for a Node.js application:  
```sh
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

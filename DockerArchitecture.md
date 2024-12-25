# Docker Architecture and Lifecycle

## **1. Docker Architecture**

Docker follows a client-server architecture that involves several key components:

### **Docker Daemon**
- The **Docker Daemon** (`dockerd`) runs on the Docker Host and is responsible for managing Docker objects such as images, containers, networks, and volumes.
- It listens to requests from the Docker Client and processes them, including container lifecycle management and image building.

### **Docker Client**
- The **Docker Client** (`docker`) is the primary interface users interact with.
- Commands entered into the client are translated into API requests that the daemon processes.

### **Docker Host**
- The **Docker Host** includes the Docker Daemon, images, containers, and other resources necessary for Docker's operation.

### **Dockerfile**
- A **Dockerfile** is a script-like text file containing instructions to build a Docker image.
- It defines the base image, dependencies, configuration, and application setup steps.

---

## **2. Docker Lifecycle**

The lifecycle of Docker involves the following steps:

1. **Write a Dockerfile**:
   - Create a Dockerfile with instructions for building the desired environment.
  ```bash
    # Use an official Python runtime as the base image
   FROM python:3.9-slim

   # Set the working directory
   WORKDIR /app

   # Copy application files
   COPY . /app

   # Install dependencies
   RUN pip install -r requirements.txt

   # Set the command to run the application
   CMD ["python", "app.py"]
  ```  

2. **Build the Image**:
   - Use the `docker build` command to convert the Dockerfile into an image.
   - `docker build -t my-python-app . `

3. **Run a Container**:
   - Use the `docker run` command to start a container based on the image.
   - `docker run -d -p 5000:5000 my-python-app`

4. **Manage the Container**:
   - Use commands like `start`, `stop`, and `restart` to manage container states.

5. **Push and Pull Images**:
   - Store images in a registry like Docker Hub or pull them from a remote registry.
```bash
    docker stop <container_id>
    docker start <container_id>
    docker restart <container_id>
    docker tag my-python-app myrepo/my-python-app:v1
    docker push myrepo/my-python-app:v1
    docker pull myrepo/my-python-app:v1
```

   
---

# Example: Installing Docker on Ubuntu and Running "Hello World"

1. **Install Docker on Ubuntu**:
   - Update the package index and install Docker's dependencies.
   - Add the official Docker repository and install Docker Engine.

2. **Run "Hello World"**:
   - Verify Docker installation by running the `hello-world` container.
   - This container outputs a confirmation message to demonstrate that Docker is running correctly.
  
```bash
sudo apt update

# Install required dependencies
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker's official repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
```

---

# Most Used and Frequently Asked Docker Commands

## **1. Image Management**
- `docker images` – Lists all locally available Docker images.
- `docker pull <image>` – Downloads an image from a Docker registry.
- `docker build -t <tag> .` – Builds an image from a Dockerfile.

## **2. Container Management**
- `docker run <image>` – Starts a new container from an image.
- `docker ps` – Lists all running containers.
- `docker ps -a` – Lists all containers, including stopped ones.
- `docker stop <container>` – Stops a running container.
- `docker rm <container>` – Removes a stopped container.

## **3. Logs and Debugging**
- `docker logs <container>` – Fetches logs of a container.
- `docker exec <container> <command>` – Runs a command inside a running container.

## **4. Networking**
- `docker network ls` – Lists all Docker networks.
- `docker network inspect <network>` – Shows details about a specific network.

## **5. Volumes**
- `docker volume ls` – Lists all Docker volumes.
- `docker volume inspect <volume>` – Shows details of a specific volume.

## **6. Docker System Commands**
- `docker system df` – Shows disk usage by Docker resources.
- `docker system prune` – Removes unused Docker resources to free up space.

---

Docker simplifies containerization, making development and deployment consistent across environments. Its architecture, lifecycle, and command-line tools are essential knowledge for leveraging its full potential.

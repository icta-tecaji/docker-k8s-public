# Praktična delavnica Docker & Kubernetes

- Prenesemo gradiva iz Git-a: `git clone https://github.com/icta-tecaji/docker-k8s-public.git`
- [Namestimo VS Code](https://code.visualstudio.com/)

## [Del 1: Intro To Docker](./01_Intro_To_Docker/README.md)
- Docker overview
- Why are containers and Docker so important?
- Installing Docker
- Running Hello World in a container
- Example: Running multiple NGINX instances
- Docker Architecture

## [Del 2: Containers](./02_Containers/README.md)
- Containers History
- Containers vs Virtual Machines
- What is a container?
- Starting a simple container
- Container processes
- Web server example
- Exploring the container filesystem and the container lifecycle
- Stopping containers gracefully

## [Del 3: Docker Images](./03_Docker_Images/README.md)
- About Docker Images
- Images are usually small
- Pulling images
- Image registries
- Image naming and tagging
- Filtering the images on the host
- Images and layers
- Deleting Images

## [Del 4: Docker Storage And Volumes](./04_Docker_Storage_And_Volumes/README.md)
- Introduction to Docker Storage And Volumes
- Containers and non-persistent data
- Running containers with Docker volumes
- Creating and managing Docker volumes
- Demonstrating volumes with containers and services
- Populate a volume using a container
- Use a read-only volume
- Share data between Docker containers
- Running containers with filesystem mounts (Bind mounts)
- Limitations of filesystem mounts
- Choose the right type of mount
- Example: Run a PostgreSQL database

## [Del 5: Docker Networking](./05_Docker_Networking/README.md)
- Docker networking overview
- Network drivers
- Bridge networks
- Host network
- Overlay networks
- Disable networking for a container
- Port Mapping

## [Del 6: Building Images](./06_Building_Images/README.md)
- Containerizing an app - overview
- Building your first container image
- Understanding Docker images and image layers
- Understanding the image layer cache
- Understand how CMD and ENTRYPOINT interact
- Difference between the COPY and ADD commands
- Using the VOLUME command inside Dockerfiles
- Run containers as non-root user
- Best practices for writing Dockerfiles
- Example: Build a Python application
- Use multi-stage builds

## [Del 7: Sharing Docker Images](./07_Sharing_Docker_Images/README.md)
- Working with registries, repositories, and image tags
- Create a repo on Docker Hub
- Pushing your own images to Docker Hub
- Run the image on a new instance
- Introducing self-hosted registries
- Running and using your own Docker registry
- Using image tags effectively

## [Del 8: Running Multi-container Apps With Docker Compose](./08_Running_Multi_container_Apps_With_Docker_Compose/README.md)
- Overview of Docker Compose and installation
- Compose background
- Running a application with Compose: counter-app
- Development with Compose 
- Production deployment with Compose: counter-app
- Scaling and Load Balancing using Compose

## [Del 9: Docker Reliability And Health Checks](./09_Docker_Reliability_And_Health_Checks/README.md)
- Self-healing containers with restart policies
- Using restart policies in Docker Compose
- Understanding PID 1 in Docker containers
- Building health checks into Docker images
- Starting containers with dependency checks
- Defining health checks and dependency checks in Docker Compose

## [Del 10: Container orchestration and microservices](./10_Container_orchestration_and_microservices/README.md)

## [Del 11: Introduction to Kubernetes](./11_Introduction_to_Kubernetes/README.md)
- About Kubernetes
- Kubernetes history
- Understanding Kubernetes
- The benefits of using Kubernetes
- Kubernetes architecture
- Kubernetes versions
- Install Kubernetes
- Should you even use Kubernetes?
- How Kubernetes runs an application
- Deploying your first application

## [Del 12: Kubernetes Pods](./12_Kubernetes_Pods/README.md)
- Pod theory
- Understanding pods
- Sidecar containers
- Creating pods
- Viewing application logs
- Copying files to and from containers
- Executing commands in running containers
- Running multiple containers in a pod
- Running init containers
- Deleting pods and other objects

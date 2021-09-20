# Del 2: Uvod v Kubernetes

## Building the container image
- Move to folder gradiva: `cd  gradiva`
- To build the image, run the following Docker command: `docker build -t simple_server .`

## Running the container image
- Run: `docker run --name simple-server-container -p 8080:8080 -d simple_server`
- Accessing your app: `curl localhost:8080`

## Exploring the inside of a running container
- Running a shell inside an existing container: `docker exec -it simple-server-container bash`
- List running processes in the container (run in the container shell): `ps aux`
- Leave container: `exit`
- List the processes on the host OS itself (run on host): `ps aux | grep app.js`

## Stopping and removing a container
- Stop the container: `docker stop simple-server-container`
- Remove a container: `docker rm simple-server-container`

## Pushing the image to an image registry
- Tagging an image under an additional tag: `docker tag simple_server leon11sj/simple_server`
- List the images stored on your system: `docker images | head`
- Pushing the image to Docker Hub: `docker push leon11sj/simple_server`

## Running your first app on Kubernetes
- The simplest way to deploy your app is to use the kubectl run command, which will create all the necessary components without having to deal with JSON or YAML.
- Run the image on Kubernetes: `kubectl run simple-server --image=leon11sj/simple_server --port=8080`
- List pods: `kubectl get pods`

## Accessing your web application
- Creating a Service object: `kubectl expose pod simple-server --port=80 --target-port=8080 --name=simple-server-service --type=NodePort`
- Listing services: `kubectl get services`
- Accessing your service through its external IP: `curl <HOST_IP>:<PORT>`

## Stop and remove the components
- Delete the service: `kubectl delete service simple-server-service`
- Delete the pod: `kubectl delete pods simple-server`

# Sharing Docker Images

## Working with registries, repositories, and image tags

## Create a repo on Docker Hub

## Pushing your own images to Docker Hub
- `sudo docker login --username <USERNAME>`
- `docker login registry.example.com`
- `sudo docker image tag web-ping:v2 <USER_ID>/web-ping:v1`
- `docker image ls`
- `sudo docker image push <USER_ID>/web-ping:v1`
- `sudo docker search <USER_ID>`
- `docker container run --rm -e TARGET=google.com leon11sj/web-ping:v1`

## Introducing self-hosted registries

## Running and using your own Docker registry
- `sudo docker run -d -p 5000:5000 --restart=always --name registry registry:2`
- `sudo docker ps`
- `sudo docker pull ubuntu:20.04`
- `sudo docker tag ubuntu:20.04 localhost:5000/my-ubuntu`
- `sudo docker push localhost:5000/my-ubuntu`
- `sudo docker image remove ubuntu:20.04`
- `sudo docker image remove localhost:5000/my-ubuntu`
- `docker pull localhost:5000/my-ubuntu`
- `docker container stop registry && docker container rm -v registry`

## Using image tags effectively

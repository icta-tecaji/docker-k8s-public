# Docker Images

## About Docker Images

## Images are usually small

## Pulling images
- `sudo docker image ls`
- `sudo docker image pull busybox:latest`
- `sudo docker image pull alpine:latest`
- `sudo docker image pull redis:latest`
- `sudo docker image ls`

## Image registries
- Run: `sudo docker info`

The following list contains a few of the official repositories: 
- nginx: https://hub.docker.com/_/nginx/
- busybox: https://hub.docker.com/_/busybox/
- redis: https://hub.docker.com/_/redis/
- mongo: https://hub.docker.com/_/mongo/

## Image naming and tagging
- `sudo docker image pull alpine:latest`
- `sudo docker image pull bitnami/postgresql:14`
- `sudo docker image pull gcr.io/google-containers/git-sync:v3.1.5`

## Filtering the images on the host
- `sudo docker image ls --filter dangling=true`
- `docker image prune`

## Images and layers
- `sudo docker image pull redis:latest`
- `sudo docker image inspect redis:latest`
- `sudo docker image pull python:3.9.13-bullseye`
- `sudo docker image pull python:3.9.13-slim-bullseye`

## Deleting Images

- `sudo docker image ls`
- `sudo docker image rm python:3.9.13-slim-bullseye`
- `sudo docker image rm python:3.9.13-bullseye`
- `sudo docker image rm $(sudo docker image ls -q)`

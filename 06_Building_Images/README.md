# Building Images

## Containerizing an app - overview

```Dockerfile
FROM node:18-alpine

ENV TARGET="ltfe.org"
ENV METHOD="HEAD"
ENV INTERVAL="3000"

WORKDIR /web-ping
COPY app.js .

CMD ["node", "/web-ping/app.js"]
```

- `cd 06_Building_Images/examples/01_ping_app`
- `ls -la`
- `sudo docker image build --tag web-ping .`
- `sudo docker image ls 'w*'`
- `sudo docker container run --rm -e TARGET=docker.com -e INTERVAL=5000 web-ping`

## Understanding Docker images and image layers
- `sudo docker image history web-ping`
- `sudo docker image ls`
- `sudo docker system df`

## Understanding the image layer cache
- `sudo docker image build -t web-ping:v2 .`
- `cd 06_Building_Images/examples/02_simpleweb/`
- `cat Dockerfile`
- Build the image:
    - `sudo docker build -t simpleweb .`
    - `sudo docker run --rm simpleweb` 
    - `sudo docker run --rm -p 80:8080 simpleweb`

## Understand how CMD and ENTRYPOINT interact
- `cd 06_Building_Images/examples/03_entrypoint_vs_cmd/`
- `sudo docker image build -t entrypointcmd:v1 -f Dockerfile_01 .` 
- `sudo docker run --rm entrypointcmd:v1`
- `sudo docker run --rm entrypointcmd:v1 -la`
- `sudo docker run --rm --entrypoint ps entrypointcmd:v1 aux`
- `sudo docker image build -t entrypointcmd:v2 -f Dockerfile_02 .`
- `sudo docker run --rm entrypointcmd:v2`
- `sudo docker run --rm entrypointcmd:v2 ls -la`
- `sudo docker image build -t entrypointcmd:v3 -f Dockerfile_03 .`
- `sudo docker run --rm entrypointcmd:v3`
- `sudo docker run --rm entrypointcmd:v3 -l`
- `sudo docker image build -t run_exec -f Dockerfile_04 .`
- `sudo docker image build -t run_shell -f Dockerfile_05 .`
- Exec form: 
    - `sudo docker run -d --rm --name exec run_exec`
    - `sudo docker exec -it exec ps x`
    - `time sudo docker stop exec`
- Shell form:
    - `sudo docker run -d --rm --name shell run_shell`
    - `sudo docker exec -it shell ps x`
    - `time sudo docker stop shell`

## Difference between the COPY and ADD commands

```Dockerfile
ADD https://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
And instead, do something like:
```Dockerfile
RUN mkdir -p /usr/src/things \
    && curl -SL https://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
```

## Using the VOLUME command inside Dockerfiles
The syntax is simply `VOLUME <target-directory>`.
```Dockerfile
FROM ubuntu:20.04

WORKDIR /data
RUN echo "Hello from Volume" > test

RUN mkdir /uploads

VOLUME /data
VOLUME /uploads

CMD ["sleep", "10000"]
```
- `cd 06_Building_Images/examples/04_volumes/`
- `sudo docker image build -t test_volume .`
- `sudo docker run -d -v uploads_data:/uploads --name test test_volume`
- `sudo docker container inspect test | grep -A 20 Mounts`
- `sudo docker volume ls`
- `sudo docker stop test`
- `sudo docker volume ls`
- `sudo docker rm test`
- `sudo docker volume ls`
- `sudo docker run -d -v uploads_data:/uploads --name test test_volume`
- `sudo docker volume ls`
- `sudo docker stop test`
- `sudo docker rm -v test` (Remove anonymous volumes associated with the container)
- `docker volume prune`

## Run containers as non-root user
- `cd 06_Building_Images/examples/06_user/`
- `sudo docker run --rm ubuntu:20.04 id`
- `cat Dockerfile`
- `sudo docker image build -t ubutnu-no-root .`
- `sudo docker run --rm ubutnu-no-root id`

## Best practices for writing Dockerfiles

### Start your Dockerfile with the steps that are least likely to change

### Clean up your Dockerfile

### Containers should be ephemeral (short-lived)

### Don’t install unnecessary packages

### One container should have one concern - decouple applications

### Exclude with .dockerignore

### Security scanning

## Example: Build a Python application
- `cd 06_Building_Images/examples/07_python_app/`
- `ls -la`
- `cat .dockerignore`
- `cat Dockerfile.prod`
- `sudo docker image build -t smart-api -f Dockerfile.prod .`
- `sudo docker run --rm -d --name smart-api -p 80:5000 smart-api`
- `sudo docker exec smart-api id`
- `sudo docker exec smart-api ps aux`
- `sudo docker stop smart-api`

## Use multi-stage builds

> Check for language specific tutorials. [Python](https://pythonspeed.com/articles/smaller-python-docker-images/) / [Java](https://dzone.com/articles/multi-stage-docker-image-build-for-java-apps) / [Go](https://docs.docker.com/develop/develop-images/multistage-build/#use-multi-stage-builds) / [NodeJS](https://cloudnweb.dev/2019/10/crafting-multi-stage-builds-with-docker-in-node-js/)

- `cd 06_Building_Images/examples/08_python_app_multi_stage/`
- `docker image ls smart-api` (size of the image)
- `cat Dockerfile.prod`
- `sudo docker image build -t smart-api-prod -f Dockerfile.prod .`
- `sudo docker run --rm -d --name smart-api -p 80:5000 smart-api-prod`
- `docker image ls smart-api-prod` (size of the image)
- `sudo docker stop smart-api`

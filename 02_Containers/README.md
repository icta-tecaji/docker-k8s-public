# Containers

## Containers History

## Containers vs Virtual Machines

## What is a container?

## Starting a simple container
- `sudo docker run -it ubuntu:latest /bin/bash`

same as

- `sudo docker container run --interactive --tty ubuntu:latest /bin/bash`

Run:
- `ls -l`
- `ping google.com`
- `apt update`
- `apt install -y iputils-ping`
- `ping google.com`

Open new terminal and run:
- `sudo docker container ls`
- `sudo docker container top <CONTAINER ID or NAME>`
- `sudo docker container logs <CONTAINER ID or NAME>`
- `sudo docker container inspect <CONTAINER ID or NAME>`

## Container processes
- Run inside the container: `ps -elf`
- `docker container ls`
- `sudo docker attach <CONTAINER ID or NAME>`
- Run inside the container: `ps -elf`
- Press `Ctrl-PQ`
- Re-attach your terminal to it with the `docker container exec` command.
- `sudo docker container exec -it <CONTAINER ID or NAME> bash`
- Run inside the container: `ps -elf` 
- `exit`
- `sudo docker container ls`
- `sudo docker container stop <CONTAINER ID or NAME>`
- `sudo docker container ls -a`
- `sudo docker container rm <CONTAINER ID or NAME>`

## Web server example
- `sudo docker container run --detach --publish 8088:80 nginx:1.23.0`
- `sudo docker container ls`
- `sudo docker image inspect nginx:1.23.0`
- `sudo docker container stats <CONTAINER ID or NAME>`
- `sudo docker container rm -f $(docker container ls -aq)

## Exploring the container filesystem and the container lifecycle
- Run the NGINX web container: `sudo docker container run --name my-nginx -d -p 8080:80 nginx:latest`
- `sudo docker container exec my-nginx ls /usr/share/nginx/html`
- Create a `index.html` file locally with some HTML content.
- Copy the file: `sudo docker container cp index.html my-nginx:/usr/share/nginx/html/index.html`. 
- Attach to the container: `sudo docker exec -it my-nginx /bin/bash`
- Inside the container run:
    - Move to data folder: `cd /usr/share/nginx/html`
    - Install nano: `apt install` and `apt install -y nano`
    - Change the file: `nano index.html`
    - Refresh the webpage.
    - Exit the container `exit`.
- Stop the container: `sudo docker stop my-nginx`
- List all running containers: `sudo docker ps`. 
- Run the container: `sudo docker start my-nginx`.
- `sudo docker rm -f my-nginx`
- Run a new container: `sudo docker container run --name my-nginx -d -p 8080:80 nginx:latest`
- Stop and remove the container: `sudo docker rm -f my-nginx`

## Stopping containers gracefully

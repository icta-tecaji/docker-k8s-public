#  Docker Storage And Volumes

## Introduction to Docker Storage And Volumes

## Containers and non-persistent data

## Running containers with Docker volumes

## Creating and managing Docker volumes
- `sudo docker volume create myvol`
- `sudo docker volume ls`
- `sudo docker volume inspect myvol`

## Demonstrating volumes with containers and services
- `sudo docker run -d --name dev-server --mount source=server-data,target=/app nginx:latest`

or
- `sudo docker run -d --name dev-server -v server-data:/app nginx:latest`

Run: 
- `sudo docker volume inspect server-data`
- `sudo docker inspect dev-server` 
- `sudo docker volume rm server-data`
- `sudo docker container exec -it dev-server sh`
- `ls -lah`
- `echo "Test volume" > /app/file1`
- `ls -l /app`
- `cat /app/file1`
- `sudo docker container rm -f dev-server`
- `sudo docker container ls -a`
- `sudo docker volume ls`
- `sudo ls -l /var/lib/docker/volumes/server-data/_data/`
- `sudo cat /var/lib/docker/volumes/server-data/_data/file1`
- `sudo docker run -d --name dev-server-2 --mount source=server-data,target=/app nginx:latest`
- `sudo docker container exec -it dev-server-2 sh`
- `cat /app/file1`
- `exit`
- `sudo docker container stop dev-server-2`
- `sudo docker container rm dev-server-2`
- `sudo docker volume rm server-data`

## Populate a volume using a container
- `sudo docker run -d --name=nginxtest -v nginx-vol:/usr/share/nginx/html nginx:latest`
- `sudo docker exec -it nginxtest /bin/bash`
- `ls /usr/share/nginx/html`
- `exit`
- `sudo docker container rm -f nginxtest`
- `sudo docker volume rm nginx-vol`

## Use a read-only volume
- `sudo docker run -d --name=nginxtest -v nginx-vol:/usr/share/nginx/html:ro nginx:latest`
- `sudo docker inspect nginxtest`
- `sudo docker exec -it nginxtest /bin/bash`
- `cd /usr/share/nginx/html`
- `touch test`
- `exit`
- `sudo docker container rm -f nginxtest`
- `sudo docker volume rm nginx-vol`

## Share data between Docker containers

Example of sharing a volume:
- `sudo docker run -d --name=nginxtest -p 80:80 -v nginx-vol:/usr/share/nginx/html:ro nginx:latest`
- `sudo docker run -d --name=downloader -v nginx-vol:/web-data busybox:latest sleep 5000`
- `sudo docker exec -it downloader /bin/sh`
- `cd web-data`
- `wget https://example.com/ -O index.html`
- `wget https://www.iana.org/domains/reserved -O index.html`
- `exit`
- `sudo docker rm -f nginxtest downloader`
- `sudo docker volume rm nginx-vol`

## Use a third party volume driver (Advanced)
- `cd 04_Docker_Storage_And_Volumes`
- `cat examples/01_nginx/example.conf`
- `CONF_SRC=$(pwd)/examples/01_nginx/example.conf`
- `LOGS_SRC=$(pwd)/examples/01_nginx/example.log`
- `touch $LOGS_SRC`

```bash
sudo docker run -d --name my-server \
        --mount type=bind,source=${CONF_SRC},target=/etc/nginx/conf.d/default.conf \
        --mount type=bind,source=${LOGS_SRC},target=/var/log/nginx/custom.host.access.log \
        -p 80:80 \
        nginx:latest
```
- `sudo docker rm -f my-server`

```bash
sudo docker run -d --name my-server \
        --mount type=bind,source=${CONF_SRC},target=/etc/nginx/conf.d/default.conf,readonly \
        --mount type=bind,source=${LOGS_SRC},target=/var/log/nginx/custom.host.access.log \
        -p 80:80 \
        nginx:latest
```
- `sudo docker rm -f my-server`


## Limitations of filesystem mounts

- `sudo docker run -d --name no-mount nginx:latest`
- `sudo docker exec no-mount ls -la /usr/share/nginx/html`
- `CONF_SRC=$(pwd)/examples/02_mount_folder`
- `sudo docker run -d --name mount --mount type=bind,source=${CONF_SRC},target=/usr/share/nginx/html nginx:latest`
- `sudo docker exec mount ls -la /usr/share/nginx/html`
- `sudo docker rm -f mount no-mount`
- `CONF_SRC=$(pwd)/examples/02_mount_folder/test.html`
- `sudo docker run -d --name mount --mount type=bind,source=${CONF_SRC},target=/usr/share/nginx/html/test.html nginx:latest`
- `sudo docker exec mount ls -la /usr/share/nginx/html`
- `sudo docker rm -f mount`


## Choose the right type of mount

## Example: Run a PostgreSQL database
- `CONF_PATH=$(pwd)/examples/03_postgres/my-postgres.conf`
- `sudo docker volume create pgdata`
```bash
sudo docker run -d --name my-database \
        --mount type=bind,source=${CONF_PATH},target=/etc/postgresql/postgresql.conf,readonly \
        -v pgdata:/var/lib/postgresql/data \
        -e POSTGRES_USER=test \
        -e POSTGRES_PASSWORD=mojegeslo \
        -e POSTGRES_DB=test \
        postgres -c 'config_file=/etc/postgresql/postgresql.conf'
```
- `sudo docker exec -it my-database psql -U test`
- `\l`
- `\c test`
- `CREATE TABLE PODATKI(ID INT);`
- `\dt`
- `exit`
- `sudo docker rm -f my-database`
- `sudo docker volume rm pgdata`

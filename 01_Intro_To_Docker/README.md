# Intro To Docker

- [Docker Dokumentacija](https://docs.docker.com/)


## Docker overview

## Why are containers and Docker so important?

## Installing Docker

### Docker Desktop
- Install Docker Desktop: https://docs.docker.com/get-docker/

### Docker Engine
1. [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
2. [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)
    - [Optional] Manage Docker as a non-root user -> Security risks!
    - Configure Docker to start on boot

- [Differences between Docker Desktop for Linux and Docker Engine](https://docs.docker.com/desktop/install/linux-install/#differences-between-docker-desktop-for-linux-and-docker-engine)

### Verifying your Docker setup
- `docker version`

## Running Hello World in a container
- `sudo docker container run hello-world` or `sudo docker run hello-world`
- List all running container processes: `sudo docker ps`
- `sudo docker ps -a`
- Repeat the exact same Docker command: `sudo docker run hello-world`

Run and check for a container:
- `sudo docker ps`
- `sudo docker ps -a`
- Clean: `sudo docker container rm -f $(docker container ls -aq)`

## Example: Running multiple NGINX instances
1. Preverimo ali je na virtualki že zagnan Apache server:
    - Preverimo delovanje: `curl http://127.0.0.1 ali IP virtualke`
    - `sudo systemctl stop apache2.service`
    - `sudo systemctl disable apache2.service`
2. Želimo namesti in stestirati delovanje NGINX instance:
    - `sudo apt-get update`
    - `sudo apt-get install -y nginx`
    - `nginx -v`
    - `sudo systemctl start nginx`
    - `sudo systemctl status nginx`
    - Preverimo delovanje: `curl http://127.0.0.1 ali IP virtualke`
3. Želimo namestiti dve instance NGINX:
    - `sudo apt-get install nginx` - > že obstaja
    - `sudo systemctl start nginx` -> vidmo da še vedno samo ena instanca je delujoča
    - `sudo ps aux | grep nginx`
    - Če želimo namestit dve instance moramo spremeniti init scripte
        - `cat /etc/init/nginx.conf`
    - Ustavimo:
        - `sudo systemctl stop nginx`
        - `sudo systemctl disable nginx`
4.  Zaženmo NGINX s pomočjo Dockrja:
    - NGINX DockerHub: https://hub.docker.com/_/nginx
    - Prenesemo image lokalno: `docker pull nginx`
        - Uporaba default taga `latest`
        - Zagon 1. instance: `sudo docker run -d nginx`
        - Preverimo ali je up: `sudo docker ps`
        - Zaženemo drugo in tretjo verzijo NGINXa
            - `sudo docker run -d nginx:1.22.0`
            - `sudo docker run -d nginx:1.14.0`
        - Preverimo ali je up: `sudo docker ps`
        -  `sudo ps aux | grep nginx`
5. Izpostavimo kontejner na internet:
    - `sudo docker run -d -p "80:80" nginx`
    - Preverimo ali je up: `sudo docker ps`
6. Clean: `sudo docker container rm -f $(docker container ls -aq)`

## About Docker Company

## Docker Architecture

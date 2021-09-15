# Del 2: Uvod v Docker

## Primerjava namestitve 2 instanc NGINX
- Želimo namesti in stestirati delovanje nginx po klasičnem načinu namestitve:
    - `sudo apt-get update`
    - `sudo apt-get install nginx`
    - `nginx -v`
    - `sudo systemctl start nginx`
    - `sudo systemctl status nginx`
    - `curl http://127.0.0.1 ali IP virtualke`
- Pri nameščanju druge instance naletimo na težave
- Namestitev z Dockerjem:
    - Poberemo sliko iz Docker repozitorija: `docker pull nginx`
    - Run the first instance: `docker run -d nginx`
    - Check if it's up:  `docker ps`
    - Run a different version of nginx:
        - `docker run -d nginx:1.9.3`
        - `docker run -d nginx:1.10.0`
    - Check how many instances are running:
        - List all running container processes: `docker ps`
        - `sudo ps aux | grep nginx`
        - `ps tree`
    - Stop all rhe containers: `docker stop $(docker ps -aq)`
    - Remove all the containers: `docker rm $(docker ps -aq)`
    - Expose the container port: `docker run -d -p "80:80" nginx`
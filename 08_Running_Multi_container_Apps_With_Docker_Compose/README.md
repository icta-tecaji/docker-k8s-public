# Running Multi-container Apps With Docker Compose

## Overview of Docker Compose and installation

## Compose background

## Running a application with Compose: counter-app
- `cat ~/docker-k8s/08_Running_Multi_container_Apps_With_Docker_Compose/examples/01_counter-app/docker-compose.yml`

> The most current, and recommended [Compose Specification](https://docs.docker.com/compose/compose-file/).

- Check the files (`cd ~/docker-k8s/08_Running_Multi_container_Apps_With_Docker_Compose/examples/01-counter-app`):
- `app.py`: is the application code (a Python Flask app)
- `requirements.txt`: lists the Python packages required for the app
- `Dockerfile`: describes how to build the image for the web-fe service
- Let’s use Compose to bring the app up: `sudo docker compose up`. 
- `sudo docker compose down`
- `sudo docker volume ls`
- `sudo docker compose up -d`
- `sudo docker image ls`
- `sudo docker container ls`
- `sudo docker network ls`
- `sudo docker volume ls`

Managing an app with Compose:
- `sudo docker compose ps`
- `sudo docker compose top`
- `sudo docker compose logs`
- `sudo docker compose logs -f`
- `sudo docker compose stop`
- `sudo docker compose ps`
- `sudo docker compose restart`
- `sudo docker compose ps`
- `sudo docker compose down -v`

## Development with Compose
- Move to: `cd ~/docker-k8s/08_Running_Multi_container_Apps_With_Docker_Compose/examples/02-counter-app-dev`
- Check the `docker-compose.dev.yml` file in your project directory.
- `docker compose -f docker-compose.dev.yml up`
- Stop the service: `docker compose -f docker-compose.dev.yml down`

## Production deployment with Compose: counter-app
- Move to folder: `cd ~/docker-k8s/08_Running_Multi_container_Apps_With_Docker_Compose/examples/03-counter-app-prod`
- Add gunicorn (a production-grade WSGI server) to `requirements.txt`
- Build the image: `sudo docker build -t leon11sj/counter -f ./Dockerfile.prod .`
- Create a new Compose file called `docker-compose.prod.yml` for production.
- Run the app: `docker compose -f docker-compose.prod.yml up`
- Stop the app: `docker compose -f docker-compose.prod.yml down -v`

## Scaling and Load Balancing using Compose
- `docker compose -f docker-compose.prod.yml up -d`
- `docker compose -f docker-compose.prod.yml ps`
- Scale up: `docker compose -f docker-compose.prod.yml up -d --scale web-fe=3`
- `docker compose -f docker-compose.prod.yml ps`
- `docker container exec -it 03-counter-app-prod-nginx-1 sh`
- `nslookup web-fe`
- `ping web-fe` 
- `ping web-fe` 
- `ping web-fe`
- `nslookup web-fe`
- `nslookup web-fe`
- `nslookup web-fe`
- `exit`

Few more steps:
- Scale down: `docker compose -f docker-compose.prod.yml up -d --scale web-fe=1`
- `docker compose -f docker-compose.prod.yml ps`
- Stop the app: `docker compose -f docker-compose.prod.yml down -v`

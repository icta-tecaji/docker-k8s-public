# Del 7: Docker Compose

## Visits app
- Build: `docker build -t leon11sj/visits .`
- Manual run:
    - `docker network create --driver bridge --attachable redis-app`
    - `docker run -d --network redis-app --name redis-server redis`
    - `docker run -d --network redis-app -p 80:8081 --name node-app --restart on-failure leon11sj/visits`
- Remove:
    - `docker stop node-app redis-server`
    - `docker rm node-app redis-server`
    - `docker network rm redis-app`
- Run with docker-compose:
    - Start the app: `docker-compose up`
    - Start and rebuild the image: `docker-compose up --build`
    - Run containers in background: `docker-compose up -d`
    - Stop and clean: `docker-compose down`

## Composetest app
- Run dev: `docker-compose -f <COMPOSE_FILE> up`
- Stop dev: `docker-compose -f <COMPOSE_FILE> down`


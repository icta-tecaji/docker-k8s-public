# Docker Reliability And Health Checks

## Self-healing containers with restart policies
- `docker container run -d --name test-restart --restart always nginx:1.23-alpine`
- `docker ps`
- Show running processes in container: `docker exec test-restart ps -a`
- Kill the PID 1 process: `docker exec test-restart kill 1`
- `docker ps`
- `docker exec test-restart ps -a`
- `docker container inspect test-restart | grep RestartCount`
- `docker rm -f test-restart`


Create the two new containers:
- `docker container run -d --name always --restart always  alpine sleep 1d`
- `docker container run -d --name unless-stopped --restart unless-stopped alpine sleep 1d`
- `docker ps`
- Stop both containers: `docker container stop always unless-stopped`
- `docker ps -a`
- Restart Docker: `sudo systemctl restart docker`
- `docker ps -a`
- `docker rm -f always unless-stopped`

## Using restart policies in Docker Compose

Try the following:
- Move to folder: `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/00_restart_polcy`
- Check the `docker-compose.yml` file.
- Run the app: `docker compose up --build`
- Stop the app: `docker compose down`
- Change the `restart` filed to `on-failure` in the `docker-compose.yml` file.
- Run the app: `docker compose up --build`
- Check: `docker compose ps` and `docker compose logs -f`.
- Stop the app: `docker compose down`
- Uncomment the `sys.exit(0)` line in the `app.py` file.
- Run the app: `docker compose up --build`
- Check: `docker compose ps` and `docker compose logs`.
    - The container exited and stopped (no restart).
- Stop the app: `docker compose down`

## Understanding PID 1 in Docker containers
- Move to folder: `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/01_app_no_signal_handling`
- Build the image: `docker build -t pid-01 .`
- Run: `docker run -d --name pid-01 pid-01`
- Look at the logs: `docker logs pid-01 -f`
- `docker ps`
- We are running Python as PID 1: `docker exec pid-01 ps -a`
- Send a SIGTERM (15) signal to a process: `docker exec pid-01 kill -15 1`
- `docker ps`
- Try to stop the container: `docker stop pid-01`. 
- `docker rm pid-01`

We added some signal handling in the next example: 
- `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/02_app_with_signal_handling`
- Build the image: `docker build -t pid-02 .`
- Run: `docker run -d --name pid-02 pid-02`
- `docker ps`
- We are running Python as PID 1: `docker exec pid-02 ps -a`
- Run the logs in a **different terminal window**: `docker logs pid-02 -f`
- Send a SIGTERM (15) signal to a process: `docker exec pid-02 kill -15 1` (the proccess shutdown gracefully)
- `docker ps -a`
- `docker rm pid-02`
- Run: `docker run -d --name pid-02 pid-02`
- Run the logs in a **different terminal window**: `docker logs pid-02 -f`
- Try to stop the container: `docker stop pid-02` (the container shutdown immediately)
- `docker rm pid-02` 
- Move to folder: `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/03_app_entrypoint_problem/`
- `docker compose up --build`
- Open a new terminal window:
    - `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/03_app_entrypoint_problem/`
    - `docker ps -a`
    - Look at the processes in the contianer: `docker exec app ps -aux`.  
    - Stop: `docker compose down`. The proccess don't shutdown gracefully.

We can try with the following solution:
- `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/04_app_entrypoint/`
- `docker compose up --build`
- Open a new terminal window:
    - `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/04_app_entrypoint/`
    - `docker ps -a`
    - Look at the processes in the contianer: `docker exec app ps -aux`
    - Stop: `docker compose down`.

## Building health checks into Docker images

- Move to: `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/05_rnd_number`
- Build the image: `docker build -t random-number-api .`
- Run the app: `docker container run -d --name rnd-api -p 80:5000 random-number-api`
- Check the container status: `docker ps`
- Run it multiple time from terminal: 
    - `curl -i http://localhost/health`
    - `curl -i http://localhost/rng` (run 5x)
    - `curl -i http://localhost/health`
- Check the container status: `docker ps`
- Stop and remove the app: `docker rm -f rnd-api`

Run the same test but using the new image:
- Look at the files
- Build the app: `docker build -t random-number-api-health -f Dockerfile.health .`
- Run the app: `docker container run -d --name rnd-api-health -p 80:5000 random-number-api-health`
- Wait 30 seconds or so and list the containers: `docker ps`
- Run it multiple time from terminal: 
    - `curl -i http://localhost/health`
    - `curl -i http://localhost/rng` (run 5x)
    - `curl -i http://localhost/health`
- Run: `docker ps`
- `docker container inspect rnd-api-health`
- Stop and remove the app: `docker rm -f rnd-api-health`

## Starting containers with dependency checks
- Move to folder: `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/06_rnd_number_web`
- Build the web container: `docker build -t random-number-web .`
- Create a network: `docker network create --attachable rnd-net`
- Run the web container: `docker run -d -p 80:5000 --network rnd-net --name rnd-number-web random-number-web`
- `docker ps`
- Stop the container: `docker rm -f rnd-number-web`
- Look at the `Dockerfile.check` file.
- Build the image: `docker build -t random-number-web-check -f Dockerfile.check .`
- Run the container: `docker run -p 80:5000 --network rnd-net --name rnd-number-web-check random-number-web-check`
- `docker ps -a`
- `docker rm -f rnd-number-web-check`
- `docker network rm rnd-net`

## Defining health checks and dependency checks in Docker Compose

- Move to: `cd ~/docker-k8s/09_Docker_Reliability_And_Health_Checks/examples/07_number_compose/`
- Start the app: `docker compose up -d`
- `docker compose ps`
- `docker compose logs -f`
- `docker compose ps`
- Stop the app: `docker compose down`

# Docker Networking

## Docker networking overview
- `sudo docker network ls`

## Network drivers

## Bridge networks
- `ip addr show docker0`
- `sudo docker network inspect bridge | grep bridge.name`

> Documentation: [Use bridge networks](https://docs.docker.com/network/bridge/)

### Use the default bridge network
- `sudo docker network ls`
- `sudo docker run -dit --name alpine1 alpine ash`
- `sudo docker run -dit --name alpine2 alpine ash`
- `sudo docker container ls`
- `sudo docker network inspect bridge` 
- `ip addr show` 
- `bridge link`
- `sudo docker attach alpine1`
- `ip addr show` 
- `ping -c 2 google.com` 
- `ping -c 2 172.17.0.3` 
- `ping -c 2 alpine2` 
- Detach from alpine1 without stopping it by using the detach sequence, `CTRL + p CTRL + q`
- `sudo docker attach alpine2`
- `ip addr show`
- `ping -c 2 google.com`
- `ping -c 2 172.17.0.2`
- `ping -c 2 alpine1`
- Detach from alpine2 without stopping it by using the detach sequence, `CTRL + p CTRL + q`
- `sudo docker container rm -f alpine1 alpine2`

### Use user-defined bridge networks

![Docker networks](./images/networks-docker.png)

- `sudo docker network create --driver bridge alpine-net1`
- `sudo docker network ls`
- `sudo docker network inspect alpine-net1` 

```bash
sudo docker network create \
        --driver bridge \
        --attachable \
        --gateway 10.0.42.1 \
        --subnet 10.0.42.0/24 \
        --ip-range 10.0.42.128/25 \
        alpine-net2
```
- `sudo docker network ls`
- `sudo docker network inspect alpine-net2`
- `sudo docker run -dit --name alpine1 --network alpine-net1 alpine ash`
- `sudo docker run -dit --name alpine2 --network alpine-net1 alpine ash`
- `sudo docker run -dit --name alpine3 alpine ash`
- `sudo docker run -dit --name alpine4 --network alpine-net1 alpine ash`
- `sudo docker network connect bridge alpine4`
- `sudo docker run -dit --name alpine5 --network alpine-net2 alpine ash`
- `sudo docker container ls`
- `sudo docker network inspect alpine-net1`
- `sudo docker network inspect alpine-net2`
- `sudo docker network inspect bridge`
- `sudo docker container attach alpine1`
- `ip addr show`
- `ping -c 2 172.18.0.3`
- `ping -c 2 alpine2`
- `ping -c 2 alpine4`
- `ping -c 2 alpine3` 
- `ping -c 2 alpine5`
- `nslookup alpine2`
- `ping -c 2 google.com`
- `apk update && apk add nmap`
- `nmap -sn  172.18.0.* -sn  172.17.0.* -sn 10.0.42.*  -oG /dev/stdout | grep Status`
- Detach: `CTRL + p CTRL + q`
- `sudo docker network connect alpine-net2 alpine1`
- `sudo docker attach alpine1`
- `ip addr show`
- `nmap -sn  172.18.0.* -sn  172.17.0.* -sn 10.0.42.*  -oG /dev/stdout | grep Status`
- Detach: `CTRL + p CTRL + q`
- `sudo docker container attach alpine4`
- `ip addr show`
- `ping -c 2 alpine1`
- `ping -c 2 alpine2`
- `ping -c 2 alpine3`
- `ping -c 2 172.17.0.2`
- `ping -c 2 alpine4`
- `ping -c 2 google.com`
- Detach: `CTRL + p CTRL + q`

Stop and remove all containers and networks.
- `sudo docker rm -f alpine1 alpine2 alpine3 alpine4 alpine5`
- `sudo docker network rm alpine-net1 alpine-net2` 

## Host network
- `sudo docker run --rm -d --network host --name my_nginx nginx`
- `ip addr show`
- `sudo netstat -tulpn | grep :80`
- `sudo docker exec -it my_nginx /bin/bash`
- `apt update`
- `apt install -y iproute2`
- `ip addr show`
- `exit`
- `sudo docker container stop my_nginx`

## Overlay networks

More info:
- [Use overlay networks](https://docs.docker.com/network/overlay/)
- [Networking with overlay networks](https://docs.docker.com/network/network-tutorial-overlay/)


## Disable networking for a container 
- `sudo docker run --rm -dit --network none --name no-net-alpine alpine:latest ash`
- `sudo docker exec no-net-alpine ip link show` 
- `sudo docker exec no-net-alpine ip route`
- `docker stop no-net-alpine`

## Port Mapping
- Run: `sudo docker run --rm -p 8088:8080/tcp alpine:latest echo "host UDP 8088 -> container UDP 8080"`

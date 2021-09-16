# Del 5: Networking

- Ukaz `docker network` za upravljanje z Docker omrežji.
- https://docs.docker.com/network/

## Bridge networks
- Create a user-defined bridge network: `docker network create my-net`
- List networks: `docker network ls`
- Remove a user-defined bridge network: `docker network rm my-net`
- Example:
```
docker network create \
    --driver bridge \
    --label project=myproject \
    --label stage=dev \
    --attachable \
    --scope local \
    --subnet 10.0.42.0/24 \
    --ip-range 10.0.42.128/25 \
    user-network
```

## Exploring a bridge network
- Create a new container attached to  user-network:
```
docker run -it \
    --network user-network \
    --name network-explorer \
    alpine:3.8 \
    sh
```
- Get a list of the IPv4 addresses available in the container from your terminal: `ip -f inet -4 -o addr`
- Create another bridge network and attach your running network-explorer container to both networks. First, detach your terminal from the running container (press Ctrl-P and then Ctrl-Q) and then create the second bridge network:
```
docker network create \
    --driver bridge \
    --label project=myproject \
    --label stage=dev \
    --attachable \
    --scope local \
    --subnet 10.0.43.0/24 \
    --ip-range 10.0.43.128/25 \
    user-network2
```
- Once the second network has been created, you can attach the network-explorer container: `docker network connect user-network2 network-explorer`
- Reattach your terminal: `docker attach network-explorer`
- Get a list of the IPv4 addresses available in the container from your terminal: `ip -f inet -4 -o addr`
- Install the nmap package inside your running container: `apk update && apk add nmap`
- Scan what other containers or other network devices are available on our bridge network: `nmap -sn 10.0.42.* -sn 10.0.43.* -oG /dev/stdout | grep Status`
- Detach from the terminal again (Ctrl-P, Ctrl-Q) and start another container attached to user-network2:
```
docker run -d \
    --name lighthouse \
    --network user-network2 \
    alpine:3.8 \
    sleep 1d
```
- Reattach to your network-explorer container: `docker attach network-explorer`
- Run again: `nmap -sn 10.0.42.* -sn 10.0.43.* -oG /dev/stdout | grep Status`
- Container hostnames are based on the container name, or can be set manually at container creation time by specifying the `--hostname` flag.
- Check and clean:
    - `nslookup lighthouse`
    - `ping lighthouse`
    - `exit`
    - `docker rm -f lighthouse network-explore`
    - `docker network ls`
    - `docker network rm user-network user-network2`
    
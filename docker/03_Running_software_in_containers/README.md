# Del 3: Zagon kontajnerjev

## Delo z kontejnerji
- Zagon: `docker run -d --name web nginx:latest`
- Ustavitev: `docker rm -f web`
- Primer uporabe imena kontjnerja:
```bash
CID=$(docker run -d --name web nginx:latest)
echo $CID
docker stop $CID
docker rm $CID
```
- Zagon kontejnerja v interaktivnem načinu: `docker run -it busybox /bin/sh`
- Listing: `docker ps`
- Run: `docker run -d --name web nginx:latest`
- Restart: `docker restart web`
- Container logs: `docker logs web`
- Stop and remove: `docker rm -f web`

## PID problems
- `docker run -d --name namespaceA busybox /bin/sh -c "sleep 30000"`
- `docker run -d --name namespaceB busybox /bin/sh -c "nc -l 0.0.0.0 -p 80"`
- `docker exec namespaceA ps`
- `docker exec namespaceB ps`
- Prikop v kontejner v interaktivnem načinu: `docker exec -it namespaceA /bin/sh`
- Conflict example: 
    - `docker run -d --name webConflict nginx:latest`
    - `docker logs webConflict`
    - `docker exec webConflict nginx -g 'daemon off;'`
- Run each in a different container, like this:
    - `docker run -d --name webA nginx:latest docker logs webA`
    - `docker run -d --name webB nginx:latest docker logs webB`
- Clean up: `docker rm -vf $(docker ps -a -q)`

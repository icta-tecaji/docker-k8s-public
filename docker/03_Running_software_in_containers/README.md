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
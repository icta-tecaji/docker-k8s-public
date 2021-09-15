# Del 4: Shramba podatkov 

## Primer: Dodajanje konfiguracije v kontejner
```bash
MAIN_PATH=${PWD}/data; \
CONF_SRC=${MAIN_PATH}/example.conf; \
CONF_DST=/etc/nginx/conf.d/default.conf; \
LOG_SRC=${MAIN_PATH}/example.log; \
LOG_DST=/var/log/nginx/custom.host.access.log; \
touch ${LOG_SRC}
HTML_SRC=${MAIN_PATH}/index.html
HTML_DST=/usr/share/nginx/html/index.html

docker run -d --name diaweb \
    --mount type=bind,src=${CONF_SRC},dst=${CONF_DST} \
    --mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
    --mount type=bind,src=${HTML_SRC},dst=${HTML_DST} \
    -p 80:80 \
    nginx
```

### Run the container with bind mounts:
```bash
docker run -d --name diaweb \
  --mount type=bind,src=${CONF_SRC},dst=${CONF_DST} \
  --mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
  -p 8000:80 \
  nginx:latest
```

### Run the container with a read-only bind mount:
```bash
docker run -d --name diaweb \
--mount type=bind,src=${CONF_SRC},dst=${CONF_DST},readonly=true \
--mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
-p 8000:80 \
nginx:latest
```

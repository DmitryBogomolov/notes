# Mongo in Docker

```bash
docker run \
    --name=some-name \
    --restart=always \
    -d \
    -v /path/on/host:/data/db \
    -p 27017:27017 \
    mongo:<tag>
```

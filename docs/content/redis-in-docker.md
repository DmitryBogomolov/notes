# Redis in Docker

[DockerHub](https://hub.docker.com/_/redis/)

## Simple container

Run container.
```bash
docker run \
    --name <name>
    --restart always \
    -d \
    -p 6379:6379 \
    redis:<tag>
```

Run commands.
```bash
docker exec -it <name> redis-cli
> ping
> dbsize
```

## With authentication

Download *redis.conf*.
```bash
curl http://download.redis.io/redis-stable/redis.conf > redis.conf
```

Update *requirepass* setting.
```conf
requirepass pwd1 # use actual password
```

Run container.
```bash
docker run \
    --name <name>
    --restart always \
    -d \
    -p 6379:6379 \
    -v /path/to/redis.conf:/etc/redis/redis.conf:ro \
    redis:<tag> \
    redis-server /etc/redis/redis.conf
```

Authenticate.
```bash
docker exec -it <name> redis-cli
> auth pwd1
> ping
```

## Client

Connect to server.
```bash
redis-cli -h <host> -p <port> -a <password>
> ping
```

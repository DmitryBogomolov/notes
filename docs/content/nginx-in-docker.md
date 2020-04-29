# Nginx in Docker

[DockerHub](https://hub.docker.com/_/nginx/)\
[Docs](http://nginx.org/en/docs/)

## Simple container

Make *nginx-content* folder and place *index.html* and *page.html* files there.\
Run container.
```bash
docker run \
    --name <name> \
    --restart always \
    -d \
    -p 8081:80 \
    -v /path/to/nginx-content:/usr/share/nginx/html:ro \
    nginx:<tag>
```

Access pages.
```bash
curl http://localhost:8081/
curl http://localhost:8081/index.html
curl http://localhost:8081/page.html
```

Reload config.
```bash
docker exec <name> nginx -s reload
docker exec <name> /bin/sh -c "nginx -s reload"
```

## Proxy

Update */etc/nginx/conf.d/default.conf* file - add *location* section.
```conf
server {
    listen       80;
    # ...
    location /test/ {
        proxy_pass http://172.17.0.1:8081/;
    }
    location /test {
        return 302 $scheme://$http_host$request_uri/;
    }
    # ...
}
```

File can be copied from container.
```bash
docker cp <name>:/etc/nginx/conf.d/default.conf /path/to/default.conf
```

Run container that proxies requests to another server.
```bash
docker run \
    --name <name> \
    --restart always \
    -d \
    -p 8091:80 \
    -v /path/to/default.conf:/etc/nginx/conf.d/default.conf:ro \
    nginx:<tag>
```

Access pages.
```bash
curl http://localhost:8091/test/
curl http://localhost:8091/test/index.html
curl http://localhost:8091/test/page.html
```

## Upsteams

Run two servers - `:8081` and `:8082`.\
Update */etc/nginx/conf.d/default.conf* file - add *upsteam* section.
```conf
upstream test-upstream {
    server 172.17.0.1:8081 weight=4;
    server 172.17.0.1:8082 weight=3;
}

server {
    listen       80;
    # ...
    location /test/ {
        proxy_pass http://test-upstream/;
    }

    location /test {
        return 302 $scheme://$http_host$request_uri/;
    }
    # ...
}
```

Run container.\
Access pages.
```bash
curl http://localhost:8091/test/
curl http://localhost:8091/test/index.html
curl http://localhost:8091/test/page.html
```

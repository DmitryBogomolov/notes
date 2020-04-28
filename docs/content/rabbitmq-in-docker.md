# RabbitMQ in Docker

[DockerHub](https://hub.docker.com/_/rabbitmq/)

## Simple container

Run container.
```bash
docker run \
    --name <name> \
    --restart always \
    -d \
    -p 5672:5672 -p 15672:15672 \
    rabbitmq:<tag>
```

Run commands.
```bash
docker exec -it <name> bash
> rabbitmqctl list_users
> rabbitmqctl list_permissions
```

## With authentication

Run container.
```bash
docker run \
    --name <name> \
    --restart always \
    -e RABBITMQ_DEFAULT_USER=admin1 \
    -e RABBITMQ_DEFAULT_PASS=pwd1 \
    -d \
    -p 5672:5672 -p 15672:15672 \
    rabbitmq:<tag>
```

Create user.
```bash
docker exec -it <name> bash
> rabbitmqctl add_user user1 user1pwd1
> rabbitmqctl set_user_tags user1 management
> rabbitmqctl set_permissions -p / user1 ".*" ".*" ".*"
> rabbitmqctl list_user_permission user1
```

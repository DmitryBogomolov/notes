# Mongo in Docker

[DockerHub](https://hub.docker.com/_/mongo/)

## Simple container

Run container.
```bash
docker run \
    --name <name> \
    --restart always \
    -d \
    -v /path/on/host:/data/db \
    -p 27017:27017 \
    mongo:<tag>
```

Run commands.
```bash
docker exec -it <name> mongo
> show dbs
> show collections
```

## With authentication

Run container.
```bash
docker run \
    --name <name> \
    --restart always \
    -e MONGO_INITDB_ROOT_USERNAME=admin1
    -e MONGO_INITDB_ROOT_PASSWORD=pwd1
    -d \
    -v /path/on/host:/data/db \
    -p 27017:27017 \
    mongo:<tag>
```

Authenticate as root. Create database and user.
```bash
docker exec -it <name> mongo -u 'admin1' -p 'pwd1' --authenticationDatabase 'admin'
> use TestDb1
> db.createUser({
    user: 'user1',
    pwd: 'user1pwd1',
    roles: ['readWrite']
  })
```

Authenticate as user.
```bash
docker exec -it <name> mongo -u 'user1' -p 'user1pwd1' --authenticationDatabase 'TestDb1'
> use TestDb1
```

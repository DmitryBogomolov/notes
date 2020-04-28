# MySQL in Docker

[DockerHub](https://hub.docker.com/_/mysql/)

## Simple container

Run container.
```bash
docker run \
    --name <name> \
    --restart <always> \
    -d \
    -p 3306:3306 \
    -v /path/on/host:/var/lib/mysql \
    -e MYSQL_ALLOW_EMPTY_PASSWORD=yes \
    mysql:<tag>
```

Run commands.
```bash
docker exec -it <name> mysql
> show databases;
> show privileges;
> select user, host from mysql.user;
```

## With authentication

Run container.
```bash
docker run \
    --name <name> \
    --restart <always> \
    -d \
    -p 3306:3306 \
    -v /path/on/host:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=rootpwd1 \
    mysql:<tag>
```

Authenticate as root. Create user.
```bash
docker exex -it <name> mysql --password=rootpwd1
> create user user1;
> create database db1;
> grant all on db1.* to 'user1' identified by 'pwd1';
```

Authenticate as user.
```bash
docker exex -it <name> mysql --user=user1 --password=pwd1
```

Remove user.
```bash
> revoke all on db1.* from 'user1';
> drop database db1;
> drop user user1;
```

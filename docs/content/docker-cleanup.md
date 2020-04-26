# Docker Cleanup

## Images

Find dangling images
```bash
docker images -a -f dangling=true
```

Remove them
```bash
docker rmi $(docker images -a -f dangling=true -q)
```
or
```bash
docker images purge
```

## Containers

Find exited containers
```bash
docker ps -a -f status=exited
```

Remove them
```bash
docker rm -f $(docker ps -a -f status=exited -q)
```

## All Unused Enities

Remove dandling images, containers, volumes, networks
```bash
docker system prune
```

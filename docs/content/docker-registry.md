# Docker Registry

## Setup

Run [registry](https://hub.docker.com/_/registry) container on **server**
```bash
docker run \
    --name=some-name \
    --restart=always \
    -d \
    -p 3001:5000 \
    -v /path/om/host:/var/lib/registry \
    registry:<tag>
```

Update */etc/docker/deamon.json* on **client**
```json
{
    "insecure-registries": [
        "repository-host:3001"
    ]
}
```
and force docker service to reload config
```bash
sudo kill -SIGHUP $(pidof dockerd)
```

## Usage

Push image
```bash
docker tag name:<tag> repository-host:3001/name:<tag>
docker push repository-host:3001/name:<tag>
```

Pull image
```bash
docker pull repository-host:3001/name:<tag>
docker tag repository-host/name:<tag> name:<tag>
```

Requests
```
GET /v2/_catalog
GET /v2/<image-name>/tags/list
GET /v2/<image-name>/manifests/<tag-name>
```

## Docker

```bash
docker -v
docker login

# List all images
docker images

# List all containers
docker ps -a

# Build an image
docker build -t <image>:<tag> .

# Run on local port 8080 and container port 8080
docker run -p 8080:8080 <image>:<tag>

# Tag and publish
docker build -t <username>/<repository>:<tag> .
docker tag <username>/<repository>:<tag> <username>/<repository>:latest
docker push <username>/<repository>:<tag>

# It might look something like this:
docker build -t user/app:v1.0.0 .
docker tag user/app:v1.0.0 user/app:latest
docker push user/app:v1.0.0
```

## Deployment

#### Container
```bash
# Stop, run, and clean
docker stop current-container
docker rm current-container
docker run --name=current-container --restart unless-stopped -d -p 80:5000 ${IMAGE}:${GIT_VERSION}
docker system prune -a -f
```

#### deploy.sh
```bash
#!/bin/sh
set -e # Stop script from running if there are any errors

IMAGE="<username>/<repository>"                             # Docker image
GIT_VERSION=$(git describe --always --abbrev --tags --long) # Git hash and tags

# Build and tag image
docker build -t ${IMAGE}:${GIT_VERSION} .
docker tag ${IMAGE}:${GIT_VERSION} ${IMAGE}:latest

# Log in to Docker Hub and push
echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
docker push ${IMAGE}:${GIT_VERSION}
```

# 163. 주요 명령

```bash
# Build an image based on a Docker file
# -t: Name & versions of an image
# . Build context
docker build -t NAME:TAG .
```

```bash
# Run a container based on a remote or local image
# --name Name: Container name
# --rm: Remove once stopped
# -d: Detached mode
docker run --name NAME --rm -d IMAGE
```

```bash
# Share (push) an image to a Registry (default: DockerHub)
docker push REPOSITORY/NAME:TAG
```

```bash
# Fetch (pull) an image from a Registry (default: DockerHub)
docker pull REPOSITORY/NAME:TAG
```
# 28. 컨테이너 중지 & 재시작

```
user@MacBookPro DOCKER-COMPLETE % docker --help

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "/Users/user/.docker")
  -c, --context string     Name of the context to use to connect to
                           the daemon (overrides DOCKER_HOST env var
                           and default context set with "docker
                           context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA
                           (default "/Users/user/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "/Users/user/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default
                           "/Users/user/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.10.0)
  compose*    Docker Compose (Docker Inc., v2.15.1)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  dev*        Docker Dev Environments (Docker Inc., v0.0.5)
  extension*  Manages Docker extensions (Docker Inc., v0.2.17)
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  sbom*       View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc., 0.6.0)
  scan*       Docker Scan (Docker Inc., v0.23.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.

To get more help with docker, check out our guides at https://docs.docker.com/go/guides/
```

`docker --help` 를 입력했을 때 많은 명령어들이 나오는데 모든 명령어에 대해 반드시 알고 있어야 할 필요는 없다. 일부 명령어는 다른 명령어로 대체되는 경우도 있다.

</br>

```
docker ps
```
모든 실행중인 컨테이너를 리스팅한다. 

</br>

```
# --help 를 붙여주면 docker ps 에 사용 가능한 모든 구성 옵션이 표시된다.
docker ps -a
```
더이상 실행되지 않는 중지된 컨테이너를 포함하여 과거에 있었던 모든 컨테이너가 표시된다. (삭제된 컨테이너는 표시되지 않는다.)

</br>

```bash
docker start <container id>
# 또는 
docker start <container name>
```

`docker run` 을 사용하면 이미지를 기반으로 새 컨테이너를 만들고 그 이후로 새 컨테이너가 시작된다.

app, dependency, source code가 변경되지 않고 이미지가 변경되지 않은 경우에는 새로운 컨테이너를 생성할 필요가 없다. 이때는 기존 컨테이너를 재시작하면 된다.

`docker run` 처럼 터미널을 차단하지는 않는다.
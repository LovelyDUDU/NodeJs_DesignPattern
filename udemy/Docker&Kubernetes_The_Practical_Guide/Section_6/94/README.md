# 94. Linux에 Docker Compose 설치하기
maxOS 및 Windows에는 Docker가 설치될 때 Docker Compose가 함께 설치되지만 Linux 시스템에서는 별도로 설치해야 한다.

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

docker-compose --version # to verify
```

참조: https://docs.docker.com/compose/install/
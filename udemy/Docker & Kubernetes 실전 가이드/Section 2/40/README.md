# 41. 모듈 요약

**Images** are the templates / blueprints for **Containers**, multiple **Containers** can be created based on one **Images**.

**Images** contain multiple layers (1 Instruction = 1 Layer) to optimize build speed (use chaching) and re-usability.

**Containers** can be listed (`docker ps`).
**Containers** can be removed (`docker rm`).
**Containers** can be stopped and started (`docker stop` / `docker start`)

**Images** are either downloaded (`docker pull`)
or created with a Dockerfile and docker build.

**Containers** are created with docker run IMAGE and can be configured with various options / flags.

**Images** can also be listed (`docker images`).
**Images** can be removed (`docker rmi`, `docker image prune`) and shared (`docker push` / `docker pull`)
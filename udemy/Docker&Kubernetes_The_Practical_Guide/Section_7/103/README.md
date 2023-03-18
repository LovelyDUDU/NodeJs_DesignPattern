# 103. 컨테이너에서 명령을 실행하는 다양한 방법

```bash
docker run -it node

docker run -it -d node

# 컨테이너 내부에서 npm init 실행. 
docker exec -it <container name> npm init
```

`docker exec` 명령을 사용하면 컨테이너가 실행하는 기본 명령 외에 실행중인 컨테이너 내에서 특정 명령을 실행할 수 있다.

컨테이너를 중지하지 않고 컨테이너 내부에 작성된 파일을 읽을 수 있기 때문에 유용하다. (실행중인 컨테이너에 대해서만 사용할 수 있다.)

일반적인 컨테이너 실행 명령어인 `run` 과 달리 컨테이너 상태를 디버깅하기 위한 용도로 주로 사용된다.

```
>> docker exec -it nice_ardinghelli npm init

This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help init` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: 

```

`docker run -it node npm init` 명령을 사용하면 `npm init` 명령으로 프로젝트를 생성하고 `npm init` 이 완료되면 컨테이너가 중지된다. (유용하지 않음)
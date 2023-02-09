# 31. 인터렉티브 모드로 들어가기

`docker run <container id>` 로 실행중인 컨테이너와는 상호 작용(interactive)할 수 없다. 

이대로 rnp.py를 도커로 실행시키면 에러가 난다.
```
user@MacBookPro DOCKER_COMPLETE % docker logs 8f98e9e9ec2c
Traceback (most recent call last):
  File "/app/rng.py", line 3, in <module>
    min_number = int(input('Please enter the min number: '))
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
EOFError: EOF when reading a line
Please enter the min number
```

이떄 `-i` 명령어 옵션을 추가하면 interactive 모드로 컨테이너를 시작할 수 있다. 

interactive 모드에서는 표준 입력을 열린 상태로 유지하며, attached 모드가 아닌 경우에도 컨테이너에 무언가 입력할 수 있다.

`-t` 옵션은 pseudo-TTY 인데 이건 터미널을 생성한다는 것을 의미한다.

즉 `-it` 옵션을 사용하면 컨테이너는 입력을 수신하고, 컨테이너에 의해 노출되는 터미널도 생기게 된다.

```
user@MacBookPro DOCKER_COMPLETE % docker run -it sha256:2efbfce0b9405b4b2093114f
Please enter the min number: 10
Please enter the max number: 100
74
```

`docker run` 명령어에 `-it` 옵션을 사용하여 컨테이너에 의해 노출된 psudo terminal과 통신하여 터미널에 뭔가 입력할 수 있게 되었다.

또한 사용자 입력을 수신하는 컨테이너 프로세스에도 연결된다.

컨테이너가 중지된 후 다시 입력하고 싶을 떄

 `docker start` 에 `-a`, `-i` 옵션을 사용하여 출력을 수신할 수 있다. `-t` 옵션이 필요없는 이유는 처음에 `-t` 옵션으로 컨테이너를 실행했기 때문이다. 
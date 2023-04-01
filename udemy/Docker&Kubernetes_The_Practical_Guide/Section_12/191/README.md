# 191. 실제 스케일링(scaling)
`kubectl scale` 명령어를 사용하면 스케일링 기능을 사용할 수 있다.

```bash
# deployment/first-app 을 스케일링 할 것이다.
# replica: pod의 인스턴스
# --replicas=3: 동일한 pod/컨테이너가 3번 실행중이라는 뜻 

kubectl scale deployment/first-app --replicas=3
```
# 202. 활성 프로브 (Liveness Probes)
쿠버네티스와 pod과 컨테이너가 정상인지 확인하기 위해 `livenessProbe` 키를 추가할 수 있다.

```yaml
...
livenessProbe:
  httpGet:
    path: /
    port: 8080
  periodSeconds: 10 # 작업 수행 빈도
  initialDeplaySeconds: 5 # 쿠버네티스가 처음으로 상태를 확인할 떄 까지 기다려야 하는 시간
...
```
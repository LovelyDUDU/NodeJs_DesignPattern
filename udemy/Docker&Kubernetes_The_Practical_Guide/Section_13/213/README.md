# 213. 두 번째 볼륨: "hostPath" 유형

replicas가 2이상일 때는 pod이 여러개 띄워지게 되고 그렇게 되면 pod별로 볼륨을 따로 쓰기 때문에 의도대로 동작하지 않는 문제가 발생한다.

이러한 문제를 해결하기 위해서 `hostPath` 를 사용하면 된다. 각각의 Pod 별로 특정 경로를 사용하는 것이 아니라 호스트 머신의 동일한 경로를 여러 pod들이 공유하는 것이다. (물론 동일한 pod에서 모든 요청을 처리하는 경우에만 유용하다)

```yaml
...
      volumes:
        - name: story-volume
          emptyDir: {}
...

# 를 

      volumes:
        - name: story-volume
          hostPath:
            path: /data # 데이터가 저장되어야하는 호스트 머신의 경로 (공유할 호스트 머신의 경로)
            type: DirectoryOrCreate # 경로를 처리하는 방법. 사용할 디렉토리가 이미 만들어져 있으면 그거 쓰고 없으면 생성

# 처럼 수정하면 된다.
```

hostPath는 하나의 노드에 특정적이라는 단점이 있다. (동일한 노드의 pod들 끼리만 공유)
# 212. 첫 번째 볼륨: "emptyDir" 유형

https://kubernetes.io/docs/concepts/storage/volumes

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: story-deployment
spec: 
  replicas: 1
  selector:
    matchLabels:
      app: story
  template:
    metadata:
      labels:
        app: story
    spec:
      containers:
        - name: story
          image: academind/kub-data-demo:1
          volumeMounts: # 컨테이너에 볼륨을 바인딩하는 부분
            - mountPath: /app/story # 마운트될 컨테이너 내부의 경로
              name: story-volume # 컨테이너 내부 경로에 사용할 볼륨 이름
      volumes: # 이 pod의 일부가 되어야 하는 모든 볼륨을 정의한다. 여기에 volumes 정의하면 이 pod의 모든 컨테이너가 해당 볼륨을 쓸 수 있음.
        - name: story-volume
          emptyDir: {}
```
### emptyDir

- 특정 구성은 없는 유형
- 디폴트 설정을 따름 (pod이 시작될 때 마다 단순히 새로운 빈 디렉토리를 생성)
- pod이 살아있는 한, 이 디렉토리를 활성 상태로 유지하고 데이터를 채움
- 컨테이너가 재시작되거나 제거되더라도 데이터가 유지된다.
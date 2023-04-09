# 217. 영구 볼륨 클레임 생성하기

### PVC yaml
```yaml
apiVersion: v1
kind: PersistentVolumeClaim # 리소스 종류
metadata:
  name: host-pvc
spec:
  volumeName: host-pv # 연결할 PV(Persist Volume)의 이름
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  resources:
    requests: 
      storage: 1Gi
```

### Deployment yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: story-deployment
spec: 
  replicas: 2
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
          volumeMounts:
            - mountPath: /app/story
              name: story-volume
      volumes:
        - name: story-volume
          persistentVolumeClaim: # PVC 정의 
            claimName: host-pvc # PVC 이름
```

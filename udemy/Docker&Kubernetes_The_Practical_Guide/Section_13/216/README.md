# 216. 영구 볼륨 정의하기

### hostPath 유형을 사용하는 영구 볼륨
```yaml
apiVersion: v1
kind: PersistentVolume # 영구 볼륨
metadata:
  name: host-pv
spec:
  capacity: # 용량
    storage: 1Gi
  volumeMode: Filesystem # Filesystem or Block (https://www.computerweekly.com/feature/Storage-pros-and-cons-Block-vs-file-vs-object-storage)
  storageClassName: standard
  accessModes: # 액세스 방법 (ReadWriteOnce, ReadOnlyMany, ReadWriteMany)
    - ReadWriteOnce 
  hostPath:
    path: /data
    type: DirectoryOrCreate
```

### access Modes 종류
- ReadWriteOnce
이 볼륨이 단일 노드에 의해, 읽기/쓰기 볼륨으로 마운트될 수 있음을 의미한다. 따라서 여러 Pod에 의해 수행되지만, 모두 동일한 노드에 있어야 한다.

- ReadOnlyMany
읽기 전용이지만, 여러 노드에서 요청할 수 있음을 의미한다. 따라서 서로 다른 노드의 여러 Pod가 동일한 영구 볼륨을 요청할 수 있다. (hostPath는 없음)

- ReadWriteMany
ReadWriteMany 읽기/쓰기 볼륨으로 여러 노드에서 요청할 수 있다. (hostPath는 없음)
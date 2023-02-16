# 48. 첫번째, 실패한 시도

Dockerfile에 VOLUME 명령을 추가하여 볼륨을 추가할 수 있다.
```
...
VOLUME [ "/app/feedback" ]
...
```

이때 "" 안에들어가는 파일의 경로는 매핑할 컨테이너 내부 폴더의 경로이다.

VOLUME 만 추가하고 컨테이너를 빌드하면 에러가 발생하는데 이때 발생하는 에러는 `cross-device` 이다.

cross-device는 장치간 링크를 허용하지 않는다는 에러로, 어플리케이션을 통해서 만든 test.txt파일을 temp폴더에서 feedback 폴더로 옮기려고 하다가 발생한다.

도커는 실제로 파일을 컨테이너 파일 시스템 내부의 다른 폴더로 이동하지 않는데 코드에서 rename을 사용하려고 해서 발생한 에러이다. (컨테이너 밖으로 이동시킬 뿐이다.)

그렇기에 rename을 copyFile로 교체하고 코드를 수정해야 한다.
```javascript
await fs.rename(tempFilePath, finalFilePath);

// rename을 아래처럼 수정한다.
// 파일을 복사하고, 기존 파일을 삭제한다.
await fs.copyFile(tempFilePath, finalFilePath);
await fs.unlink(tempFilePath);
```

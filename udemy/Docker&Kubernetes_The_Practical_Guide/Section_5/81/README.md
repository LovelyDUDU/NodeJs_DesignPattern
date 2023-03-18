# 81. MongoDB 서비스 도커화 하기
MongoDB용 컨테이너를 위해 Docker Hub에서 공식 Mongo 이미지를 사용한다. 

```javascript
// /backend/app.js
mongoose.connect(
  'mongodb://localhost:27017/course-goals',
  {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  },
  (err) => {
    if (err) {
      console.error('FAILED TO CONNECT TO MONGODB');
      console.error(err);
    } else {
      console.log('CONNECTED TO MONGODB');
      app.listen(80);
    }
  }
);

```


MongoDB의 default port는 27017 이다. 컨테이너 내의 포트와 로컬포트를 27017로 통일시켰다.

```bash
# --name: 이미지 이름 설정
# -d: detached
# -p: 호스트머신(좌)과 컨테이너(우) 간의 포트를 매핑
docker run --name mongodb --rm -d -p 27017:27017 mongo
```
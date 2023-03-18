# 73. 컨테이너 간 통신: 기본 솔루션

새로운 컨테이너에 mongodb를 띄웠다. (`docker run -d --name mongodb mongo`)

그리고 `docker inspect mongodb` 로 mongodb의 ip 주소를 확인하고, `app.js` 에 소스코드를 일부 수정했다.

```javascript
"mongodb://host.docker.internal:27017/swfavorites"
// ->
"mongodb://172.17.0.3:27017/swfavorites"  
```

코드를 수정했으면 이미지를 새로 빌드하고 (`docker build -t favorites-node .`)

컨테이너를 실행한다. (`docker run --name favorites -d --rm -p 3000:3000 favorites-node`)

GET peoples - `http://localhost:3000/people`

POST favorites - `http://localhost:3000/favorites`

Body params
```JSON
{
  "name": "A New Hope",
  "type": "movie",
  "url": "https://swapi.dev/api/films/1/"
}
```

GET favorites - `http://localhost:3000/favorites`

이렇게 하면 되긴 되는데 `app.js`에서 mongoDb의 컨테이너 IP 주소를 하드코딩한 것이 추후 문제가 될 수 있다. (mongodb 컨테이너 IP 주소가 변경될 때 마다 새 이미지를 빌드해야함)
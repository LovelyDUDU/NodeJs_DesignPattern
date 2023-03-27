# 150. 업데이트 & Target 아키텍처
```bash
# 현재 구조
AWS ECS
ECS Task
- NodeJS REST API

MongoDB Atlas
- Mongo DB

AWS Load Balancer
```

```bash
# 목표 구조
AWS ECS
ECS Task
- NodeJS REST API
- React SPA

MongoDB Atlas
- Mongo DB

AWS Load Balancer
```

User -> AWS Load Balancer -> React SPA -> NodeJS RestAPI -> MongoDB Atlas
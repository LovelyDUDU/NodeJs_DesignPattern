# 146. 현재 아키텍처
```bash
AWS ECS
ECS Task
- NodeJS REST API
- Mongo DB
  ─ Volume 
    - AWS EFS Storage
AWS Load Balancer
```

User -> AWS Load Balancer -> NodeJS RestAPI -> MongoDB -> (Volume) -> AWS EFS Storage
# 142. NodeJS 백엔드 컨테이너 구성하기 & 143. 두 번째 컨테이너 & 로드 밸런서 배포하기

백엔드 컨테이너 구성 순서
클러스터 생성 - 클러스터는 컨테이너 주변 네트워크
1. Select cluster template(클러스터 템플릿 선택)
   1. choose Networking only 
2. Configure cluster
   1. Set Cluster name
   2. Create VPC (Aws가 클러스터의 모든 컨테이너에 대해 이 클러스터를 private cluster로 설정)

클러스터 안에 Task 생성
1. Task Definition (태스크 생성)
   1. FARGATE 로 태스크 생성
   2. Set Task Definition Name
   3. Set ecsTaskExecutionRole 
   4. Set Task memory and CPU
   5. Container Definition (Add container) - backend container
      1. Set Container name, image
      2. Port mapping (80)
      3. Set Envirionment
         1. Command: node,app.js
         2. Variables: 백엔드 서버에서 사용될 ENV 설정값 
   6. Container Definition (Add container) - mongodb container
      1. Set Container name, image
      2. Port mapping (27017)
      3. Set Envirionment
         1. Variables: mongo db 서버에서 사용될 ENV 설정값
   
서비스 생성 
1. Configure Service(서비스 구성)
   1. Task Definition: goals
   2. Cluster: goals-app
   3. Service Name: goals_service
   4. Number of tasks: 1
2. Configure Network (네트워크 구성)
   1. Cluster VPC 설정 (VPC는 클러스터를 생성할 때 만들어짐)
   2. 서브넷 추가
   3. 로드밸런싱 선택
      1. Application Load Balancer 선택
      2. Load balancer name 에서 생성한 LB 선택 (ecs-lb)
      3. set target group name
3. Set Auto Scaling

LB 생성
1. Configure Load Balancer 
   1. Set lb name (ecs-lb)
   2. Set scheme (internet-facing)
   3. Set Load Balancer Port (80)
   4. Connect to the same VPC as the VPC selected in Service creation
2. Configure Security Settings
3. Configure Security Groups
4. Configrue Routing
   1. Set target type IP 

LB 수정
1. Target Groups 수정
   1. LB 의 Health Check Settings 수정
      1. Health Chck path를 / -> /goals로 수정
2. Load Balancers 수정 
   1.  Security groups 수정 
       1.  default security group 외에 goals 보안그룹 추가


# 144. 안정적인 도메인을 위해 로드 밸런서 사용하기

Public IP 는 업데이트된 컨테이너를 배포할 때 마다 변경된다.

로드 밸런서는 요청을 리디렉션하는 서비스의 상태를 확인하고 서비스가 비정상임을 발견하면 서비스를 종료하고 다시 시작한다. 이때 서비스의 상태를 체크하는 부분이 Health Check path 이다.

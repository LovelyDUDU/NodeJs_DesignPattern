# 142. NodeJS 백엔드 컨테이너 구성하기
백엔드 컨테이너 구성 순서
1. 클러스터 템플릿 네트워크 선택
2. Create VPC 선택
3. 태스크 생성 (Task Definition)
   1. backend 컨테이너 추가
   2. mongodb 컨테이너 추가
4. 서비스 생성 
   1. 서비스 구성 
      1. 서비스 이름: goals-app
      2. 작업개수: 1
   2. 네트워크 구성
      1. 서브넷 추가
      2. 로드밸런스 설정
         1. LB 생성 (application lb)
         2. LB 설정

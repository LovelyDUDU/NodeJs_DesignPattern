# 62. 환경 변수 & 보안

db 정보와 같은 보안 데이터는 Dockerfile에 직접 포함시키면 안된다.

보안 데이터는 런타임에만 사용되는 별도의 환경 변수 파일로 이동시켜서 `Docker run` 으로 컨테이너를 실행할 때만 사용한다.
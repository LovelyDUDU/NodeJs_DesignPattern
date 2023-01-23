1장 HTTP 개관

HTTP = Hypertext Transfer Protocol

 

1.1 HTTP: 인터넷의 멀티미디어 배달부

HTTP는 신뢰성 있는 데이터 전솔 프로토콜을 사용하기 때문에, 데이터가 전송 중 손상되거나 꼬이지 않음을 보장한다.

 

1.2 웹 클라이언트와 서버

Web Client ------- request ------> Web Server

Web Client <----- response ------ Web Server

 

1.3 리소스

 

1.3.1 미디어 타입

mime type = data format type

ex) jpeg = image/jpeg

 

1.3.2 URI

URI 에는 URL과 URN 이 있다. 대부분의 URI 는 URL 이다. 

https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=coding 

https:// : 프로토콜

search.naver.com : 서버

search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=coding  : 리소스

 

1.3.3 URL

URL(Uniform Resource Locator) 은 리소스가 정확히 어디에 있고, 어떻게 접근할 수 있는지 알려준다.

 

1.3.4 URN

URN (Uniform Resource Name) 이다.

 

1.4 트랜잭션

HTTP 트랜잭션은 client 에서 server로 보내는 request 명령과 server가 client에게 돌려주는 response 결과로 구성되어 있다.

 

1.4.1 메서드

HTTP는 HTTP 메서드라고 불리는 여러가지 종류의 요청 명령을 지원한다. 모든 HTTP request 메세지는 한개의 메서드를 가진다.

GET, PUT, DELETE, POST, HEAD 이렇게 흔히 쓰인다.

 

1.4.2 상태코드

모든 HTTP response 는 상태 코드(status code)와 함께 반환된다. 

 

1.4.3 웹페이지는 여러 객체로 이루어질 수 있다.

보통 하나의 작억을 수행하기 위해서 여러 http 트랜잭션을 수행한다. 

 

1.5 메세지

request = start line + request header

response  = start line + response header + body

 

1.5.1 간단한 메세지의 예

 

1.6 TCP 커넥션

TCP = Transmission Control Protocol

 

1.6.1 TCP/IP

TCP는 오류없는 데이터 전송, 순서보장, 조각나지 않는 데이터 스트림 의 특징을 가진다.

 

1.6.2 접속, IP 주소 그리고 포트번호

IP 주소와 포트번호를 이용하여 클라이언트는 TCP/IP로 쉽게 통신이 가능하다.

 

1.6.3 텔넷을 이용한 실제 예제

 

1.7 프로토콜 버전

 

1.8 웹의 구성요소

Proxy, Cache, Gateway, Tunnel 등이 있다.

 

1.8.1 프락시

클라이언트와 서버 사이에 위치하여, 클라이언트의 모든 HTTP요청을 받아 서버에 전달한다.

 

1.8.2 캐시

성능 향상을 위해 자주찾는 데이터를 저장해둔다. (서버까지 가는 일을 최소화한다.)

 

1.8.3 게이트웨이

다른 서버들의 중개자로 동작한다. 주로 HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용된다.

 

1.8.4 터널

두 커넥션 사이에서 raw data 를 열어보지 않고 그대로 전달해주는 HTTP 어플리케이션이다.

 

1.8.5 에이전드

 

1.9 시작의 끝

 

1.10 추가정보

 

1.10.1 HTTP 프로토콜에 대한 정보
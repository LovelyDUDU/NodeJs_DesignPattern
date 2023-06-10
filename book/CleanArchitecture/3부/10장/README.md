# 10장 ISP: 인터페이스 분리 원칙

ISP(Interface Segregation Principle)은 언어 종류에 따라 영향받는 정도가 다르다. 

ISP는 필요 이상으로 많은 것을 포함하는 모듈에 의존하지 말자는 의미를 내포하고 있다.

```
System S -> Framework F > Database D
```
이 상황에서 S는 F에 의존하며, F는 다시 D에 의존한다. D내부가 변경되었을 떄, F를 재배포해야 할 수 도 있고 심지어 S까지 재배포 해야할 수도 있다. 

D 내부의 기능중 F와 S와 관련이 없는 기능에 문제가 발생해도 F와 S에 영향을 줄 수도 있다.
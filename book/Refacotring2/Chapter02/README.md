# Chapter 02 - 리팩토링 원칙

## 리팩토링 정의

리팩토링이라는 말의 정의는 소프트웨어의 겉보기 동작은 그대로 유지한 채, 코드를 이해하고 수정하기 쉽도록 내부 구조를 변경하는 기법이다. 

리팩토링의 목적은 코드를 이해하고 수정하기 쉽게 만드는 것이며, 리팩토링 전/후로 코드가 똑같이 동작해야 한다.

성능을 최적화는 리팩토링과 목적이 다르다.

</br>

## 두개의 모자

개발을 할 때 목적을 `기능 추가` 와 `리팩토링` 을 명확히 해야한다. 한가지를 할때 그것에 전념해야 한다.

</br>

## 리팩터링 하는 이유
### 소프트웨어 설계가 좋아진다.
- 설계가 튼튼하면 유지보수과 기능추가가 쉽다.
### 소프트웨어를 이해하기 쉬워진다.
- 시간이 지나고 다시 코드를 봤을 때 금방 이해된다. 
### 버그를 쉽게 찾을 수 있다.
### 프로그래밍 속도를 높일 수 있다.
- 내부 설계가 잘 된 소프트웨어는 새로운 기능을 추가할 지점과 어떻게 고칠지를 쉽게 찾을 수 있다.

</br>

## 리팩토링 타이밍
글쓴이의 리팩토링 타이밍을 잡는 3의 법칙
> 1. 처음에는 그냥 한다.
> 2. 비슷한 일을 두 번째로 하게 되면 당황스럽겠지만 일단 계속한다.
> 3. 비슷한 일을 세 번째로 하게 되면 리팩토링 한다.

</br>

### 준비를 위한 리팩토링
`함수 매개변수화하기`로 중복을 줄인다.

</br>

### 이해를 위한 리팩토링
코드가 하는 일을 파악해야 한다. 변수명/함수명을 적절하게 수정하거나, 긴 함수를 잘게 나눈다.

</br>

### 쓰레기 줍기 리팩토링 (수시로 리팩토링 하기)
간단한건 즉시 고치고, 시간이 좀 걸리는 일은 짧게 메모(주석)을 달아뒀다가 일을 끝내고 나서 처리한다. 

</br>

### 오래 걸리는 리팩토링
오래 걸리는 리팩토링은 작게 쪼개서 수시로 한다. 

</br>

### 코드 리뷰에 리팩토링 활용하기
코드 리뷰를 정기적으로 수행해서 깔끔한 코드를 유지하자.

</br>

### 리팩토링하지 말아야 할 때
굳이 수정할 필요가 없을 떄, 외부 API 다루듯 호출해야 쓰는 코드일 때, 처음부터 새로 작성하는게 쉬울 떄

</br>

## 리팩토링시 고려할 문제
리팩토링에 딸려오는 문제도 있다.

</br>

### 새 기능 개발 속도 저하
리팩토링의 궁극적인 목적은 개발 속도를 높여서, 더 적은 노력으로 더 많은 가치를 창출하는 것이다.

즉 리팩토링을 더 자주 하도록 노력하자. 리팩토링이 새 기능을 빨리 추가할 수 있게 도와준다.

</br>

### 코드 소유권
- 코드 소유권이 나누어져 있으면 리팩토링에 방해된다.
- 코드의 소유권을 팀에 두는 것이 좋다. 

</br>

### 브랜치
마스터 브랜치와 통합하는 작업을 자주하여(가급적 매일) 마스터 브랜치와 다른 브랜치간의 격차를 최소화하자.

</br>

### 테스팅
테스팅은 리팩토링의 핵심이다. 리팩토링은 전/후에 프로그램의 겉보기 동작이 똑같이 유지되어야 하는데 이를 검증하기 위한 도구가 테스팅이다.

</br>

### 레거시 코드
레거시 코드를 파악할 때 리팩토링이 굉장히 도움된다. (테스트 코드가 있으면 아주 좋다)

서로 관련된 부분끼리 나눠서 하나씩 공략하자.

레거시 시스템의 규모가 클수록 리팩토링은 코드를 이해하고 개선하는데 큰 도움이 된다.
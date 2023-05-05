# 중개자

객체의 대표적인 기능 하나로, 캡슐화가 있다. 캡슐화하는 과정에서 위임이 자주 활용이 되는데 지나치면 문제가 된다.

</br>

## 중개자 제거하기(7.8)
단순히 전달만 하는 위임 메서드들이 너무 많아지는 경우 서버 클래스가 그저 중개나 역할로 전락할 수 있다. 이런 경우 클라이언트가 위임 객체를 직접 호출하는게 나을 수 있다.

### before
```js
manager = aPerson.manager;

class Person {
  get manager() { return this.department.manager; }
}
```

### after
```js
// manager()를 거치지 않고 바로 department(위임 객체)를 직접 호출하게 바꾸었다.
manager = aPerson.department.manager;
```

</br>

## 함수 인라인하기(6.2)
목적이 분명히 드러나는 이름의 짤막한 함수를 이용해야 코드가 명료해지고 이해하기 쉬워진다. 하지만 때로는 함수 본문이 이름만큼 명확한 경우도 있다. 또는 함수 본문 코드를 이름만큼 깔끔하게 리팩토링할 때도 있다. 이럴 때는 그 함수를 제거한다. 간접 호출은 유용할 수도 있지만 쓸데없는 간접 호출은 거슬릴 뿐이다.

리팩토링 과정에서 잘못 추출된 함수들도 다시 인라인하고, 간접 호출을 너무 과하게 쓰는 코드도 인라인하자. 가령 다른 함수로 단순히 위임하기만 하는 함수들이 많아서 위임관계가 복잡하게 얽혀 있으면 인라인하자. 

다만 재귀 호출, 반환문이 여러개 등 상황이 복잡한 경우에는 무작정 적용하지 말자. 
### before
```js
function getRaing(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```

### after
```js
function getRaing(driver) {
  return (driver.numberOfLateDeliveries > 5) ? 2 : 1;
}
```

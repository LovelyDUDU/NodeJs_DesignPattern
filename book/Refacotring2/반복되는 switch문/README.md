# 반복되는 switch문
switch 문은 최대한 리팩토링 해야 한다. 중복된 switch문이 문제가 되는 이유는 조건절을 하나 추가할 때마다 다른 switch문도 모두 찾아서 함께 수정해야 하기 때문이다.

</br>

## 조건부 로직을 다형성으로 바꾸기(10.4)

복잡한 조건부 로직은 프로그래밍에서 해석하기 가장 난해한 대상에 속한다. 조건부 구조는 클래스와 다형성을 이용하여 더 확실하고 직관적인 분리가 가능하다.

### before

```javascript
switch (bird.type) {
  case "유럽 제비":
    return "보통이다.";
  case "아프리카 제비":
    return (bird.numberOfCoconuts > 2) ? "지쳤다" : "보통이다.";
  case "노르웨이 파랑 앵무":
    return (bird.voltage > 100) ? "그을렸다" : "예쁘다.";
  default:
    return "알 수 없다.";
}
```

### after

```javascript
class EuropeanSwallow {
  get plumage() {
    return "보통이다."; 
  }

  // ...
}
class AfricanSwallow {
  get plumage() {
    return (bird.numberOfCoconuts > 2) ? "지쳤다" : "보통이다.";
  }
  
  // ...
}

class NorwegianBlueParrot {
  get plumage() {
    return (bird.voltage > 100) ? "그을렸다" : "예쁘다.";
  }

  // ...
}
```

</br>

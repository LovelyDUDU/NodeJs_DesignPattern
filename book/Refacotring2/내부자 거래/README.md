# 내부자 거래

모듈 사이의 데이터 거래가 많으면 결합도가 높아진다. 일이 돌아가게 하려면 거래가 이뤄질 수 밖에 없지만
그 양을 최소로 줄이고 모두 투명하게 처리해야 한다.

</br>

## 함수 옮기기(8.1)

좋은 소프트웨어 설계의 핵심은 모듈화가 얼마나 잘 되어 있느냐를 뜻하는 모듈성이다. 모듈성이란 프로그램의 어딘가를 수정하려 할 때 해당 기능과 깊이 관련된 작은 일부만 이해해도 가능하게 해주는 능력이다. 모듈성을 높이려면 서로 연관된 요소들을 함께 묶고, 요소 사이의 연결관계를 쉽게 찾고 이해할 수 있도록 해야 한다.

### before

```javascript
class Account {
  get bankCharge() { // 은행 이자 계산 
    let result = 4.5;
    if (this._daysOverdrawn > 0) result += this.overdraftCharge;
    return result;
  }
  get overdraftCharge() { // 초과 인출 이자 계산
    if (this.type.isPremium) {
      const baseCharge = 10;
      if (this.daysOverdrawn <= 7) {
        return baseCharge;
      }
      else {
        return baseCharge + (this.daysOverdrawn - 7) * 0.85;
      }
    } else {
      return this.daysOverdrawn * 1.75;
    }
  }
}
```

### after

```javascript
class Account {
  get bankCharge() {
    let result = 4.5;
    if (this._daysOverdrawn > 0) result += this.type.overdraftCharge(this.daysOverdrawn);
    return result;
  }
}

class AccountType {
  get overdraftCharge(daysOverdrawn) {
    if (this.isPremium) {
      const baseCharge = 10;
      if (daysOverdrawn <= 7) {
        return baseCharge;
      }
      else {
        return baseCharge + (daysOverdrawn - 7) * 0.85;
      }
    } else {
      return daysOverdrawn * 1.75;
    }
  }
}
```

</br>

## 필드 옮기기(8.2)

주어진 문제에 적합한 데이터 구조를 활용하면 동작 코드는 자연스럽게 단순하고 직관적으로 짜여진다. 반면 데이터 구조를 잘못 선택하면 아귀가 맞지 않는 데이터를 다루기 위한 코드로 범벅이 된다. 이해하기 어려운 코드가 만들어지는 데서 끝나지 않고, 데이터 구조 자체도 그 프로그램이 어떤 일을 하는지 파악하기 어렵게 한다. 즉 데이터 구조가 중요하다.

데이터 구조는 한번에 제대로 하기 어렵다. 경험과 도메인 주도 설계 같은 기술이 충분함에도 불구하고 실수는 발생한다. 현재 데이터 구조가 적철이 않음을 깨닫게 되면 곧바로 수정하는 것이 좋다.

### before

```js
class Customer {
  constructor(name, discountRate) {
    this._name = name;
    this._discountRate = discountRate;
    this._contract = new CustomerContract(dateToday());
  }
  get plan() { return this._plan; }
  get discountRate() { return this._discountRate; }
}

class CustomerContract {
  constructor(startDate) {
    this._startDate = startDate;
  }
}
```

### after

```js
// 할인율을 뜻하는 discountRate 필드를 Customer 에서 CustomerContract 로 옮겼다.
class Customer {
  constructor(name, discountRate) {
    this._name = name;
    this._contract = new CustomerContract(dateToday());
    this._setDiscountRate(discountRate);
  }

  get plan() { return this._plan; }
  get discountRate() { return this._contract.discountRate; }
  _setDiscountRate(aNumber) { this._contract.discountRate = aNumber; }
}

class CustomerContract {
  constructor(startDate, discountRate) {
    this._startDate = startDate;
    this._discountRate = discountRate;
  }
}
```

</br>

## 위임 숨기기(7.7)

모듈화 설계를 제대로 하는 핵심은 캡슐화이다. 캡슐화는 모듈들이 시스템의 다른 부분에 대해 알아야 할 내용을 줄여준다. 캡슐화가 잘 되어 있다면 무언가를 변경해야할 때 함께 고려해야할 모듈 수가 적어져서 코드를 변경하기가 훨씬 쉬워진다. 물론 캡슐화의 기능이 단순히 필드를 숨기는 것만 있는 것은 아니다.

예를들어 서버 객체의 필드가 가리키는 객체(위임 객체)의 메서드를 호출하려면 클라이언트는 이 위임객체를 알아야 한다. 위임 객체의 인터페이스가 바뀌면 이 인터페이스를 사용하는 모든 클라이언트가 코드를 수정해야한다. 하지만 위임을 숨겨 이러한 의존성을 없애면 위임 객체가 수정되더라도 서버 코드만 수정하면 된다.

### before

```js
// 위임객체가 수정되면 클라이언트에서 해당 객체를 호출하는 부분을 모두 바꿔야 한다.
manager = aPerson.department.manger;
```

### after

```js
// 위임을 숨기게 되면 클라이언트에서 해당 객체를 호출하는 부분은 그대로 두고, 서버 코드만 수정하면 된다.
manager = aPerson.manager;

class Person {
  get manager { return this.department.manager; }
}
```

</br>

## 서브클래스를 위임으로 바꾸기(12.10)

### before

```javascript
class Order {
  get daysToShip() {
    return this._warehouse.daysToShip;
  }
}

class PriorityOrder extends Order {
  get daysToShip() {
    return this._priorityPlan.daysToShip;
  }
}
```

### after

```javascript
class Order {
  get daysToShip() {
    return (this.priorityDelegate) ? this.priorityDelegate.daysToShip : this._warehouse.daysToShip;
  }
}

class PriorityOrderDelegate {
  get daysToShip() {
    return this._priorityPlan.daysToShip;
  }
}
```

</br>

## 슈퍼클래스를 위임으로 바꾸기(12.11)

### before

```js
class List { ... }
class Stack extends List {
  ...
}
```

### after

```js
class Stack {
  constructor() {
    this._storage = new List();
  }
}

class List {
  ...
}
```

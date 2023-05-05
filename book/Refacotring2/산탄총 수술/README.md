# 산탄총 수술
산탄총 수술은 뒤엉킨 변경과 비슷하면서도 정반대다. 변경할 부분이 코드 전반에 퍼져 있다면 찾기도 어렵고 꼭 수정해야 할 곳을 지나치기 쉽다.

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

## 여러 함수를 클래스로 묶기(6.9)

클래스는 데이터와 함수를 하나의 공유 환경으로 묶은 후, 다른 프로그램 요소와 어우러질 수 있도록 그중 일부를 외부에 제공한다.

공통 데이터를 중심으로 긴밀하게 엮여 작동하는 함수 무리를 발견하면 클래스 하나로 묶자.

클래스로 묶으면 이 함수들이 공유하는 공통 환경을 더 명확하게 표현할 수 있고, 각 함수에 전달되는 인수를 줄여서 객체 안에서의 함수 호출을 간결하게 만들 수 있다.

객체를 시스템의 다른 부분에 전달하기 위한 참조를 제공할 수도 있다.

이미 만들어진 함수들을 재구성할 떄는 물론, 새로 만든 클래스와 관련하여 놓친 연산을 찾아서 새 클래스의 메서드로 뽑아내는 데도 좋다.

### before

```javascript
function base(aReading) {}
function taxableCharge(aReading) {}
function calculateBaseCharge(aReading) {}
```

### after

```javascript
class Reading {
  constructor(data) {
    this._customer = data.customer;
    this._quantity = data.quantity;
    this._month = data.month;
    this._year = data.month;
  }
  function base() { ... }
  function taxableCharge() { ... }
  function calculateBaseCharge() { ... }
}
```

</br>

## 여러 함수를 변환 함수로 묶기(6.10)

소프트웨어는 데이터를 입력받아서 여러 가지 정보를 도출하곤 한다. 이렇게 도출된 정보는 여러 곳에서 사용될 수 있는데, 그러다 보면 이 정보가 사용되는 곳마다 같은 도출 로직이 반복되기도 한다. 이런 도출 작업들은 한곳에 모아두면 검색과 갱신을 일관된 장소에서 처리할 수 있고 로직 중복도 막을 수 있다.

이렇게 하기 위한 방법으로 변환 함수(transform)를 사용할 수 있다. **변환 함수는 원본 데이터를 입력받아서 필요한 정보를 모두 도출한 뒤, 각각의 출력 데이터의 필드에 넣어 반환한다.** 이렇게 해두면 도출 과정을 검토할 일이 생겼을 때 변환 함수만 살펴보면 된다.

### before

```js
function base(aReading) { ... }
function taxableCharge(aReading) { ... }
```

### after

```js
function enrichReading(argReading) {
  const aReading = _.cloneDeep(argReading);
  aReading.baseCharge = base(aReading);
  aReading.taxableCharge = taxableCharge(aReading);
  return aReading;
}
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

</br>

## 클래스 인라인하기(7.6)
"클래스 인라인하기"은 "클래스 추출하기"를 거꾸로 돌리는 리팩토링이다. 보통 역할을 옮기는 리팩토링을 하고나니 특정 클래스에 남은 역할이 거의 없을 때 제 역할을 못하는 클래스가 생긴다. 이때 클래스를 인라인하여 가장 많이 사용하는 클래스로 흡수시키자.

뿐만 아니라 두 클래스의 기능을 재분배하고 싶을 때도 클래스를 인라인한다. 클래스를 인라인해서 하나로 합친 다음 새로운 클래스를 추출하는게 쉬울 수도 있기 때문이다.

### before
```js
// 제 역할을 하지 못하는 소스 클래스(TelephonNumber)를 
class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  get number() { return this._number; }
}

// 타깃 클래스(Person)에 인라인하자.
class Person {
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  get officeNumber() { return this._telephoneNumber.number; }
}

```

### after
```js
class Person {
  get officeAreaCode() { return this._officeAreaCode; }
  get officeNumber() { return this._officeNumber; }
}
```

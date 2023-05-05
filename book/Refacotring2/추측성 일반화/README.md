# 추측성 일반화
미래를 대비해 작성한 부분을 실제로 사용하게 되면 다행이지만, 그렇지 않는다면 쓸데없는 낭비일 뿐이다. 걸리적거리는 코드는 치우자.

</br>

## 계층 합치기(12.9)

클래스 계층구조를 리팩토링하다 보면 어떤 클래스와 그 부모가 너무 비슷해져서 더는 독립적으로 존재해야 할 이유가 사라지는 경우가 생기기도 한다. 이럴때는 계층을 하나로 합쳐주자

### before
```js
class Employee { ... }

class SalesPerson extends Employee { ... }
```

### after
```js
class Employee { ... }
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

</br>

## 함수 선언 바꾸기(6.5)

### 함수 이름 바꾸기(간단한 절차)

### before

```javascript
//(cirucm = 둘레)
function circum(radis) {
  return 2 * Math.PI * radius;
}
```

### after

```javascript
// (circumference = 원의 둘레)
function circumference(radis) {
  return 2 * Math.PI * radius;
}
```

</br>

### 매개변수 추가하기

### before

```javascript
class Book {
  constructor(){}

  addReservation(customer) {
    this._reservcations.push(customer);
  }
}
```

### after

```javascript
// (우선순위 기능이 추가된 경우)
class Book {
  constructor(){}

  addReservation(customer) {
    this.zz_addReservation.push(customer, false);
  }

  zz_addReservation(customer, isPriority) {

    // assert 를 사용하여 새로 추가한 매개변수를 실제로 사용하는지 확인이 가능하다.
    assert(isPriority === true || isPriority === false);
    this._reservations.push(customer);
  }
}
```

</br>

### 매개변수를 속성으로 바꾸기

### before

```javascript
function inNewEngland(aCustomer) {
  return ["MA", "CT", "ME"].includes(aCustomer.address.state);
}

const newEnglanders = someCustomers.filter(c => inNewEngland(c));
```

### after

```javascript
function inNewEngland(stateCode) {
  return ["MA", "CT", "ME"].includes(stateCode);
}

// state 식별 코드를 매개변수로 받도록 하여 고객에 대한 의존성을 제거하였다. (더 넓은 문맥에서 활용할 수 있게 됨)
const newEnglanders = someCustomers.filter(c => inNewEngland(c.address.state));
```

</br>

## 죽은 코드 제거하기(8.9)
코드가 더 이상 사용되지 않게 됐다면 지우자. 다시 필요해질 날이 온다면 git history를 살펴보자. 예전에는 죽은 코드를 주석 처리했지만 이때는 git이 보편화되지 않았던 시절이다. 
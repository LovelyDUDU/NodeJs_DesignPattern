# 서로 다른 인터페이스의 대안 클래스들
클래스를 사용할 때의 큰 장점은 필요에 따라 언제든 다른 클래스로 교체할 수 있다는 것이다. 

단, 교체하려면 인터페이스가 같아야 한다. 
1. "함수 선언 바꾸기"로 메서드 시그니처를 일치시키고, 
2. "함수 옮기기"를 이용하여 인터페이스가 같아질 때까지 필요한 동작들을 클래스 안으로 밀어 넣는다. 
3. 그리고 중복 코드가 생기면 "슈퍼클래스 추출하기"를 적용할지 고려해본다.


</br>

## 함수 선언 바꾸기(6.5)

### 함수 이름 바꾸기(간단한 절차)

#### before 
```javascript
//(cirucm = 둘레)
function circum(radis) {
  return 2 * Math.PI * radius;
}
```

#### after 
```javascript
// (circumference = 원의 둘레)
function circumference(radis) {
  return 2 * Math.PI * radius;
}
```
</br>

### 매개변수 추가하기

#### before 
```javascript
class Book {
  constructor(){}

  addReservation(customer) {
    this._reservcations.push(customer);
  }
}
```

#### after 
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

#### before
```javascript
function inNewEngland(aCustomer) {
  return ["MA", "CT", "ME"].includes(aCustomer.address.state);
}

const newEnglanders = someCustomers.filter(c => inNewEngland(c));
```

#### after 
```javascript
function inNewEngland(stateCode) {
  return ["MA", "CT", "ME"].includes(stateCode);
}

// state 식별 코드를 매개변수로 받도록 하여 고객에 대한 의존성을 제거하였다. (더 넓은 문맥에서 활용할 수 있게 됨)
const newEnglanders = someCustomers.filter(c => inNewEngland(c.address.state));
```

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

## 슈퍼클래스 추출하기(12.8)

비슷한 일을 하는 두 클래스가 보이면 상속 메커니즘(extends)를 이용해서 비슷한 부분을 공통의 슈퍼클래스로 옮겨 담을 수 있다.

### before

```javascript
class Department {
  get totalAnnualCost() {}
  get name() {}
  get headCount() {}
}

class Employee {
  get annualCost() {}
  get name() {}
  get id() {}
}
```

### after

```javascript
class Party {
  get name() {}
  get annualCost() {}
}

class Department extends Party{
  get headCount() {}
  // get totalAnnualCost() {} 
  // get name() {} 
}

class Employee extends Party{
  get id() {}
  // get annualCost() {} 
  // get name() {} 
}
```

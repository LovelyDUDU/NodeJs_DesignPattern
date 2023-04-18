# 기이한 이름

함수, 모듈, 변수, 클래스 등은 그 이름만 보고도 각가 무슨 일을 하고 어떻게 사용해야 하는지 명확히 알 수 있도록 신경써서 이름을 지어야 한다.

이름만 잘 지어도 나중에 문맥을 파악하는데 헤매는 시간을 크게 절약할 수 있다.

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

## 변수 이름 바꾸기(6.7)
### before
```javascript
const a = height * weight;
```

### after
```javascript
const area = height * weight;
```


## 필드 이름 바꾸기(9.2)
### before
```javascript
class Organization {
  get name() {}
}
```

### after
```javascript
class Organization {
  get title() {}
}
```

# 주석
주석을 남겨야겠다는 생각이 들면, 가장 먼저 주석이 필요 없는 코드로 리팩토링해보자.

</br>

## 함수 추출하기(6.1)
독립된 함수로 만드는 기준은 코드의 길이, 코드의 사용 횟수 등 사람마다 다양하다.

코드를 함수로 묶는 기준은 '목적과 구현을 분리'하는 방식이 가장 적합하는 것이 저자의 의견이다.

코드를 보고 무슨 일을 하는지 파악하는데 한참 걸린다면 그 부분을 함수로 추출한 뒤 '무슨 일'에 걸맞는 이름을 짓는다.

코드의 길이는 중요하지 않다. (함수의 코드 길이가 단 한줄짜리라도 함수명만 잘 지었으면 괜찮다는 뜻)

### before
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding();

  console.log(`고객명: ${invoice.customer}`);
  console.log(`채무액: ${outstanding}`);
}
```
### after
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding();
  printDetails(outstanding);

  function printDetails(outstanding) {
    console.log(`고객명: ${invoice.customer}`);
    console.log(`채무액: ${outstanding}`);
  }
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

## 어서션 추가하기(10.6)
제곱근 계산은 입력이 양수일 때만 정상 작동한다. 이처럼 특정 조건이 참일 때만 제대로 동작하는 코드에 assertion을 넣어두자. 

assertion은 항상 참이라고 가정하는 조건부 문장이다. 어서션 실패는 시스템의 다른 부분에서는 절대 검사하지 않아야 하며, **어서션이 었고 없고가 프로그램 기능의 정상 동작에 아무런 영향을 주지 않도록 작성되어야 한다.**
### before
```javascript
if (this.discountRate) base = base - (this.discountRate * base);
```
### after
```javascript
assert(this.discountRate >= 0);
if (this.discountRate) base = base - (this.discountRate * base);
```
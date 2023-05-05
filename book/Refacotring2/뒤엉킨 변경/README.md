# 뒤엉킨 변경
하나의 모듈이 서로 다른 이유들로 인해 여러 가지 방식으로 변경되는 일이 많을 때 발생한다. 코드를 수정할 때는 시스템에서 고쳐야 할 딱 한 군데를 찾아서 그 부분만 수정할 수 있기를 바란다. **뒤엉킨 변경은 단일 책임 원칙이 지켜지지 않았을 때 나타난다.**

</br>

## 단계 쪼개기(6.11)

서로 다른 두 대상을 한꺼번에 다루는 코드를 발견하면 각각을 별개의 모듈로 나누는 방법을 모색하자. 코드를 수정할 때 두 대상을 동시에 생각할 필요 없이 하나에만 집중하기 위해서다. 다른 단계로 볼 수 있는 코드 영역들이 서로 다른 데이터와 함수를 사용한다면 단계 쪼개기에 적합하다는 뜻이다.

### before

```js
const orderData = orderString.split(\s+/);
const productPrice = priceList[orderData[0].split("-")[1]];
const orderPrice = parseInt(orderData[1]) * productPrice;
```

### after

```javascript
const orderRecord = parseOrder(order);
const orderPrice = price(orderRecord, priceList); 

function parseOrder(aString) {
  const values = aString.split(\s+/);
  return ({
    productID: values[0].split("-")[1],
    quantity: parseInt(values[1]),
  });
}

function price(order, priceList) {
  return order.quantity * priceList[order.productId];
}
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
// 정보를 출력하는 부분을 함수로 추출했다.
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


## 클래스 추출하기(7.5)

메서드와 데이터가 너무 많은 클래스는 이해하기가 쉽지 않아 적절히 분리하는 것이 좋다. 일부 데이터와 메서드를 따로 묶을 수 있다면 분리하는 것이 좋고, 함께 변경되는 일이 많거나 서로 의존하는 데이터들도 분리하는 것이 좋다.

추가로 특정 데이터나 메서드 일부를 제거했을때 다른 필드나 메서드들이 논리적으로 문제가 없다면 분리할 수 있다고 볼 수도 있다.

### before

```javascript
class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get telephoneNumber() { return `(${this.officeAreaCode}) ${this.officeNumber}` };
  get officeAreaCode() { return this._officeAreaCode; }
  set officeAreaCode(arg) { this._officeAreaCode = arg; }
  get officeNumber() { return this._officeNumber; }
  set officeNumber(arg) { this._officeNumber = arg; }
}
```

### after

```javascript
class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  set areaCode(arg) { this._areaCode = arg; }
  get number() { return this._number; }
  set number(arg) { this._number = arg; }
}

class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  set officeAreaCode(arg) { this._telephoneNumber.areaCode = arg; }
  get officeNumber() { return this._telephoneNumber.number; }
  set officeNumber(arg) { this._telephoneNumber.number = arg; }
}
```

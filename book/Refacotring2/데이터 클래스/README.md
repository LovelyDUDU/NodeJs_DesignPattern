# 데이터 클래스

데이터 클래스란 데이터 필드와 게터/세터 메서드로만 구성된 클래스를 말한다.

## 레코드 캡슐화하기(7.1)

가변 데이터를 저장하는 용도로는 레코드보다 객체가 낫다. 객체를 사용하면 어떻게 저장했는지를 숨긴 채 세 가지 값을 각각의 메서드로 제공할 수 있다. 중첩된 리스트나 해시맵을 받아서 JSON이나 XML같은 포맷으로 직렬화할 때가 많은데 이런 구조 역시 캡슐화하면 나중에 포맷을 바꾸거나 추적하기 어려운 데이터를 수정하기가 수월해진다.

### before

```javascript
const organization = { name: "애크미 구스베리", country: "GB" }
```

### after

```javascript
class Organization {
  constructor(data) {
    this._name = data.name;
    this._country = data.country;
  }
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get country() { return this._country }
  set country(arg) { this._country = arg; }
}
```

</br>

## 세터 제거하기(11.7)

세터를 제거해야하는 상황은 주로 두가지다. 하나는 사람들이 무조건 접근자 메서드를 통해서만 필드를 다루려 할 때이다. 이런 경우 오직 생성자에서만 호출하는 세터가 생겨나기 때문에 세터를 제거하여 객체가 생성된 후에는 값이 바뀌지 못하게 하는 것이 좋다.

다른 하나는 클라이언트에서 생성 스크립트를 사용해 객체를 생성할 때다. 생성 스크립트는 생성자를 호출한 후 일련의 세터를 호출하여 객체를 완성하는 형태의 코드를 말한다. 해당 세터들은 처음 생성할 때만 호출되리라 가정하지만, 정확하게 하기 위해서 세터를 제거하는 것이 좋다.

### before

```javascript
class Person {
  get name() { ... }
  set name(aString) { ... }
}
```

### after

```javascript
class Person {
  get name() { ... }
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

## 단계 쪼개기(6.11)

서로 다른 두 대상을 한꺼번에 다루는 코드를 발견하면 각각을 별개의 모듈로 나누는 방법을 모색하자. 코드를 수정할 때 두 대상을 동시에 생각할 필요 없이 하나에만 집중하기 위해서다. 다른 단계로 볼 수 있는 코드 영역들이 서로 다른 데이터와 함수를 사용한다면 단계 쪼개기에 적합하다는 뜻이다.

### before

```javascript
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

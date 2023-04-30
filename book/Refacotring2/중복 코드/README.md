# 중복 코드

똑같은 코드 구조가 여러 곳에서 반복된다면 하나로 통합하여 더 나은 프로그램을 만들 수 있다.

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

## 문장 슬라이드하기(8.6)

관련된 코드들이 가까이 모여있다면 이해하기가 더 쉽다. 하나의 데이터 구조를 이용하는 문장들은 한데 모여 있어야 좋다.

### before

```javascript
const pricingPlan = retrievePricingPlan();
const order = retreiveOrder();
const baseCharge = pricingPlan.base;
let charge;
const chargePerUnit = pricingPlan.unit;
const units = order.units;
let discount;
charge = baseCharge + units * chargePerUnit;
let discountableUnits = Math.max(units - pricingPlan.discountThreshold, 0);
discount = discountableUnits * pricingPlan.discountFactor;
if (order.isRepeat) discount += 20;
charge = charge - discount;
chargeOrder(charge);
```

### after

```javascript
// 관련 있는 코드끼리 모아줬다.
const pricingPlan = retrievePricingPlan();
const baseCharge = pricingPlan.base;
const chargePerUnit = pricingPlan.unit;
const order = retreiveOrder();
const units = order.units;

let discount;
discount = discountableUnits * pricingPlan.discountFactor;
if (order.isRepeat) discount += 20;

let discountableUnits = Math.max(units - pricingPlan.discountThreshold, 0);

let charge;
charge = baseCharge + units * chargePerUnit;
charge = charge - discount;
chargeOrder(charge);
```

</br>

## 메서드 올리기(12.1)

중복 코드 제거는 중요하다. 중복된 두 메서드가 당장은 문제없이 동작할 수도 있다. 하지만 무언가 중복되었다는 것은 한쪽의 변경이 다른 쪽에는 반영되지 않을 수 있다는 위험을 항상 수반한다.

### before

```javascript
class Employee { ... }

class SalesPerson extends Employee {
  get name() { ... }
}

class Engineer extends Employee {
  get name() { ... }
}
```

### after

```javascript
class Employee {
  get name() { ... }
}

class SalesPerson extends Employee {
  // get name() { ... }
}

class Engineer extends Employee {
  // get name() { ... }
}

```

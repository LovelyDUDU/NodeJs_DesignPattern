# 가변 데이터

## 변수 캡슐화하기(6.6)

함수는 데이터보다 다루기가 수월하다. 변수로의 접근(get)과 갱신(set)을 전담하는 캡슐화 함수들을 만들어서 관리하면 유효범위가 변수일때 보다 줄어들어 관리가 편해진다. 다만 단순히 변수를 캡슐화하기만 했을떄, 변수에 담긴 내용을 변경하는 행위를 제어할 수는 없다.

### before

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
spaceship.owner = defaultOwner;
```

### after

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
spaceship.owner = getDefaultOwner();
export function getDefaultOwner() { return defaultOwner; }
export function setDefaultOwner(arg) { defaultOwner = arg; }
```

변수를 캡슐화하고 변수에 담긴 내용을 변경하는 행위까지 제어하는 방법은 두가지 있다. 하나는 get할때 데이터의 복제본을 반환하도록 하는 것이다. 이렇게 하면 복제본을 get해서 오고, 복제본에 대해서 수정은 가능하지만 원본은 수정하지 못한다.

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
export function getDefaultOwner() { return Object.assgin({}, defaultOwner); }
export function setDefaultOwner(arg) { defaultOwner = arg; }
```

그리고 두번째 방법은 레코드를 캡슐화하는 것이다.

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
export function getDefaultOwner() { return new Person(defaultOwner); }
export function setDefaultOwner(arg) { defaultOwner = arg; }

class Person {
  constructor(data) {
    this._lastName = data.lastName;
    this._firstName = data.firstName;
  }

  get lastName() { return this._lastName }
  get firstName() { return this._firstName }
}
```

</br>

## 변수 쪼개기(9.1)

변수는 다양한 용도로 쓰인다. 그중 for문에서 사용되는 i 와 같이 변수에 값을 여러번 대입할 수 밖에 없는 경우도 있다. 이와 같은 수집 변수는 메서드가 동작하는 중간중간 값을 저장한다.

이러한 경우외에 변수가 두가지 이상의 역할을 할 경우 쪼개야 한다. 역할 하나당 변수 하나다. 여러 용도로 쓰인 변수는 코드를 읽는 이에게 혼란을 줄 수 있다.

### before

```js
let temp = 2 * (height + width);
console.log(temp);
temp = height * width;
console.log(temp);
```

### after

```js
// 변수가 하나의 역할만 하게 바꿔줬다. 
const perimeter = 2 * (height + width);
console.log(perimeter);
const area = height * width;
console.log(area);
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

## 질의 함수와 변경 함수 분리하기(11.1)

겉보기 부수효과 (observable side effect)가 전혀 없는 값을 함수를 추구해야 한다. 이런 함수는 어느 때건 원하는 만큼 호출해도 아무 문제가 없다.

겉보기 부수효과가 있는 함수와 없는 함수는 구분하는 것이 좋다. 이를 위한 한가지 방법은 '질의 함수(읽기 함수)는 모두 부수효과가 없어야 한다' 는 규칙을 따르는 것이다. 이를 '명령-질의 분리(command-query separation)' 이라고 한다.

값을 반환하면서 부수효과가 있는 함수를 발견하면 '상태를 변경하는 부분'과 '질의하는 부분'을 분리하자.

### before

```js
function getTotalOutstandingAndSendBill() {
  const result = cutomer.invoiced.reduce((total, each) => each.amount + total, 0);
  sendBill();
  return result;
}
```

### after

```js
// 값을 반환하면서 부수효과가 있는 함수를 역할에 맞게 나누었다.
function totalOutstanding() {
  return cutomer.invoiced.reduce((total, each) => each.amount + total, 0);
}
function sendBill() {
  emailGateway.send(formatBill(customer));
}
```

</br>

## 세터 제거하기(11.7)

세터를 제거해야하는 상황은 주로 두가지다. 하나는 사람들이 무조건 접근자 메서드를 통해서만 필드를 다루려 할 때이다. 이런 경우 오직 생성자에서만 호출하는 세터가 생겨나기 떄문에 세터를 제거하여 객체가 생성된 후에는 값이 바뀌지 못하게 하는 것이 좋다.

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

## 파생 변수를 질의 함수로 바꾸기(9.3)

가변 데이터는 소프트웨어에 문제를 일으키는 가장 큰 골칫거리에 속한다. 가변 데이터는 서로 다른 두 코드를 이상한 방식으로 결합하기도 하는데, 예를들어 한 쪽 코드에서 수정한 값이 연쇄 효과를 일으켜 다른 쪽 코드에 원인을 찾기 어려운 문제를 야기하기도 한다. 이러한 가변 데이터를 아예 안쓸 수는 없으니 유효 범위를 가능한한 좁히는게 좋다.

### before

```js
get discountTotal() { return this._discountedTotal; }
set discount(aNumber) {
  const old = this._discount;
  this._discount = aNumber;
  this._discountedTotal += old - aNumber;
}
```

### after

```js
get discountTotal() { return this._baseTotal - this._discount; }
set discount(aNumber) {
  this._discount = aNumber;
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

## 참조를 값으로 바꾸기(9.4)

객체(데이터 구조)를 다른 객체(데이터 구조)에 중첩하면 내부 객체를 참조 혹은 값으로 취급할 수 있다. 참조냐 값이냐의 차이는 내부 객체의 속성을 갱신하는 방식에서 가장 극명하게 드러난다.

- 참조로 다루는 경우에는 내부 객체는 그대로 둔 채 그 객체의 속성만 갱신한다.
- 값으로 다루는 경우에는 새로운 속성을 담은 객체로 기존 내부 객체를 통째로 갱신한다.

필드를 값으로 다룬다면 내부 객체의 클래스를 수정하여 값 객체(Value Object)로 만들 수 있다. 값 객체는 불변이기 때문에 대체로 자유롭게 활용하기 좋다. 불변 데이터 값은 바뀌지 않기 때문에 값이 몰래 바뀌어 내부에 영향을 줄까 염려하지 않다도 된다는 장점이 있다.

대신 이 리팩토링을 사용하면 안되는 경우도 있다. 예를 들어, 특정 객체를 여러 객체에서 공유하고자 한다면, 그래서 공유 객체의 값을 변경했을 때 이를 관련 객체 모두에 알려줘야 한다면 공유 객체를 참조로 다뤄야 한다.

### before

```js
class Product {
  applyDiscount(arg) { this._price.amount -= arg; }
}
```

### after

```js
class Product {
  applyDiscount(arg) {
    this._price = new Money(this._price.amount - arg, this._price.currency);
  }
}
```

# 기본형 집착
최소한 사용자에게 보여줄 때는 일관된 형식으로 출력해주는 기능이라도 갖추어야 한다.

</br>

## 기본형을 객체로 바꾸기(7.3)
개발 초기에는 단순한 정보를 숫자나 문자열 같은 간단한 데이터 항목으로 표현할 때가 많다. 그러다 개발이 진행되면서 간단했던 이 정보들이 더 이상 간단하지 않게 변한다. 

단순한 출력 이상의 기능이 필요해지는 순간 그 데이터를 표현하는 전용 클래스를 정의하자. 시작은 기본형 데이터를 단순히 감싼 것과 큰 차이가 없을 것이라 효과가 미미하겠지만, 특별한 동작이 필요해지고 프로그램이 커질 수록 효과가 커진다.

### before
```js
orders.filter(o => "high" === o.priority || "rush" === o.priority);
```

### after
```js
// priority 속성을 Priority 클래스로 만들어서 바꿔주었다.
orders.filter(o => o.priority.higherThan(new Priority("normal")));
```

</br>

## 타입 코드를 서브클래스로 바꾸기(12.6)

소프트웨어 시스템에서는 비슷한 대상들을 특정 특성에 따라 구분해야 할 때가 자주 있다. 이런 일을 다루는 수단으로는 타입 코드(Type code) 필드가 있다.

서브클래스는 두가지면에서 좋다. 첫째, 조건에 따라 다르게 동작하도록 해주는 다형성을 제공한다. 타입 코드에 따라 동작이 달라져야 하는 함수가 여러 개일 때 특히 유용하다.

두번째는 특정 타입에서만 의미가 있는 값을 사용하는 필드나 메서드가 있을때 유용하다. 이런 상황에 서브클래스를 만들고 필요한 서브클래스만 필드를 갖도록 정리하다.

### before

```javascript
function createEmployee(name, type) {
  return new Employee(name, type);
}
```

### after

```javascript
function createEmployee(name, type) {
  switch (type) {
    case "engineer": return new Engineer(name);
    case "salesperson": return new Salesperson(name);
    case "manager": return new Manager(name);
  }
}
```

</br>

## 조건부 로직을 다형성으로 바꾸기(10.4)

복잡한 조건부 로직은 프로그래밍에서 해석하기 가장 난해한 대상에 속한다. 조건부 구조는 클래스와 다형성을 이용하여 더 확실하고 직관적인 분리가 가능하다.

### before

```javascript
switch (bird.type) {
  case "유럽 제비":
    return "보통이다.";
  case "아프리카 제비":
    return (bird.numberOfCoconuts > 2) ? "지쳤다" : "보통이다.";
  case "노르웨이 파랑 앵무":
    return (bird.voltage > 100) ? "그을렸다" : "예쁘다.";
  default:
    return "알 수 없다.";
}
```

### after

```javascript
class EuropeanSwallow {
  get plumage() {
    return "보통이다."; 
  }

  // ...
}
class AfricanSwallow {
  get plumage() {
    return (bird.numberOfCoconuts > 2) ? "지쳤다" : "보통이다.";
  }
  
  // ...
}

class NorwegianBlueParrot {
  get plumage() {
    return (bird.voltage > 100) ? "그을렸다" : "예쁘다.";
  }

  // ...
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

</br>

## 매개변수 객체 만들기(6.8)

데이터 항목 여러개가 몰려다니는 경우 데이터 구조 하나로 묶으면 데이터 사이의 관계가 명확해지고 매개변수 수가 줄어든다는 장점이 있다.

데이터 구조는 클래스로 만들었을 때 나중에 동작까지 묶을 수 있다.

### before

```javascript
const station = { name: "ZB1", reading: [
  {temp: 47, time: "2023-11-11 09:10"},
  {temp: 48, time: "2023-11-11 09:11"},
  {temp: 49, time: "2023-11-11 09:12"},
]};

function readingsOutsideRange(station, min, max) {
  return station.readings.filter(r => r.temp < min || r.temp > max);
}

const alerts = readingsOutsideRange(station, operationPlan.temperatureFloor, operationPlan.temperatureCeiling);
```

### after

```javascript
// station 이라는 매개변수와 그와 관련된 기능을 하나의 객체로 만들었다.
class NumberRange {
  constructor(min, max) {
    this._data = {min: min, max: max};
  }
  get min() { return this._data.min; }
  get max() { return this._data.max; }
}

function readingsOutsideRange(station, range) {
  return station.readings.filter(r => r.temp < range.min || r.temp > range.max);
}

const range = new NumberRange(operationPlan.temperatureFloor, operationPlan.temperatureCeiling);
const alerts = readingsOutsideRange(station, range);
```

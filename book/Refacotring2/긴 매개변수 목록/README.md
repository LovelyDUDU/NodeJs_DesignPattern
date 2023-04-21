# 긴 매개변수 목록

</br>

## 매개변수를 질의 함수로 바꾸기(11.5)
매개변수 목록은 함수의 변동 요인을 모아놓은 곳이다. 중복은 피하는게 좋으면 짧을수록 이해하기 좋다.

리팩토링을 했음에도 불구하고 새로운 의존성이 생기거나 제거하고 싶은 기존 의존성을 강화하는 경우가 생기지 않게 해야한다. 
### before
```javascript
availableVacation(anEmployee, anEmployee.grade);

function availableVacation(anEmployee, grade) {
  // ...
}
```

### after
```javascript
availableVacation(anEmployee);

function availableVacation(anEmployee) {
  const grade = anEmployee.grade;
  // ...
}
```

</br>

## 객체 통째로 넘기기(11.4)
하나의 레코드에서 값 두어 개를 가져와 인수로 넘기는 부분을 값들 대신 레코드를 통째로 넘기고 함수 본문에서 필요한 값들을 꺼내 쓰도록 수정하면 변화에 대응하기 쉽다.

함수가 더 다양한 데이터를 사용하도록 바뀌어도 매개변수 목록은 수정할 필요가 없다. (통쨰로 넘겼기 때문에)

### before
```javascript
const low = aRoom.daysTempRange.low;
const high = aRoom.daysTempRange.high;
if (aPlan.withinRange(low, high))
```

### after
```javascript
if (aPlan.withinRange(aRoom.daysTempRange))
```

</br>

## 매개변수 객체 만들기(6.8)
데이터 항목 여러개가 몰려다니는 경우 데이터 구조 하나로 묶으면 데이터 사이의 관계가 명확해지고 매개변수 수가 줄어든다는 장점이 있다.

데이터 구조는 클래스로 만들었을 떄 나중에 동작까지 묶을 수 있다. 

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

</br>

## 플래그 인수 제거하기(11.3)
플래그 인수(flag argument)란 호출되는 함수가 실행할 로직을 호출하는 쪽에서 선택하기 위해 전달하는 인수다. 

플래그 인수를 사용하면 호출할 수 있는 함수들이 무엇이고 어떻게 호출해야 하는지를 이해하기가 어려워진다.

플래그 인수가 있으면 함수들의 기능 차이가 잘 드러나지 않고, 사용할 함수를 선택한 후에도 플래그 인수로 어떤 값을 넘겨야하는지를 또 알아내야 하는 번거로움이 생긴다.
### before
```javascript
// 함수가 이렇게 되어있으면 name 에 뭘 넣어야 하고, 어떤 값을 넣었을때 어떤 결과가 나오는지 명확히 알 수가 없다.
function setDimenstion(name, value) {
  if (name === "height") {
    this._height = value; 
    return;
  }

  if (name === "width") {
    this._width = value;
    return;
  }
}
```

### after
```javascript
function setHeight(value) { this._height = value; }
function setWidth(value) { this._width = value; }
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

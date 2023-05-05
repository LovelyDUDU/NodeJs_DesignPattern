# 데이터 뭉치

몰려다니는 데이터 뭉치는 보금자리를 따로 마련해줘야 한다.

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
// low, high 정보를 한꺼번에 넘겼다.
if (aPlan.withinRange(aRoom.daysTempRange))
```

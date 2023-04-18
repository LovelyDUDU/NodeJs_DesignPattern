# 긴 함수

코드를 이해하고, 공유하고, 선택하기 쉬워진다는 장점은 함수를 짧게 구성할 때 나온다. 함수가 길수록 사람들은 함수를 이해하기 힘들다.


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

## 임시변수를 질의 함수로 바꾸기(7.4)
긴 함수의 한 부분을 별도 함수로 추출하고자 할 떄 먼저 변수들을 각각의 함수로 만들면 추출한 함수에 변수를 따로 전달할 필요가 없어지기 때문에 좋다.

추출한 함수와 원래 함수의 경계가 분명해진다는 장점도 있다.

변수 대신 함수로 만들어두면 비슷한 계산을 수행하는 다른 함수에서도 사용할 수 있다.

### before
```javascript
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000) return basePrice * 0.95;
else return basePrice * 0.98
```

### after
```javascript

get basePrice() {
  this._quantity * this._itemPrice;
} 
if (this.basePrice > 1000) return this.basePrice * 0.95;
else return this.basePrice * 0.98
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

## 함수를 명령으로 바꾸기(11.9)
함수를 그 함수만을 위한 객체 안으로 캡슐화하면 더 유용해지는 상황이 있다. 이런 객체를 가리켜 '명령 객체' 혹은 명령(command)라고 한다.

명령 객체 대부분은 메서드 하나로 구성되며, 이 메서드를 요청해 실행하는 것이 이 객체의 목적이다.

### before
```javascript
function score(candidate, medicalExam, scoringGuide) {
  let result = 0;
  let healthLevel = 0;
  let highMedicalRiskFlag = false;

  if (medicalExam.isSmoker) {
    healthLevel += 10;
    highMedicalRiskFlag = true;
  }

  let certificationGrade = 'regular';
  if (scoringGuide.stateWithLowCertification(candidate.originState)) {
    certificationGrade = 'low';
    result -= 5;
  }

  // lots more code like this
  result -= Math.max(healthLevel - 5, 0);
  return result;
}
```

### after
```javascript
function score(candidate, medicalExam, scoringGuide) {
  return new Scorer(candidate, medicalExam, scoringGuide).execute();
}

class Scorer {
  constructor(candidate, medicalExam, scoringGuide) {
    this._candidate = candidate;
    this._medicalExam = medicalExam;
    this._scoringGuide = scoringGuide;
  }
  execute() {
    this._result = 0;
    this._healthLevel = 0;
    this._highMedicalRiskFlag = false;

    this.scoreSmoking();
    this._certificationGrade = "regular";

    if (this._scoringGuide.stateWithLowCertification(this._candidate.originState)) {
      this._certificationGrade = 'low';
      this._result -= 5;
    }

    // lots more code like this
    this._result -= Math.max(this._healthLevel - 5, 0);
    return this._result;
  }

  scoreSmoking() {
    if (this._medicalExam.isSmoker) {
      this._healthLevel += 10;
      this._highMedicalRiskFlag = true;
    }
  }
}

```

</br>

## 조건문 분해하기(10.1)
긴 함수는 그 자체로 읽기가 어렵지만, 조건문은 그 어려움은 한층 가중시킨다.

거대한 코드 블럭이 주어지면 코드를 부위별로 분해한 다음 해체된 코드 덩어리들을 각 덩어리의 의도를 살린 이름의 함수 호출로 바꿔주자.

### before
```javascript
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)) {
  charge = quality * plan.summerRate;
} else {
  charge = quality * plan.summerRate + plan.regularServiceCharge;
}
```

### after
```javascript
if (summer()) charge = summerCharge();
else charge = regularCharge();

function summer() {
  return !aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd);
}

function summerCharge() {
  return quality * plan.summerRate;
}

function regularCharge() {
  return quality * plan.summerRate + plan.regularServiceCharge;
}
```


</br>

## 조건부 로직을 다형성으로 바꾸기(10.4)



</br>

## 반복문 쪼개기(8.7)

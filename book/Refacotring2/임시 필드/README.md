# 임시 필드
임시 필드를 갖도록 작성하면 코드를 이해하기 어렵다.

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



## 특이 케이스 추가하기(10.5)

특이 케이스를 확인하는 코드 대부분을 단순한 함수 호출로 바꾸자. 

특수한 경우의 공통 동작을 요소 하나에 모아서 사용하는 "특이 케이스 패턴" 이라는 것이 있는데 이럴때 해당 리팩토링을 적용하면 좋다.

- 단순히 데이터를 읽기만 하는 경우: 변환할 값들을 담은 리터럴 객체 형태로 준비
- 어떤 동작을 수행해야 하는 경우: 필요한 메서드를 담은 객체를 생성

### before
```js
if (aCustomer === "미확인 고객") customerName = "거주자";
```

### after
```js
class UnknownCustomer {
  get name() { return "거주자" }
}
```

### 기타
추후 내용 보완 및 이해가 필요한 리팩토링 기법이다. 읽어봤을때는 특이 케이스에 대한 처리를 객체로 처리하라는 것 같은데 명확히 이해가 되지 않는다ㅠ 어차피 한번 정리하고 떙칠 내용의 책은 아니니깐 조금씩 내용을 보완하고 이해해보자.
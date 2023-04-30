# 거대한 클래스

한 클래스에서 너무 많은 일을 하려다 보면 필드 수가 상당히 늘어난다. 클래스에 필드가 너무 많으면 중복 코드가 생기기 쉽다. 클래스 안에서 자체적으로 중복을 제거하자.

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

## 슈퍼클래스 추출하기(12.8)

비슷한 일을 하는 두 클래스가 보이면 상속 메커니즘(extends)를 이용해서 비슷한 부분을 공통의 슈퍼클래스로 옮겨 담을 수 있다.

### before

```javascript
class Department {
  get totalAnnualCost() {}
  get name() {}
  get headCount() {}
}

class Employee {
  get annualCost() {}
  get name() {}
  get id() {}
}
```

### after

```javascript
class Party {
  get name() {}
  get annualCost() {}
}

class Department extends Party{
  get headCount() {}
  // get totalAnnualCost() {} 
  // get name() {} 
}

class Employee extends Party{
  get id() {}
  // get annualCost() {} 
  // get name() {} 
}
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

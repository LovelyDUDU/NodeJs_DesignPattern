# 메시지 체인
메시지 체인은 클라이언트가 한 객체를 통해 다른 객체를 얻은 뒤 방금 얻은 객체에 또 다른 객체를 요청하는 식으로, 다른 객체를 요청하는 작업이 연쇄적으로 이어지는 코드를 말한다. 이는 클라이언트가 객체 내비게이션 구조에 종속됐음을 의미한다. 그래서 내비게이션 중단 단계를 수정하면 클라이언트 코드도 수정되어야 한다.

메세지 체인의 전형적인 예시는 다음과 같다.
```js
managerName = aPerson.department.manager.name; 
```

'체인을 구성하는 모든 객체에 위임 숨기기를 적용할 수 있다'는 다음과 같다.
- 부서장 이름(managerName)을 바로 반환하는 메서드를 사람 클래스에 추가할 수도 있다.
  - `managerName = aPerson.managerName` -> 부서 객체와 관리자 객체 모두의 존재를 숨김
- 부서장 이름(managerName)을 바로 반환하는 메서드를 부서 클래스에 추가할 수도 있다
  - `managerName = aPerson.department.managerName` -> 관리자 객체(manager)의 존재를 숨김
- 부서장(manager)을 얻는 메서드를 사람 클래스에 추가할 수도 있다.
  - `managerName = aPerson.manager.name` -> 부서 객체(department)의 존재를 숨김

</br>

## 위임 숨기기(7.7)

모듈화 설계를 제대로 하는 핵심은 캡슐화이다. 캡슐화는 모듈들이 시스템의 다른 부분에 대해 알아야 할 내용을 줄여준다. 캡슐화가 잘 되어 있다면 무언가를 변경해야할 때 함께 고려해야할 모듈 수가 적어져서 코드를 변경하기가 훨씬 쉬워진다. 물론 캡슐화의 기능이 단순히 필드를 숨기는 것만 있는 것은 아니다.

예를들어 서버 객체의 필드가 가리키는 객체(위임 객체)의 메서드를 호출하려면 클라이언트는 이 위임객체를 알아야 한다. 위임 객체의 인터페이스가 바뀌면 이 인터페이스를 사용하는 모든 클라이언트가 코드를 수정해야한다. 하지만 위임을 숨겨 이러한 의존성을 없애면 위임 객체가 수정되더라도 서버 코드만 수정하면 된다.

### before

```js
// 위임객체가 수정되면 클라이언트에서 해당 객체를 호출하는 부분을 모두 바꿔야 한다.
manager = aPerson.department.manger;
```

### after

```js
// 위임을 숨기게 되면 클라이언트에서 해당 객체를 호출하는 부분은 그대로 두고, 서버 코드만 수정하면 된다.
manager = aPerson.manager;

class Person {
  get manager { return this.department.manager; }
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

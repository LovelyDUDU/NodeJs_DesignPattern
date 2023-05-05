# 기능 편애
프로그램을 모듈화할 때는 코드를 여러 영역으로 나눈 뒤 영역 안에서 이뤄지는 상호작용은 최대한 늘리고 영역 사이에서 이뤄지는 상호작용은 최소로 줄이는 데 주력한다. 기능 편애는 어떤 함수가 자기가 속한 모듈의 함수나 데이터보다 다른 모듈의 함수나 데이터와 상호 작용할 일이 더 많을 때 나타난다. 적절히 모듈 이동을 시키면 된다.

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

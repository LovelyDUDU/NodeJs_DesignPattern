# 전략 패턴 (Strategy Pattern)

전략 패턴은 객체가 특정 동작 또는 알고리즘을 실행할 때 유연성을 제공하는 디자인 패턴입니다. 이 패턴은 동적으로 알고리즘을 교체하거나 다양한 알고리즘을 선택할 수 있는 구조를 제공합니다. 전략 패턴은 알고리즘의 구현을 캡슐화하고, 알고리즘을 사용하는 객체와 독립적으로 변경할 수 있도록 합니다.



</br>

## 특징
<details>
<summary>1. 유연한 알고리즘 교체</summary>

전략 패턴은 알고리즘을 독립적으로 정의하고, 클라이언트에서 알고리즘을 교체할 수 있도록 합니다. 이는 실행 중에 다른 전략으로 쉽게 전환할 수 있으며, 알고리즘의 변경이 클라이언트 코드에 영향을 주지 않습니다.
</details>

<details>
<summary>2. 객체 간 결합도 감소</summary>

전략 패턴은 알고리즘을 전략 객체로 캡슐화하여 클라이언트와 알고리즘 간의 결합도를 낮춥니다. 클라이언트는 추상화된 전략 인터페이스를 통해 전략에 접근하므로, 실제 구현에 대한 의존성을 줄일 수 있습니다.
</details>

<details>
<summary>3. 확장성과 유지보수성 개선</summary>

전략 패턴은 알고리즘을 독립적으로 관리하므로, 새로운 전략을 추가하거나 기존 전략을 변경하는 작업이 간편해집니다. 새로운 전략은 새로운 전략 클래스를 구현하여 추가할 수 있으며, 기존 코드에 영향을 주지 않고 전략을 확장할 수 있습니다.
</details>

<details>
<summary>4. 코드 중복 최소화</summary>

전략 패턴은 공통된 기능을 추상화하여 전략 객체에 위임하므로, 클라이언트 코드에서 반복적인 코드를 최소화할 수 있습니다. 공통된 동작은 전략 객체에서 한 번만 구현하면 되므로 코드 중복을 효과적으로 제거할 수 있습니다.
</details>

<details>
<summary>5. 테스트 용이성</summary>

전략 패턴은 각각의 전략을 독립적으로 테스트할 수 있습니다. 각 전략은 인터페이스를 따르므로 개별적으로 테스트 가능하며, 전략을 교체하는 경우에도 테스트 케이스의 수정이 필요하지 않습니다.
</details>


</br>

## 구현 방법
주문 시스템을 예로 들어보겠습니다. 

주문 객체가 전략을 선택하고, 해당 전략을 사용하여 결제를 처리합니다. 

주문 시스템은 컨텍스트 클래스를 통해 결제 방법을 유연하게 변경할 수 있으며, 각 결제 방법에 대한 실제 처리는 전략 클래스에서 수행됩니다. 

이를 통해 주문 시스템은 결제 방법에 대한 변경에 영향을 받지 않으면서도 다양한 결제 전략을 지원할 수 있습니다.

```ts
// 전략 인터페이스 정의
interface PaymentStrategy {
  processPayment(amount: number): void;
  // 추상화된 알고리즘
}



// 전략 구현 클래스 생성
class CreditCardStrategy implements PaymentStrategy {
  processPayment(amount: number): void {
    console.log(`Credit card payment processed. Amount: ${amount}`);
    // 실제 결제 처리 로직 구현
  }
}

class PayPalStrategy implements PaymentStrategy {
  processPayment(amount: number): void {
    console.log(`PayPal payment processed. Amount: ${amount}`);
    // 실제 결제 처리 로직 구현
  }
}



// 컨텍스트 클래스 생성
class Order {
  private paymentStrategy: PaymentStrategy;

  // 어떤 전략을 사용할지 설정
  setPaymentStrategy(strategy: PaymentStrategy): void {
    this.paymentStrategy = strategy;
  }

  processOrder(amount: number): void {
    // 주문 처리 로직

    if (this.paymentStrategy) {
      this.paymentStrategy.processPayment(amount);
    } else {
      console.log('Payment strategy is not set.');
    }
  }
}



// 주문 객체 생성 -> 원하는 결제 방법의 전략을 설정
const order = new Order();

// 신용카드 결제 전략 설정
const creditCardStrategy = new CreditCardStrategy();
order.setPaymentStrategy(creditCardStrategy);
order.processOrder(100);

// PayPal 결제 전략 설정
const payPalStrategy = new PayPalStrategy();
order.setPaymentStrategy(payPalStrategy);
order.processOrder(200);
```

</br>

## 사용 예시
전략 패턴은 다음과 같은 상황에 사용될 수 있습니다.
<details>
<summary>여러 알고리즘 또는 동작이 필요한 경우</summary>

여러 알고리즘 또는 동작을 각각의 전략(Strategy)으로 정의하고, 클라이언트는 실행시키려는 전략을 선택하여 사용합니다. 이렇게 하면 클라이언트는 전략을 교체하거나 새로운 전략을 추가하는 것이 간편해집니다.
</details>

<details>
<summary>동적으로 알고리즘을 선택해야 하는 경우</summary>

프로그램이 실행 중에 알고리즘을 동적으로 선택해야 할 때 스트래티지 패턴을 사용할 수 있습니다. 이는 조건에 따라 다른 전략을 선택하여 실행하는 유연성을 제공합니다.
</details>

</br>

## 장단점

### 장점 
- 알고리즘을 쉽게 교체하거나 확장할 수 있으며, 코드의 재사용성과 유지보수성을 높일 수 있습니다. 
- 알고리즘의 변경이 컨텍스트와 독립적으로 이루어지므로 코드의 결합도를 낮출 수 있습니다.


### 단점 
- 복잡성

  전략 패턴은 객체 간의 상호작용을 추가합니다. 전략 인터페이스, 전략 클래스, 컨텍스트 클래스 등의 추가적인 클래스와 인터페이스가 필요하므로 코드의 복잡성이 증가할 수 있습니다. 작은 규모의 프로젝트나 단순한 로직에서는 이러한 추가 구조가 불필요할 수 있습니다.

- 과도한 클래스

  전략 패턴을 사용하면 전략에 해당하는 각각의 클래스가 생성됩니다. 따라서 전략의 수가 많아질수록 클래스의 수도 증가하게 됩니다. 이로 인해 클래스의 관리가 복잡해지고, 코드베이스가 커질 수 있습니다.

- 실행 시간 오버헤드

  전략 패턴은 실행 시간에 동적으로 전략을 선택하므로, 선택 과정에 대한 오버헤드가 발생할 수 있습니다. 매번 전략을 선택하는 작업은 성능에 영향을 줄 수 있으며, 전략이 자주 변경되지 않는 경우에는 정적인 방식으로 구현하는 것이 더 효율적일 수 있습니다.

- 의존성 관리

  전략 패턴은 컨텍스트와 전략 클래스 간의 강한 의존성을 가지고 있습니다. 컨텍스트 클래스는 전략 인터페이스를 통해 전략에 의존하며, 전략 클래스도 컨텍스트에 의존합니다. 이로 인해 클래스 간의 결합도가 높아질 수 있으며, 한 클래스의 변경이 다른 클래스에도 영향을 주는 경우가 발생할 수 있습니다.
# 브릿지 패턴 (Bridge Pattern)

객체의 추상화와 구현을 분리하여 두 개의 계층을 독립적으로 확장할 수 있는 구조를 제공하는 소프트웨어 디자인 패턴입니다. "브릿지" 라는 개념을 사용하여 추상화와 구현을 연결하고, 서로를 독립적으로 변경할 수 있도록 합니다.

브릿지 패턴은 추상화와 구현을 분리하여 상호작용을 가능하게 하는 것이 핵심입니다. 추상화와 구현자는 독립적으로 확장될 수 있으며, 런타임 시에도 상호 교환이 가능합니다. 이를 통해 유연성과 확장성이 향상되며, 새로운 기능을 추가하거나 변화에 대응하기 쉬워집니다.

브릿지 패턴은 객체 지향 설계 원칙 중 "의존성 역전 원칙(Dependency Inversion Principle)"와 관련이 있습니다. 이 패턴은 추상화에 의존하는 것을 선호하고, 구체적인 구현에 의존하는 것을 피하도록 도와줍니다. 이를 통해 유연하고 확장 가능한 코드를 작성할 수 있습니다.

</br>

## 특징
<details>
<summary>1. Abstraction (추상화)</summary>
추상화는 클라이언트 코드에서 사용되는 인터페이스를 정의합니다. 이 인터페이스는 구현 세부 정보에 의존하지 않고 추상적인 개념에 초점을 맞춥니다.
</details>

<details>
<summary>2. Implementor (구현자)</summary>
구현자는 추상화에 대한 구현을 제공합니다. 이는 추상화에서 정의한 인터페이스를 구체적으로 구현하는 역할을 합니다.
</details>

<details>
<summary>3. Concrete Implementor(구체적인 구현자)</summary>
구체적인 구현자는 구현자 인터페이스의 실제 구현을 제공합니다. 추상화와 구현자 간의 연결을 구체화합니다.
</details>

</br>

## 사용 예시
- UI 프레임워크에서 UI 요소와 그렇지 않은 부분을 분리하여 각각 독립적으로 확장할 수 있게 합니다.
- 데이터베이스 연결에 사용되는 접근 계층과 실제 데이터베이스 구현을 분리하여 서로를 독립적으로 확장할 수 있게 합니다.
- 플러그인 아키텍처에서 핵심 애플리케이션과 플러그인을 분리하여 독립적으로 확장 가능하게 합니다.

</br>

## 예시 코드

```ts
// Abstraction: 도형을 그리는 추상화 인터페이스
interface Shape {
  draw(): void;
}

// Implementor: 그래픽 플랫폼을 위한 구현자 인터페이스
interface DrawingAPI {
  drawCircle(x: number, y: number, radius: number): void;
  drawLine(x1: number, y1: number, x2: number, y2: number): void;
  // 다른 도형을 그리기 위한 메서드들...
}

// Concrete Implementor: Windows 그래픽 플랫폼을 위한 구현자
class WindowsDrawingAPI implements DrawingAPI {
  drawCircle(x: number, y: number, radius: number): void {
    console.log(`Windows: Drawing a circle at (${x}, ${y}) with radius ${radius}`);
  }

  drawLine(x1: number, y1: number, x2: number, y2: number): void {
    console.log(`Windows: Drawing a line from (${x1}, ${y1}) to (${x2}, ${y2})`);
  }
  // Windows에서 다른 도형을 그리기 위한 메서드들...
}

// Concrete Implementor: macOS 그래픽 플랫폼을 위한 구현자
class MacOSDrawingAPI implements DrawingAPI {
  drawCircle(x: number, y: number, radius: number): void {
    console.log(`macOS: Drawing a circle at (${x}, ${y}) with radius ${radius}`);
  }

  drawLine(x1: number, y1: number, x2: number, y2: number): void {
    console.log(`macOS: Drawing a line from (${x1}, ${y1}) to (${x2}, ${y2})`);
  }
  // macOS에서 다른 도형을 그리기 위한 메서드들...
}

// Refined Abstraction: 선(Line) 도형
class LineShape implements Shape {
  constructor(private x1: number, private y1: number, private x2: number, private y2: number, private drawingAPI: DrawingAPI) {}

  draw(): void {
    this.drawingAPI.drawLine(this.x1, this.y1, this.x2, this.y2);
  }
}

// Refined Abstraction: 원(Circle) 도형
class CircleShape implements Shape {
  constructor(private x: number, private y: number, private radius: number, private drawingAPI: DrawingAPI) {}

  draw(): void {
    this.drawingAPI.drawCircle(this.x, this.y, this.radius);
  }
}

// 예시 코드의 실행
const windowsDrawingAPI = new WindowsDrawingAPI();
const macOSDrawingAPI = new MacOSDrawingAPI();

const lineShape = new LineShape(0, 0, 100, 100, windowsDrawingAPI);
lineShape.draw();

const circleShape = new CircleShape(50, 50, 30, macOSDrawingAPI);
circleShape.draw();
```

- Shape 인터페이스: 추상화 역할을 합니다. 
- LineShape 및 CircleShape 클래스: 추상화를 상속하며, 그리기 작업을 수행할 때 DrawingAPI 인터페이스를 사용합니다. 
- WindowsDrawingAPI 및 MacOSDrawingAPI 클래스: DrawingAPI 인터페이스를 구현하여 그래픽 플랫폼별로 그리기 작업을 수행합니다.
  
해당 예시에서 브릿지 패턴은 도형과 그래픽 플랫폼 간의 결합도를 낮추고, 도형과 그래픽 플랫폼 간의 확장성과 유연성을 제공합니다. 새로운 도형이나 새로운 그래픽 플랫폼이 추가되더라도, 기존 코드의 수정 없이 새로운 도형과 그래픽 플랫폼을 조합하여 사용할 수 있습니다.

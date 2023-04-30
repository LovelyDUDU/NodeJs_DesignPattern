# 전역 데이터

</br>

## 변수 캡슐화하기(6.6)

함수는 데이터보다 다루기가 수월하다. 변수로의 접근(get)과 갱신(set)을 전담하는 캡슐화 함수들을 만들어서 관리하면 유효범위가 변수일때 보다 줄어들어 관리가 편해진다. 다만 단순히 변수를 캡슐화하기만 했을떄, 변수에 담긴 내용을 변경하는 행위를 제어할 수는 없다.

### before

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
spaceship.owner = defaultOwner;
```

### after

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
spaceship.owner = getDefaultOwner();
export function getDefaultOwner() { return defaultOwner; }
export function setDefaultOwner(arg) { defaultOwner = arg; }
```

변수를 캡슐화하고 변수에 담긴 내용을 변경하는 행위까지 제어하는 방법은 두가지 있다. 하나는 get할때 데이터의 복제본을 반환하도록 하는 것이다. 이렇게 하면 복제본을 get해서 오고, 복제본에 대해서 수정은 가능하지만 원본은 수정하지 못한다.

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
export function getDefaultOwner() { return Object.assgin({}, defaultOwner); }
export function setDefaultOwner(arg) { defaultOwner = arg; }
```

그리고 두번째 방법은 레코드를 캡슐화하는 것이다.

```javascript
let defaultOwner = { firstName: "마틴", lastName: "파울러" };
export function getDefaultOwner() { return new Person(defaultOwner); }
export function setDefaultOwner(arg) { defaultOwner = arg; }

class Person {
  constructor(data) {
    this._lastName = data.lastName;
    this._firstName = data.firstName;
  }

  get lastName() { return this._lastName }
  get firstName() { return this._firstName }
}
```

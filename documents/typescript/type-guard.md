# Type Guard

## Type Assertion

```typescript
class Character {
  // ...
}

class Wizard extends Character {
  fireBall() {
    /* ... */
  }
}

class Warrior extends Character {
  attack() {
    /* ... */
  }
}

if (character.isWizard()) {
  character.fireBall(); // Property 'fireBall' does not exist on type 'Character'.
  // ...
} else if (character.isWarrior()) {
  character.attack(); // Property 'attack' does not exist on type 'Character'.
}
```

- 위와 같은 경우, TS는 character의 type을 모르기 때문에 fireBall() 함수가 있는지 알 수 없다. 이 경우 `as` 키워드를 사용해서 TS에 해당 변수의 타입을 알려 줄 수 있다.

```typescript
if (character.isWizard()) {
  (character as Wizard).fireBall(); // Pass
  // ...
} else if (character.isWarrior()) {
  (character as Warrior).attack(); // Pass
}
```

- 타입 단언을 하기 위해서는 아래와 같이 2가지 키워드를 사용 할 수 있다.

```typescript
(<Wizard>character).fireBall();
(character as Wizard).fireBall();
```

- 단, 타입 단언은 많이 사용 할수록 버그의 위험이 높아질 수 있기 때문에 어쩔 수 없는 경우에만 사용 하는 것이 좋다. 아래와 같은 함수의 경우, 문제가 없지만 조건에 `typeof value === 'object'` 와 같은 코드가 추가 된 경우

```typescript
function print(value: number | string) {
  // 조건에 typeof value === "object" 를 추가
  if (typeof value === "number" || typeof value === "object") {
    // value는 number 또는 object가 될 수 있으므로 value as number는 버그를 유발 할 수 있다.
    (value as number).toFixed(2);
  } else {
    (value as string).trim();
  }
}
```

## Type Guard

- 값의 영역에서 사용한 코드를 분석해서 변수의 타입을 확인 할 수 있는 기능

- 함수의 경우

```typescript
function print(value: number | string) {
  if (typeof value === "number") {
    // typeof 를 사용하여 value의 타입을 확인 했기 때문에, TS도 value의 type을 이해할 수 있다. 그래서 value as number 와 같이 따로 명시하지 않아도 된다.
    value.toFixed(2);
    // vaule = number
  } else {
    value.trim();
    // value = string
  }
}
```

- 클래스의 경우 instanceof 키워드를 사용

```typescript
class Person {
  // ...
}

class Product {
  // ...
}

function print(value: Person | Product) {
  if (value instanceof Person) {
    // value = Person
  } else {
    // value = Product
  }
}
```

- 클래스나 생성자 함수가 아닌 interface의 경우, instanceof를 사용 할 수 없음.

```typescript
interface Person {
  // ...
}

interface Product {
  // ...
}

function print(value: Person | Product) {
  // interface는 type을 확인하기 위한 코드이기 때문에 컴파일 후에는 다 제거되므로 instanceof는 사용불가.
  if (value instanceof Person) {
    // ...
  } else {
    //...
  }
}
```

- interface를 구분하기 위한 방법
  - 같은 이름의 속성을 정의하고, 속성의 값이 겹치지 않게 정의해서 확인.

```typescript
// discreiminated union

interface Person {
  type: "a";
  // ...
}

interface Product {
  type: "b";
  // ...
}

function print(value: Person | Product) {
  if (value.type === "a") {
    // 이 경우에도 type guard 가 동작해서 value의 타입은 Person 이라는 것을 알 수 있음.
  } else {
    //value = Product
  }
}
```

- switch 문을 사용하여 가독성을 높일 수 있다.

```typescript
interface Person {
  type: "a";
  // ...
}

interface Product {
  type: "b";
  // ...
}

interface Product2 {
  type: "c";
}

function print(vaule: Person | Product | Product2) {
  switch (value.type) {
    case "a":
      // ...
      break;
    case "b":
      // ...
      break;
    case "c":
      // ...
      break;
  }
}
```

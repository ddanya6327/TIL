# Compatibility

- 어떤 타입을 다른 타입으로 바꿔도 되는지 판단 하는 것.
  - 어떤 변수를 다른 변수로 할당하기 위해서는 해당 변수가 다른 쪽 변수에 할당 가능해야 한다.

```typescript
function func1(a: number, b: number | string) {
  const v1: number | string = a;
  const v2: number = b;
  // v2의 경우 number만 지원하지만, b는 string 일 확률도 있으니 할당 할 수 없다.
}
```

- 타입 스크립트는 structural typing 을 사용하기 때문에, 타입 이름이 다르더라도 내부 구조가 같으면 할당 할 수 있다.

```typescript
interface Person {
  name: string;
  age: number;
}
interface Product {
  name: string;
  age: number;
}
const person: Person = { name: "mike", age: 23 };
const product: Product = person;
// 타입 이름은 다르지만 내부 구조가 같으므로 person을 product 타입에 할당 할 수 있다.
```

인터페이스 A가 인터페이스 B로 할당 가능하기 위한 조건을 정리하면

1. B에 있는 모든 필수 속성의 이름이 A에도 존재 해야 한다.
2. 같은 속성 이름에 대해, A의 속성이 B의 속성에 할당 가능해야 한다.

```typescript
interface Person {
  name: string;
}
interface Product {
  name: string;
  age: number;
}

const obj = { name: "mike", age: 23, city: "abc" };
// 아래의 경우는 할당 가능하다.
// obj에 Person에서 정의한 name이 있기 때문. 그 외의 속성은 어떤 값이 와도 상관없음.
let person: Person = obj;

// 아래의 경우는 할당 불가능.
// obj의 age가 숫자가 아니므로 할당 할 수 없다.
let product: Product = obj;
```

- 작은 집합을 큰 집합에 할당 하는 것은 가능하지만, 더 큰 집합을 작은 집합으로 할당 하는 것은 불가능하다.

```typescript
interface Person {
  name: string;
  age: number;
  gender: string;
}
interface Product {
  name: string;
  age: number | string;
}

const person: Person = {
  name: "mike",
  age: 23,
  gender: "male",
};
// Product 의 집합이 더 크기 때문에 값의 집합이 더 작은 person 을 할당 할 수 있다. 반대로는 불가능.
const product: Product = person;
```

## 함수의 타입 호환성

- 호출 시점에 문제가 없어야 할당 가능

함수 타입 A 가 함수 타입 B로 할당 가능하기 위한 조건

1. A의 매개변수 개수가 B의 매개변수 보다 적어야 한다.
2. 같은 위치의 매개변수에 대해 B의 매개변수가 A의 매개변수로 할당 가능해야 한다.
3. A의 반환값은 B의 반환값으로 할당 가능해야 한다.

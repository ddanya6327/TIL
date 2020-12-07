# Interface

```typescript
interface Person {
  name: string;
  age: number;
}

const p1: Person = { name: "mike", age: 20 };
```

## 선택 속성

```typescript
interface Person {
  name: string;
  age?: number;
  // age가 없어도 상관없음. undefined와 다르다.
}

const p1: Person = { name: "mike", age: 20 };
```

## readonly

```typescript
interface Person {
  readonly name: string;
  age?: number;
  // age가 없어도 상관없음. undefined와 다르다.
}

const p1: Person = { name: "mike" };
p1.name = "error";
// readonly 의 값을 변경하려면 error
```

## index type

- 속성 이름을 구체적으로 정의하지 않고 값의 타입만 정의 하는 것.

```typescript
interface Person {
  readonly name: string;
  age: number;
  [key: string]: string | number;
  // key 부분은 아무거나 입력해도 상관 없음. 속성 이름이 string 인 속성에 대해, 값이 string 또는 number 라고 정의
}

const p1: Person = {
  name: "mike",
  birthday: "1991-01-01", // index type을 사용
  age: "25", // index type이 아닌 index라고 명시적으로 정의한 type을 사용하므로 Number가 적용되어 Error
};
```

```typescript
interface Aa {
  [year: number]: number;
  [year: string]: string | number;
}
const bb: Aa = {};
bb[123] = 100; // OK
bb[123] = "abc"; // Error
bb["123"] = 1234; // OK
bb["123"] = "test"; // OK
```

## function type

- interface를 사용한 함수의 타입 정의

```typescript
interface GetText {
  (name: string, age: number): string;
}

const getText: GetText = function (name, age) {
  // ...
  return "";
};
```

- 함수 내부의 속성 값 정의 할 수 있음

```typescript
interface GetText {
  (name: string, age: number): string;
  total?: number;
}
// ...
```

## class로 구현한 interface

```typescript
interface Person {
  name: string;
  age: number;
  isYounger(age: number): boolean;
}

class SomePerson implements Person {
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  isYounger(age: number) {
    return 123;
  }
}
```

## interface 를 확장한 interface

```typescript
interface Person {
  name: string;
  age: number;
}

interface Programmer {
  language: string;
}

interface Korean extends Person, Programmer {
  isLiveInSeoul: boolean;
}

// & (교차타입) 을 이용한 예
type PP = Person & Programmer;
const pp: PP = {
  // ...
};
```

## 익명 인터페이스(anonymous interface)

```typescript
let ai: {
  name: string;
  age: number;
  etc?: boolean;
} = { name: "Jack", age: 32 };
```

- 주로 함수를 구현할 때 사용

```typescript
function printMe(me: { name: string; age: number; etc?: boolean }) {
  // ...
}
```

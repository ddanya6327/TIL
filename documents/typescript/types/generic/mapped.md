# Mapped

- 대표적으로 interface가 있을 때, interface의 모든 속성을 optional이나 readonly로 바꾸는 일을 할 수 있음.

```typescript
// mapped type 으로 만들어 지는 것은 객체이므로 {}를 씀.
// [] 는 key 부분을 나타냄
// in 키워드를 사용하며 뒤의 내용들이 객체의 속성으로 만들어짐.
// k는 아무 이름이나 써도 됨.
type T1 = { [K in "prop1" | "prop2"]: boolean };
// type T1 = {
//  prop1: boolean;
//  prop2: boolean;
//}
```

```typescript
interface Person {
  name: string;
  age: number;
}

type MakeBoolean<T> = { [P in keyof T]?: boolean };
const pMap: MakeBoolean<Person> = {};

// pMap = {
//  name?: boolean;
//  age?: boolean;
//}
```

```typescript
interface Person {
  name: string;
  age: number;
}

type T1 = Person["name"];

// Readonly 와 Partial 은 TS에 내장되어 있기 때문에 따로 정의하지 않아도 된다.
type Readonly<T> = { readonly [P in keyof T]: T[P] };
type Partial<T> = { [P in keyof T]?: T[P] };
type T2 = Partial<Person>;
// T2 = {
//  name?: string,
//  age?: number,
//}
type T3 = Readonly<Person>;
// T3 = {
//  readonly name: string,
//  readonly age: number,
//}
```

Pick

```typescript
// pick 은 내장 type 이므로 따로 정의하지 않아도 사용 가능.
type Pick<T, K extends keyof T> = { [P in K]: T[P] };
interface Person {
  name: string;
  age: number;
  language: string;
}
type T1 = Pick<Person, "name" | "language">;
```

Record

```typescript
interface Person {
  name: string;
  age: number;
  language: string;
}
// Record는 TS의 내장 Type이므로 정의하지 않아도 사용 가능.
type Record<K extends string, T> = { [P in K]: T };
type T1 = Record<"p1" | "p2", Person>;
// T1 = {
//   p1: Person;
//   p2: Person;
// }
type T2 = Record<"p1" | "p2", number>;
// T2 = {
//   p1: number;
//   p2: number;
// }
```

## Enum 에 활용

아래와 같이 Enum을 정의해서 과일의 값을 매기는 경우

```typescript
enum Fruit {
  Apple,
  Banana,
  Orange,
}

const FRUIT_PRICE = {
  [Fruit.Apple]: 1000,
  [Fruit.Banana]: 1500,
  [Fruit.Orange]: 2000,
};
```

새로운 과일이 추가 되었을 때, enum에는 추가하지만 FRUIT_PRICE에는 추가 해주지 않는 경우가 종종 있음.
그럴 경우, mapped type을 이용하면 추가하지 않는 것을 방지 할 수 있음.

```typescript
enum Fruit {
  Apple,
  Banana,
  Orange,
  Orange2,
}

const FRUIT_PRICE: { [key in Fruit]: number } = {
  [Fruit.Apple]: 1000,
  [Fruit.Banana]: 1500,
  [Fruit.Orange]: 2000,
  // enum에 존재하는 Fruit.Orange2 가 정의되어 있지 않으므로 Error
};
```

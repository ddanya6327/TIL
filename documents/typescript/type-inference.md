# Type Inference

- 명시적으로 코드를 작성하지 않아도 TS는 타입 추론을 통해 타입 값을 추론 해낼 수 있다.

```typescript
let v1 = 123; // v1은 number type
let v2 = "abc"; // v2는 string type

v1 = "a"; // Error
v2 = 456; // Error
```

- const 의 경우는 좀 더 엄격하게 타입 추론을 한다.

```typescript
// v1의 type은 number 일 것 같지만, const의 경우는 v1의 tpye은 123이 된다. v2의 type은 "abc"
const v1 = 123;
const v2 = "abc";
let v3: typeof v1 = 234; // v1의 타입은 number가 아닌 123이므로 234는 타입이 일치하지 않음.
```

- 배열과 객체의 경우

```typescript
const arr1 = [10, 20, 30]; // type은 number[]
const [n1, n2, n3] = arr1; // 비구조화 할당. n1 = number, n2 = number, n3 = number

const obj = { id: "abc", age: 123, language: "korean" };
const { id, age, language } = obj; // id = string, age = number, language = string
```

- interface의 경우

```typescript
interface Person {
  name: string;
  age: number;
}
interface Korean extends Person {
  liveInSeoul: boolean;
}
interface Japanese extends Person {
  liveInTokyo: boolean;
}

const p1: Person = { name: "mike", age: 23 };
// p1 = Person;
const p2: Korean = { name: "mike", age: 25, liveInSeoul: true };
// p2 = Korean;
const p3: Japanese = { name: "mike", age: 26, liveInTokyo: false };
// p3 = Japanese;

// interface 배열의 경우, 다른 type으로 할당 가능한 type은 제거한다. Korean, Japanese 둘 다 Person에 할당 가능하므로, arr1 = Person[];
const arr1 = [p1, p2, p3];

// Korean, Japanese 서로 할당될 수 없으므로 제거되지 않는다.
// arr2: (Korean | Japanese)[]
const arr2 = [p2, p3];
```

- 함수의 타입 추론

```typescript
function func1(a = "abc", b = 10) {
  return `${a} ${b}`;
}
// function func1(a?: string,b?: number): string
func1(3, 6); // a = string 이므로 Error

// 반환 값은 string이므로 Error
const v1: number = func1("a", 1);

// 2개 이상의 return 을 가진 경우에도 타입 추론이 가능
function func2(value: number) {
  if (value < 10) {
    return value;
  } else {
    return `${value} is too big`;
  }
}
// function func2(value: number): string | number

// 매개변수의 type이 여러개인 경우는 명시적으로 써주면 됨.
```

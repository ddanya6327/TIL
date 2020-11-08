# this type

- 함수의 매개변수의 맨 앞에 `this: type` 형태로 정의한다. (매개변수 취급 하지 않음)

```typescript
function getParam(this: string, index: number): string {
  // ...
  return this.split(",");
}
```

- prototype 을 이용해서 속성을 주입할 때, 타입을 정의 하기 위해서는 interface를 사용하면 된다.

```typescript
// ...
String.prototype.getParam = getParam; // getParam Type이 없으므로 Error
```

```typescript
// ...

interface String {
  getParam(this: string, index: number): string;
}

String.prototype.getParam = getParam;
```

## 2개 이상의 Type 값을 가진 경우

```typescript
function add(x: number | string, y: number | string): number | string {
  return ""; // number or string
}

const v1: number = add(1, 2); // Error, v1은 number type 이지만, add의 반환 값은 string이 될 수도 있기 때문.
console.log(add(1, "2")); // 이 경우는 Error가 발생하지 않는다. 함수가 number + number, string + string 을 원한다면 문제가 될 수 있음.
```

- 위와 같은 경우에는 정의한 함수 위에 같은 이름으로 타입을 정의 하는 함수 오버로드를 이용해 해결 할 수 있다.

```typescript
function add(x: number, y: number): number; // type 정보, comfile시 사라짐
function add(x: string, y: string): string;
function add(x: number | string, y: number | string): number | string {
  return ""; // number or string
}
const v1: number = add(1, 2); // OK
console.log(add(1, "2")); // number + string은 허용되지 않으므로 Error
```

## named parameters (매개변수 이름 부여)

```typescript
function getText({
  name,
  age = 15,
  language,
}: {
  name: string;
  age?: number;
  language?: string;
}): string {
  // ...
  return "";
}

// 함수 사용시 이름을 적어 정의 가능.
getText({ name: "aaa", age: 11 });
```

- 파라메터를 객체로 감싸주고 그 뒤에 타입을 정의 해줌.
- named paramters 를 다른 곳에서도 사용하고 싶다면 interface 형태로 사용한다.

```typescript
interface Param {
  name: string;
  age?: number;
  language?: string;
}

function getText({ name, age = 15, language }: Param): string {
  // ...
  return "";
}
```

- 기존의 함수를 named parameters 형태로 변환하기엔 번거롭지만, typescript에서는 변환 함수를 제공하기 때문에 그걸 사용하면 쉽게 변환 할 수 있다.

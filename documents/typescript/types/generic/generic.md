# Generic

- 타입 정보가 동적으로 결정되는 타입
  - 같은 규칙을 여러 타입에 적용 할 수 있어서 중복 코드를 제거 할 수 있음.

예를 들어, 같은 일을 하지만 매개변수가 다른 함수가 여러개 있다면

```typescript
// return number array
function makeNumberArray(value: number): number[] {
  const arr: number[] = [];
  // ...
  return arr;
}

// return string array
function makeStringArray(value: string): string[] {
  const arr: string[] = [];
  // ...
  return arr;
}
```

위와 같은 경우, 함수 overload를 사용해 코드를 줄일 수 있다.

```typescript
function makeArray(value: number): number[];
function makeArray(value: string): string[];
function makeArray(value: number | string): Array<number | string> {
  const arr: Array<number | string> = [];
  // ...
  return arr;
}
```

수정이 없다면 위의 경우로도 괜찮지만 type을 추가해야 한다면?

```typescript
function makeArray(value: number): number[];
function makeArray(value: string): string[];
function makeArray(value: boolean): boolean[]; // type에 대한 정의를 계속 추가해야 한다.
function makeArray(
  value: number | string | boolean
): Array<number | string | boolean> {
  const arr: Array<number | string | boolean> = [];
  // ...
  return arr;
}
```

이런 경우 제네릭을 사용해 코드를 줄일 수 있다.

- 함수 이름 오른쪽에 <> 를 이용해서 Type을 입력한다.

```typescript
function makeArray<T>(value: T): T[] {
  const arr: T[] = [];
  // ...
  return arr;
}

const arr1 = makeArray<number>(1);
const arr2 = makeArray<string>("empty");
const arr3 = makeArray<boolean>(true);
```

`makeArray<number>(1)`와 같이 호출시에 type 정보를 입력해도 되지만, 입력하지 않아도 TS에서 자동으로 처리 해준다.

```typescript
const arr1 = makeArray(1); // makeArray<number>(1);
const arr2 = makeArray("empty"); // makeArray<string>("empty");
const arr3 = makeArray(true); // makeArray<boolean>(true);
```

## Generic 이용시 Type 종류 제한

- extends 키워드를 이용

```typescript
function identity<T extends number | string>(p1: T): T {
  return p1;
}

identity(1);
identity("1");
identity([]); // Error
identity(true); // Error
```

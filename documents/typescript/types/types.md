# Types

## Num, Boolean, String

```typescript
const num: number = 123;
const bool: boolean = true;
const string: string = "string";
```

## Array

```typescript
const num_arr: number[] = [1, 2, 3];
const num_arr2: Array<number> = [1, 2, 3]; // number[] 와 동일
// num_arr.push('a'); // 'a'는 문자이므로 Error
```

## Tuple Type

```typescript
const data: [string, number] = ["string", 123];
```

## Undefined, Null

```typescript
const v1: undefined = undefined;
const v2: null = null;
// v1 = 123; => undefined가 아니므로 Error
```

## Union Type (2개 이상의 Type)

```typescript
const v3: number | undefined = undefined; // number 또는 undefined

// 숫자와 문자의 리터럴도 Type으로 정의 가능하다.
let num1: 10 | 20 | 30;
num1 = 10;
// num1 = 15; => Error

let str1: "type" | "script";
str1 = "type";
// str1 = 'JS' => Error;
```

## Any (모든 값)

```typescript
// 타입을 알 수 없거나 타입 정의가 안된 외부 패키지를 사용하는 경우에 사용하기 좋다.
// 단, Any Type을 너무 자주 사용한다면 TS를 쓰는 이유가 없으므로 되도록 사용하지 않는 편이 좋다.
let value: any;
value = 123;
value = "456";
value = () => {};
```

## Function 반환 타입

```typescript
// 반환 값이 없는 경우 void
function f1(): void {
  console.log("hello");
}

// 항상 예외가 되거나 무한루프 때문에 종료가 되지 않는 함수는 never (잘 사용되지 않음)
function f2(): never {
  throw new Error("error");
}

function f3(): never {
  while (true) {
    // ...
  }
}
```

## Object

```typescript
let v: object;
v = { name: "abc" };
// console.log(v.age); => 객체의 속성 정보가 없을 경우, 특정 속성에 접근하면 Error (interface 필요)
```

## Union Type과 Intersection Type을 이용한 교집합과 합집합

```typescript
let intersection1: (1 | 3 | 5) & (3 | 5 | 7);
// intersection1 = 1 # => Error
// intersection1 = 3 # => OK
// intersection1 = 5 # => OK
// intersection1 = 7 # => Error

let union1: (1 | 3 | 5) | (3 | 5 | 7);
// 1, 3, 5, 7 # => OK
```

## Alias (타입의 별칭 주기)

```typescript
type Width = number | string;
let width: Width;
width = 100;
width = "100px";
// width = true; # => Error
```

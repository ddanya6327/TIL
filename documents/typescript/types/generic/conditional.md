# Conditional Types

- `extends` 키워드와 `?`를 사용해 표현

```typescript
type IsString<T> = T extends string ? "yes" : "no";
type T1 = IsString<string>; // T1 = yes
type T2 = IsString<number>; // T2 = no
```

- 조건부 타입 코드를 union type 과 함께 사용하면?

```typescript
type IsString<T> = T extends string ? "yes" : "no";

// 아래의 경우 yes나 no 둘 중 하나만 나올 것 같지만 union type과 조건부 타입을 같이 사용하면 아래와 같은 결과가 나옴.
type T1 = IsString<string | number>;
// T1 = 'yes' | 'no'

type T2 = IsString<string> | IsString<number>;
// T2 = 'yes' | 'no'

// 아래의 코드도 string[] | number[] 타입이 만들어 질 것 같지만, 조건부 타입이 아니기 때문에 T3 = (string | number)[] 와 같은 결과가 나온다.
type Array2<T> = Array<T>;
type T3 = Array2<string | number>;
// T3 = (string | number)[]
```

## Exclude

- 특정 타입을 제외 할 수 있음.

```typescript
// never 라는 타입은 TS 에서 자동적으로 제거된다.

type T1 = number | string | never;
// Exclude는 TS 기본함수
type Exclude<T, U> = T extends U ? never : T;
type T2 = Exclude<1 | 3 | 5 | 7, 1 | 5 | 9>;
// T2 = 3 | 7;
type T3 = Exclude<string | number | (() => void), Function>;
// T3 = string | number

// Extract는 TS 기본 함수
type Extract<T, U> = T extends U ? T : never;
type T4 = Extract<1 | 3 | 5 | 7, 1 | 5 | 9>;
// T4 = 1 | 5
```

## ReturnType

- T가 함수 일 때, T 함수의 반환 타입을 뽑아줌

```typescript
// ReturnType은 TS 내장함수
// infer 키워드를 사용하여 함수의 반환 타입을 변수 R 에 담음.
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
type T1 = ReturnType<() => string>;
function f1(s: string): number {
  return s.length;
}
type T2 = ReturnType<typeof f1>;
```

## Omit

- 특정 타입을 제거

```typescript
// Omit은 TS 내장함수
type Omit<T, U extends keyof T> = Pick<T, Exclude<keyof T, U>>;

interface Person {
  name: string;
  age: number;
  nation: string;
}
type T1 = Omit<Person, "nation" | "age">;
// T1 = {
//   name: string
// }
```

## Overwrite

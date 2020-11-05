# Function Type

- function 함수명(매개변수:타입, 매개변수:타입): 반환값타입

```typescript
function getText(name: string, age: number): string {
  // ...
  return "string";
}

const v1: string = getText("name", 10); // OK
const v2: string = getText("name", "10"); // 매개변수 age의 type은 number이므로 Error
const v3: number = getText("name", 10); // getText의 반환값은 String 이지만 v3 의 type은 number이므로 Error
```

## 함수를 저장하는 경우

- 화살표 기호를 사용해서 타입을 정의

```typescript
const getText: (name: string, age: number) => string = function (name, age) {
  return "";
};
// 함수를 구현하는 부분 function (name, age) 에서는 따로 type을 정의하지 않아도 된다. 변수에서 함수의 매개변수와 반환값의 타입을 지정했기 때문.
```

## optional parameters (선택 매개변수)

- 매개변수명 뒤에 ?를 붙여서 표현

```typescript
function getText(name: string, age: number, language?: string): string {
  // ...
  return "";
}
getText("mike", 10, "ko"); // OK
getText("mike", 10); // language? 이므로 매개변수가 없어도 OK
getText("mike", 10, 11); // language의 type는 string이므로 Error
```

- 단, 선택 매개변수가 매개변수들 중간에 위치하면 에러가 발생한다.

```typescript
function getText(name: string, language?: string, age: number): string {
  // language? 가 중간에 있기 때문에 Error
  return "";
}
```

- 아래와 같이 사용 할 수는 있지만 가독성이 좋지 않으므로 권장하지 않음.

```typescript
function getText(
  name: string,
  language: string | undefined,
  age: number
): string {
  // language? 가 중간에 있기 때문에 Error
  return "";
}
getText("mike", undefined, 10);
```

## 기본값 입력을 통한 type 정의

- 기본값 입력을 통해 type을 정의 할 수도 있다.
  - 예를 들어, 파라미터 `language = "ko"`라고 입력해 준다면 `"ko"`는 string이므로 language의 type은 자동적으로 string이 된다.

```typescript
function getText(name: string, age: number = 15, language = "ko"): string {
  return "";
}
```

- 기본값을 입력 한다는 것은 함수를 호출 할 때 해당 매개변수를 입력하지 않아도 된다는 뜻이므로 optional parameter가 된다.

## rest parameters (나머지 매개변수)

```typescript
function getText(name: string, ...rest: number[]): string {
  return "";
}
getText("mike", 1, 2, 3);
```

- rest type의 파라메터는 항상 Array로 정의 해야 한다.

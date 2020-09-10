# Data Type

## Primitive Type

- number
- string
- boolean
  - flase: 0, null, undifined, NaN
  - true: any other value
- null
  - null 값을 할당한 경우
- undifined
  - 선언은 했지만 값이 초기화 되지 않음
- symbol
  - 유니크 한 식별자

```javascript
const symbol1 = Symbol("id");

console.log(`value: ${symbol1.description});
// symbol을 그냥 출력하면 에러가 되므로 symbol1.description 같은 형태로 출력한다.
```

## Object

- array
- function
- date
- regexp

# Variable

## Var

- Hoisting 때문에 선언 하기 전에 값을 할당하거나 불러 올 수 있다. 그래서 문제가 생길 수 있으므로, let이나 const를 사용하는걸 권장한다.

```javascript
var a = 1;
```

### Hoisting

- 코드에 선언 된 변수 및 함수를 코드 상단으로 끌어 올리는 것.

## Let

- ES6 에서 추가됨.
- 값을 변경 할 수 있음. (mutable)
- Block Spoce

```javascript
let a = 1;
```

## Constans

- 첫 할당 이후로 변경이 불가능한 데이터 타입 (immutable)

```javascript
const a = 1;
```

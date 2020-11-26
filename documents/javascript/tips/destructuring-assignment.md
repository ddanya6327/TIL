# 구조 분해 할당 (Destructuring Assignment)

## Array

- 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있는 표현식

```javascript
var a, b, rest;
[a, b, ...rest] = [10, 20, 30, 40, 50];
// rest = [30,40,50];

({ a, b } = { a: 10, b: 20 });
// a: 10, b: 20
```

- 변수에 기본값을 할당하면 분해한 값이 `undified`일 때 그 값을 사용.

```javascript
var a, b;

[a = 5, b = 7] = [1];
// a: 1, b는 값이 없으므로 7
```

- 값을 교환 할 때 유용

```javascript
var a = 1;
var b = 2;
[a, b] = [b, a];
// a: 2가되고 b는 1이 됨
```

- 특정 위치의 값이 필요 없을 경우

```javascript
[a, , b] = [1, 2, 3];
// 가운대 값인 2가 무시되고 a: 1, b: 3이 됨.
```

## Object

```javascript
var o = { p: 42, q: true };
var { p, q } = o;
// p: 42, q: true
```

- 새로운 속성명으로 할당하기

```javascript
var o = { p: 42, q: true };
var { p: foo, q: bar } = o;
```

- 중첩된 객체의 구조할당, for of 반복문을 사용한 구조할당 등은 MDN을 참조
  - https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

## 주의점

```javascript
const machine = {
  status: {
    name: node,
    count: 5,
  },
  get() {
    this.status.count--;
    return this.status.count;
  },
};

const { getMachine } = machine;

getMachine(); // undifined
// get 함수에서 this를 사용하고 있지만, 구조 분해 할당을 한다면 machine 로 부터 get 이 분리 되므로 this를 찾을 수 없다.
// getMachine.call(machine); 와 같이 this를 bind 해주면 해결 가능
```

# TypeScript

- 정적 타입 언어
- compile time에 변수의 형이 결정 됨
- MS에서 개발중
- 생산성이 높아진다.

```javascript
// JavaScript
// 아래와 같은 경우, JavaScript에서는 실행하기전 까지는 에러인지 알 수 없다.
const mike = { friend: [] };
const total = mike.friendList.length;

// TypeScript의 경우는 코드 작성중에 변수의 자료형을 알 수 있기 때문에 실행하지 않아도 알 수 있다.
```

## 타입 주석(type annotation)과 타입 추론(type interface)

```typescript
let n: number = 1; // 타입 주석
let m = 2; // 타입을 입력하지 않음. 타입 추론
```

- 타입 추론의 경우 값을 분석해서 타입을 자동으로 결정함

## 튜플(tuple)

- 배열과 같지만 저장되는 아이템의 데이터 타입이 모두 같으면 배열, 다르면 튜플

```typescript
let numberArray: number[] = [1, 2, 3]; // array
let tuple: [boolean, number, string] = [true, 1, "Ok"]; // tuple
```

## 설치

- TypeScript 를 사용하기 위해서는 Node.js 가 필요

0. package.json이 없는 경우 생성해준다.

```
npm init -y
```

1. 타입스크립트를 설치

```
npm install typescript
```

2. 타입스크립트 설정 파일 생성

```
npx tsc --init
// tsconfig.json 파일 생성
// npx 명령어는 node_modules/.bin/ 의 파일을 실행해줌
```

2-1. tsconfig.json 파일의 설정

- target: 컴파일 타겟(es5 등)
- strict: 기존 JS를 TS로 포팅하는 작업이 아니라면 설정 할 필요 거의 없음

## 실행

- 컴파일을 통해 JS파일을 만들고 Node.js로 실행하는 방법.

1. terminal에 npx tsc를 입력

```
npx tsc
// *.ts 파일을 JS로 변환한 *.js가 생성 됨.
```

# TIP

- npx tsc를 이용하면 ts파일을 js로 바꾸고 -> 바뀐 js를 node로 실행해야 하지만, ts-node 를 설치하면 ts -> js와 실행을 함께 해준다.

```
npm i -g ts-node
```

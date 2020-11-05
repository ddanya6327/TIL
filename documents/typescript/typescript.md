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

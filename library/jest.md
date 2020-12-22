# Jest

Facebook 에서 만든 JavaScript Testing Framework

## 설치

```
npm install jest
npm install jest --save-dev
```

- package.json 의 scripts의 test 항목도 변경 해주자.

```json
  "scripts": {
    "test": "jest"
  },
```

### jest가 파일을 찾는 방법

- 파일 이름에 test가 포함: {filename}.test.js
- 파일 이름에 spec이 포함: {filename}.spec.js
- tests 폴더 안의 파일들

## jest의 파일 구조

- describe : 여러 관련 테스트를 그룹화 하는 블록
- it (또는 test) : 개별 테스트를 수행하는 곳. 문장처럼 설명한다.

```javascript
describe("TEST CASE", () => {
  it("TEST", () => {
    // test
  });

  it("TEST", () => {
    // test
  });

  it("TEST", () => {
    // test
  });
});
```

- expect : 값을 테스트 할 때마다 사용.
- matcher : 다른 방법으로 값을 테스트 하도록 매처를 사용.

```javascript
test("two plus two is four", () => {
  expect(2 + 2).toBe(4);
});
```

## jest.fn()

Mock 함수를 생성하는 함수. 단위 테스트 작성할 때, 해당 코드가 의존하는 부분을 가짜로 만들어 줌.

- 의존적인 부분이 있으면 단위 테스트의 결과에 영향을 줄 수 있기 때문에 Mock를 만들어서 사용한다. (서버가 죽어있다던지)

```javascript
const mockFn = jest.fn();

// return 값을 알려주지 않았으므로 호출 결과는 undefined
mockFn(); // undefined
mockFn(1); // undefined
mockFn("a"); // undefined

mockFn.mockReturnValue("return value");
mockFn(); // return value
```

Promise가 필요한 경우

```javascript
mockFn.mockResolvedValue("return promise");
mockFn().then((result) => {
  console.log(result);
});
```

Mock는 호출 횟수, 파라미터를 기억한다.

```javascript
mockFn("a");
mockFn(["b", "c"]);

expect(mockFn).toBeCalledTimes(2);
expect(mockFn).toBeCalledWith("a");
expect(mockFn).toBeCalledWith(["b", "c"]);
```

## node 환경에서 Mongoose를 Jest로 테스트 할 경우

Jest의 기본 테스트 환경은 jsdom 이지만, mongoose에서는 jsdom을 지원하지 않는다. 문제를 해결하기 위해서는 jest의 test 환경을 node로 변경한다.
-> `jest.config.js` 파일을 생성하고

```javascript
module.exports = {
  testEnviroment: "node",
};
```

## 공통 코드 줄이기 (beforeEach)

공통적으로 실행 되어야 할 코드가 있다면 beforeEach에 작성하여 코드를 줄일 수 있다.

```javascript
// beforeEach 사용 전
it("test1") {
  dummy = dummyData;
  // ...
}

it("test2") {
  dummy = dummyData;
  // ...
}

it("test3") {
  dummy = dummyData;
  // ...
}
```

```javascript
// beforeEach 사용

beforeEach(() => {
  dummy = dummyData;
})

it("test1") {
  // ...
}

it("test2") {
  // ...
}

it("test3") {
  // ...
}

// describe 내부나 외부에서도 사용 가능.
```

## async await 를 사용하는 함수의 경우

- 단위 테스트에도 async await를 넣어줘야 한다.

# node-mocks-http

## Unit Test 에서 request, response 객체를 얻으려면?

`node-mocks-http` 모듈을 이용한다.

- https://github.com/howardabrams/node-mocks-http

```javascript
req = httpMocks.createRequest();
res = httpMocks.createResponse();
```

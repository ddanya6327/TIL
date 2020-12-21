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
- it : 개별 테스트를 수행하는 곳. 문장처럼 설명한다.

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

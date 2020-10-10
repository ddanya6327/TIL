# Generator

- 이터러블을 생성하는 함수
  - 코드 블록을 한 번에 실행하지 않고 실행을 일시중지 했다가 필요한 시점에서 재시작 할 수 있는 특수한 함수

```javascript
// * 키워드를 붙여 제네레이터 객체를 선언 할 수 있다.
// 이터레이터 객체를 반환함.
function* f1() {
  console.log("f1-1");
  yield 10; // 첫번째 호출 시에 이 지점까지 실행
  console.log("f1-2");
  yield 20; // 두번째 호출 시에 이 지점까지 실행
  console.log("f1-3");
  return "finished"; // 세번째 호출 시에 이 지점까지 실행
}
const gen = f1();
console.log(gen.next());
console.log(gen.next());
console.log(gen.next());
// f1-1
// {value: 10, done: false}
// f1-2
// {value: 20, done: false}
// f1-3
// {value: "finished", done: true}
```

- 이터레이터(iterator)

  - next 메서드를 가지고 있다.
  - next 메서드는 value와 done 속성 값을 가진 객체를 반환한다.
  - done 속성 값은 작업이 끝났을 때 참이 된다.

- 다음의 조건을 만족하면 반복 가능(iterable)한 객체다.

  - Symbol.iterator 속성값으로 함수를 갖고 있다.
  - 해당 함수(Symbol.iterator)를 호출하면 반복자(iterator)를 반환한다.

- 제네레이터는 iterable 이면서 동시에 iterator 인 객체이다.

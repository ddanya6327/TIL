# Promise

- 비동기 처리를 간편하게 처리 할 수 있도록 도와주는 Object

## state

    - pending -> fulfilled or rejected

## Producer vs Consumer

1. Producer

- 새로운 프로미스가 만들어 질 때에는, 우리가 전달한 함수가 바로 실행 된다.

```javascript
const = promise = new Promise((resolve, reject) => {
    // doing some heavy work (network, read files)

    // 2초간 network 작업을 한다고 가정
    setTimeout(() => {
        resolve('yang');
        // reject(new Error('no network'));
    }, 2000);
});
```

2. Consumer

- then
  - promise 가 정상적으로 수행 되었을 경우, resolve에 전달 된 값을 value로 받을 수 있음.
- catch
  - error가 발생 했을 때의 호출
- finally
  - 성공, 실패 여부와 상관 없이 호출

```javascript
promise
  .then((value) => {
    console.log(value);
  })
  .catch((error) => {
    console.log(error);
  })
  .finally(() => {
    console.log("finally");
  });
```

3. Promise chaining

```javascript
const fetchNumber = new Promise((resolve, reject) => {
  //netWork에 연결 한다고 가정
  console.log("실행");
  setTimeout(() => resolve(1), 1000);
});

fetchNumber
  .then((num) => num * 2)
  .then((num) => num * 3)
  .then((num) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve(num - 1), 1000);
    });
  })
  .then((num) => console.log(num));
```

4. Error Handling

```javascript
const getHen = () =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve("hen"), 1000);
  });
const getEgg = (hen) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${hen} => egg`), 1000);
  });
const cook = (egg) =>
  new Promise((resolve, reject) => {
    setTimeout(() => resolve(`${egg} => Fried eggs`), 1000);
  });

getHen()
  .then((hen) => getEgg(hen))
  .then((egg) => cook(egg))
  .then((meal) => console.log(meal))
  .catch(console.log);
// hen => egg => Fried eggs
```

- Promise의 결과 값이 한 가지인 경우, then 에 함수명만 넣어줘도 자동으로 매개변수로 설정 하도록 만들 수 있다.

```javascript
getHen() //
  .then(getEgg)
  .catch((error) => {
    // getEgg의 에러를 처리 하고 싶을 경우
    return "bread";
  })
  .then(cook)
  .then(console.log)
  .catch(console.log); // 전체적인 error를 처리

// 한 줄로 입력되면 가독성이 안좋지만, // 를 입력해주면 위 처럼 여러줄로 표시함
```

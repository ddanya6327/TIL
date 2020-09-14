# Async & Await

1. Async

```javascript
// promise
function fetchUser() {
  return new Promise((resolve, reject) => {
    // do network request in 10 secs ....

    return "yang";
  });
}

const user = fetchUser();
user.then(console.log);

// async

async function fetchUser() {
  return "yang";
}

const user = fetchUser();
```

2. await

```javascript
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(3000);
  return "apple";
}

async function getBanana() {
  await delay(3000);
  return "banana";
}

// promise로 처리한다면
function pickFruits() {
  return getApple().then((apple) => {
    return getBanana().then((banana) => `${apple} + ${banana}`);
  });
}

pickFruits().then(console.log);

// await로 처리한다면
async function pickFruits() {
  const apple = await getApple();
  const banana = await getBanana();
  return `${apple} + ${banana}`;
}

pickFruits().then(console.log);
```

3. useful Promise APIs

- 여러 Promise의 결과를 집계할 때 유용, 병렬 처리

```javascript
async function pickFruits() {
  const apple = await getApple();
  const banana = await getBanana();
  return `${apple} + ${banana}`;
}
// 이 경우, await getApple()의 결과가 돌아온 뒤 await getBanana() 를 실행하므로, 순차적으로 실행 하기보단 여러 Promise를 동시에 실행하고 싶을 수 있다.
```

그 경우, Promise.all 을 사용해 처리 할 수 있다.

```javascript
function pickAllFruits() {
  return Promise.all([getApple(), getBanana()]).then((fruits) =>
    fruits.join(" + ")
  );
}
pickAllFruits().then(console.log);
```

- 가장 먼저 처리가 완료되는 Promise의 값을 출력

```javascript
function pickOnlyOne() {
  return Promise.race([getApple(), getBanana()]);
}

pickOnlyOne().then(console.log);
```

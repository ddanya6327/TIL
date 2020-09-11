# Array

- 비슷한 타입의 데이터를 묶는 것을 자료 구조 라고 한다.
  - object는 서로 연관된 특징과 행동을 묶는것이고, 비슷한 타입의 object를 묶는 것을 자료 구조 라고 생각 하면 된다.
  - 다른 언어에는 동일한 타입의 데이터를 담을 수 있지만, 자바스크립트는 동적 언어이므로 타입에 관계없이 담을 수 있다. (하지만 권장하지 않음)

1. Declaration (선언)

```javascript
const arr1 = new Array();
const arr2 = [1, 2];
```

2. Index position

```javascript
const fruits = ["apple", "banana"];
// fruits[0] => apple
// fruits[1] => banana
// fruits[2] => undifined
```

3. Looping over an array

- 배열 전체의 데이터를 출력

```javascript
// for
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}

// for of
for (let fruit of fruits) {
  console.log(fruit);
}

// forEach
fruits.forEach((fruit) => console.log(fruit));
```

4. Addtion, deletion, copy

- push
  - 가장 끝에 항목 추가

```javascript
fruits.push("orange");
```

- pop
  - 가장 끝의 항목 삭제

```javascript
fruits.pop(); // delete orange
```

- unshift
  - 가장 앞에 항목 추가

```javascript
fruits.unshift("peach");
```

- shift
  - 가장 앞의 요소를 제거

```javascript
fruits.shift(); // delete peach
```

unshift, shift 는 pop과 push보다 속도가 느리다.
가능하면 pop과 push를 사용하자.

- spilce
  - 특정 index의 데이터를 삭제

```javascript
const fruits = ["apple", "banana", "peach", "apple"];

// splice(start index, delete count);
fruits.splice(1); // index 1 부터 모든 데이터를 삭제
fruits.splice(1, 1); // index 1부터 1개의 항목을 삭제
fruits.splice(1, 1, "melon", "apple"); // index 1부터 1개의 항목을 삭제하고, index 1 부터 melon 과 apple을 추가한다.

fruits.splice(1, 0, "strawberry"); // 데이터를 삭제하지 않고 index 1 위치에 strawberry를 끼워 넣을 수 있음
```

- concat
  - 2개 이상의 배열을 합쳐서 새로운 배열을 생성

```javascript
const newFruits = fruits.concat(frutis2);
```

5. Searching

- 특정 데이터의 index를 찾음

```javascript
const fruits = ["apple", "banana", "peach", "apple"];

// indexOf: 해당 데이터의 index를 반환
fruits.indexOf("apple"); // => 0
fruits.indexOf("peach"); // => 2

// includes: 해당 데이터가 array 안에 있는지 확인
fruits.includes("apple"); // => true
fruits.includes("melon"); // => false

// lastIndexOf: 특정 데이터의 마지막 index를 반환. 위의 경우 apple이 2개가 있는데 마지막 apple의 index를 반환함.
fruits.lastIndexOf("apple"); // => 3
```

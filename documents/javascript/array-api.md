# Array Api

- join(구분자)
  - array -> string 형태로 변환

```javascript
const fruits = ["apple", "banana", "orange"];

const result = fruits.join();
// apple,banana,orange

const result = fruits.join("|");
// apple|banana|orange
```

- split(구분자, limit?)
  - string -> array 형태로 변환

```javascript
const fruits = "apple,banana,orange";
const result = fruits.split(",");
// ["apple", "banana", "orange"]
// 구분자를 전달하지 않으면 하나의 string으로 인식하므로 필수
```

- reverse()

  - 주어진 배열의 순서를 거꾸로 만듬
  - 호출환 배열 자체의 순서를 바꾸므로 주의

- slice(start?, end?)
  - 특정 index 부터 새로운 배열 만들기
  - splice의 경우 원본 배열 자체에도 영향을 주므로 slice 를 사용.
  - end의 index는 배제됨 (end가 5인 경우, index 4까지)

```javascript
const array = [1, 2, 3, 4, 5];
const result = array.slice(2, 5); // 5는 배제되므로 index 2~4 까지의
// [3, 4, 5]
```

- find()
  - 특정 조건을 만족하는 첫 번째 데이터를 찾기
  - 있으면 요소를 반환하고 없으면 undefined를 반환

```javascript
class Student {
  constructor(name, age, enrolled, score) {
    this.name = name;
    this.age = age;
    this.enrolled = enrolled;
    this.score = score;
  }
}

const students = [
  new Students("A", 29, true, 45),
  new Students("B", 28, false, 80),
  new Students("C", 30, true, 90),
  new Students("D", 40, false, 66),
];

const result = students.find((student) => student.score === 90);
// result = Student {name: "C", age: 30, enrolled: true, score: 90}
```

- filter()
  - 특정 조건에 부합하는 데이터들을 return

```javascript
const result = students.filter((student) => student.enrolled);
// [Student, Student]
// enrolled 이 true인 student 값들을 return
```

- map()
  - 배열안의 모든 요소에게 callback을 적용한 값을 새로운 배열로 만들어 return

```javascript
const result = students.map((student) => student.score);
```

- some()
  - 배열안에 특정 조건에 부합하는 데이터가 1개라도 있는지 없는지
  - 하나라도 조건에 만족되는 데이터가 있으면 true를 반환

```javascript
const result = students.some((student) => student.score < 50);
```

- every()

  - some과 비슷하지만 모든 요소가 만족해야 함.

- reduce()
  - 배열의 값을 누적할 때 사용.
  - 배열을 역순으로 호출하고 싶으면 reduceRight 를 사용.

```javascript
const result = students.reduce((prev, curr) => prev + curr.score, 0);
// 281
// 0 + 45 + 80 + 90 + 66
```

- sort()
  - 정렬

```javascript
const result = students
  .map((student) => student.score)
  .sort((a, b) => a - b)
  .join();
```

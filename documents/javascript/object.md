# Object

- object는 key와 value의 집합체
- Primitive Type은 변수 하나당 값을 하나만 담을 수 있다. 이 경우, 여러 값을 처리 해야 할 때, 불편한데 Object의 경우 전달, 호출시 Object 하나만 넘기면 되기 때문에 편리하다.

1. Literals and properties

```javascript
const obj1 = {}; // 'object literal' syntax
const obj2 = new Object(); // 'object constructor' syntax

const yang = { name: "yang", age: 4 };

yang.hasJob = true;
// JavaScript는 동적 언어라서 실시간으로 속성을 추가 할 수 있다. 다만, 유지 보수가 어렵기 때문에 가능하면 중간중간 추가 하는 것 보다 처음부터 선언 해주는게 좋다.
```

2. Computed properties

- 기본적으로 코딩 할 때에는 object.key 같은 형태로 호출하지만, 동적으로 실행 할 때에는 key가 불분명 한 경우가 있기 때문에 Computed properties를 사용하는 경우가 있다.
- key는 항상 string type

```javascript
const yang = { name: "yang", age: 4 };

yang.name;
yang["name"]; // Computed properties 접근 방법
yang["hasJob"] = true;

function printValue(obj, key) {
  console.log(obj.key);
}
printValue(yang, "name"); // undifined

// function printValue(obj, key) {
//   console.log(obj[key]);
// }
// printValue(yang, "name");
// 이 경우 출력이 됨.
```

3. Property value shorthand

```javascript
const person1 = { name: 'bob', age: 2 }
const person2 = { name: 'steve', age: 3 }
const person3 = { name: 'dave', age: 4 }
...

// 같은 형태의 오브젝트를 여러 개 만들 때
const person4 = makePerson('yang', 5); // {name: "yang", age: 5}

function makePerson(name, age) {
    return {
        name,
        age
    }
}
```

4. Constructor Function

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  // return this;
}
```

5. in operator

- key가 있는지 없는지 확인 할 수 있는

```javascript
"name" in object;
"age" in object;

// 해당하는 키가 없는 경우는 undifined를 출력
```

6. for..in vs for..of

- for (key in obj)

```javascript
for (let key in yang) {
  console.log(key);
  // object의 key를 출력
}
```

- for (value of iterable)

```javascript
const array = [1, 2, 3, 4, 5];
for (value of array) {
  console.log(value);
  // array의 value를 출력
}
```

7. cloning

- Object.assign(dest, [obj1, obj2, obj3...])

```javascript
const user = { name: "yang", age: 20 };
const user2 = user;

user2.name = "coder";
// 이 경우 user2의 name을 변경했지만 같은 reference를 사용하고 있기 때문에 user의 name도 변경된다.
```

그러면 배열을 clone 하기 위해서는?

```javascript
// old way
const user3 = {};
for (key in user) {
  user3[key] = user[key];
}

// object.assign
const user4 = Object.assign({}, user);

const fruit1 = { color: "red" };
const fruit2 = { color: "blue", size: "big" };
const mixed = Object.assign({}, fruit1, fruit2);
// 위의 경우 color가 중복되지만, 덮어쓰기를 하기 때문에 fruit2의 color가 적용된다.
```

## ES6+ 의 object

- 더 간결하게 작성할 수 있다.

```javascript
var say = function () {
  console.log("test");
};
var es = "ES";

// ES5
var oldObject = {
  sayJS: function () {
    console.log("JS");
  },
  say: say,
};
oldObject[es + 6] = "good";

// ES6+
const newObject = {
  sayJS() {
    // 익명함수를 사용하는 경우 function 을 입력하지 않아도 됨.
    console.log("JS");
  },
  say, // 함수명과 같다면 say: say 같은 형태로 쓰지 않아도 됨.
  [es + 6]: "good", // 동적 할당도 object 내부에서 가능
};
```

```javascript
{ data: data, value: value, length: length }
// ES6+ : { data, value, length } 로도 정의 가능
```

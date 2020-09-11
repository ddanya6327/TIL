# Operator

1. String concatenation

```javascript
console.log("my" + "cat");
console.log("1" + 2);
console.log(`1 + 2 = ${1 + 2}`);
```

2. Numeric operatiors

```javascript
1 + 1;
1 - 1;
1 / 1;
1 * 1;
5 % 2; // 나머지
2 ** 3; // 2의 3승
```

3. Increment and decrement operators

```javascript
let counter = 2;

counter++;
++counter;
counter--;
--counter;
```

4. Assignment operators

```javascript
let x = 3;
let y = 6;

x += y;
x -= y;
x *= y;
x /= y;
```

5. Comparison operators

```javascript
10 < 6;
10 <= 6;
10 > 6;
10 >= 6;
```

6. Logical operators: || (or), && (and), ! (not)

```javascript
const a = true;
const b = false;
const c = null;

// || (or) , finds the first truthy value

// 하나라도 true가 나온다면 뒤의 연산은 진행하지 않음.
// 이 경우, a가 true이기 때문에 뒤의 연산을 시행하지 않는다.
a || b || c || check();
// 이 특성을 살려서 check같이 해야 할 연산이 큰걸 뒤에 두는게 좋다.

// && (and) , finds the first falsy value

// 앞에서 false가 하나라도 나오면 뒤의 연산이 실행되지 않으므로, heavy한걸 뒤에 배치하는게 좋음.
b && a && c && check();

// and의 경우 null check에도 많이 사용한다.

// ! (not)
!a; // => a is false
```

7. Equality

```javascript
const stringFive = "5";
const numberFive = 5;

// == loose equality, with type conversion
stringFive == numberFive; // true
stringFive != numberFive; // false

// === strict equality, no type conversion
stringFive === numberFive; // false
stringFive !== numberFive; // true

// strict equality 의 경우 type 까지 체크하므로 '5'와 5는 flase의 결과가 나온다.

// object equality by reference
const yang1 = { name: "yang" };
const yang2 = { name: "yang" };
const yang3 = yang1;

yang1 == yang2; // false
yang1 === yang2; // false
yang1 === yang3; // true

// 내용이 같아 보이더라도 reference 주소값이 다르기 때문에 yang1과 yang2는 flase의 결과가 나온다.
// yang3은 yang1 의 주소를 할당 받았으므로 같은 값이 나온다.
```

8. Conditional operators: if

- if, else if, else

```javascript
if (name === "yang") {
    ...
} else if (name === "no") {
    ...
} else {
    ...
}
```

9. Ternary operator: ?

- condition ? value1 : value2;

```javascript
name === "yang" ? "yes" : "no";
```

10. Switch statement

```javascript
const browser = "IE";

switch (browser) {
  case "IE":
    console.log("IE");
    break;
  case "Chrome":
  case "Firefox":
    console.log("ok");
    break;
  default:
    console.log("same");
    break;
}
```

11. Loops

```javascript
let i = 3;
while (i > 0) {
  console.log(i);
  i--;
}
```

```javascript
do {
    ...
} while (i > 0);
```

```javascript
// for(begin; condition; step;)
for (let i = 3; i > 0; i--) {
  console.log(i);
}
```

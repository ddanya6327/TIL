# JSON

JavaScript Object Notation

1. Object to JSON

- JSON.stringify()

```javascript
let json = JSON.stringify(["apple", "banana"]);

const rabbit = {
  name: "tori",
  color: "white",
  size: null,
  birthDate: new Date(),
  symbol: Symbol("id"), // Symbol은 제외 된다.
  jump: () => {
    console.log("Jump"); // 함수는 제외 된다.
  },
};

json = JSON.stringify(rabbit);
// "{"name":"tori","color":"white","size":null,"birthDate":"2020-09-14T02:38:03.871Z"}"

// name, color만 json으로 만듬
json = JSON.stringify(rabbit, ["name", "color"]);
//"{"name":"tori","color":"white"}"

json = JSON.stringify(rabbit, (key, value) => {
  console.log(value);
  return value;
});
// 첫 번째 value는 해당 object를 반환함.
// key: , value: [object Object]
// key: name, value: tori
// key: color, value: white
// key: size, value: nul

// 이런 식으로 사용 가능
json = JSON.stringify(rabbit, (key, value) => {
  console.log(value);
  return key === "name" ? "yang" : value;
});
```

2. JSON to Object

- JSON.parse()

```javascript
json = JSON.stringify(rabbit);
const obj = JSON.parse(json);

// rabbit Object의 경우 birthDate가 Date Type 이었기 때문에 Date 관련 함수를 사용 가능했지만, 단순히 JSON.parse(json)만 해주면 birthDate는 String이 되므로 date 함수를 사용 할 수 없다. 그런 경우 이런 식으로 사용하면 된다.
const obj = JSON.parse(json, (key, value) => {
  return key === "birthDate" ? new Date(value) : value;
});
```

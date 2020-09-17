# Class

- fields(속성)과 methods(행동)으로 구성된 것.
  - methods없이 fields만으로 구성된 data class도 있음.

## Class와 Object의 차이

class

- template
- no data in
- class는 데이터가 없고 틀만 정의 해둔 것.
- ES6에 추가 됨. (prototype을 base로 만들어짐.)

object

- instance of a class
- class에 data를 넣어서 만든 것.
- class는 정의만 했기 때문에 메모리상에 없지만, object는 실제 메모리에 올라감.

1. Class의 선언

```javascript
class Person {
  constructor(name, age) {
    // fields
    this.name = name;
    this.age = age;
  }

  // methods
  // 클래스에서 함수를 선언 할 때에는 function 이라는 키워드를 사용하지 않아도 된다.
  speak() {
    console.log(`${this.name}: hello!`);
  }
}

const yang = new Person("yang", 20);
yang.speak();
```

2. Getter and setters

```javascript
class User {
  constructor(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
  }

  get age() {
    return this._age;
  }

  set age(value) {
    this._age = value;
  }
}

const user1 = new User("Steve", "Job", -1);
console.log(user1.age);
```

3. Fields (public, private)

- 최근에 추가 된 옵션이라 지원하지 않는 브라우저도 있음.

```javascript
class Experiment {
  publicField = 2;
  #privateField = 0;
}
const experiment = new Experiment();
console.log(experiment.publicField);
console.log(experiment.privateField); // undifined
```

4. Static properties and methods

- 최근에 추가 된 옵션이라 지원하지 않는 브라우저도 있음.

```javascript
class Article {
  static publisher = "yang";
  constructor(articleNumber) {
    this.articleNumber = articleNumber;
  }

  static printPublisher() {
    console.log(Article.publisher);
  }
}

const article1 = new Article(1);
const article2 = new Article(2);
console.log(Article.publisher);
Article.printPublisher();
```

5. Inheritance (상속 & 다양성)

```javascript
class Shape {
  constructor(width, height, color) {
    this.width = width;
    this.height = height;
    this.color = color;
  }

  draw() {
    console.log(`drawing ${this.color} color of`);
  }

  getArea() {
    return this.width * this.height;
  }
}

class Rectangle extends Shape {}
class Triangle extends Shape {
  draw() {
    super.draw();
    console.log("△");
  }
  getArea() {
    return (this.width * this.height) / 2;
  }
}

const rectangle = new Rectangle(20, 20, "blue");
rectangle.draw();
console.log(rectangle.getArea());
const triangle = new Triangle(20, 20, "blue");
triangle.draw();
console.log(triangle.getArea());
```

6. instanceOf

- class checking

```javascript
rectangle instanceof Rectangle; // True
triangle instanceof Triangle; // True
triangle instanceof Shape; // True
triangle instanceof Object; // True
```

## 참고 사이트

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference

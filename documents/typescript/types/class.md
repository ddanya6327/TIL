# Class

```typescript
class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayHello() {
    console.log("hello");
  }
}

class Programmer extends Person {
  constructor(name: string) {
    super(name);
    // 자식 class에서는 super를 호출 해줘야 함.
  }
  sayHello() {
    super.sayHello();
    console.log("fix bug");
  }
}

const programmer = new Programmer("test");
programmer.sayHello();
```

## 접근 범위

- 접근 범위

  - public
  - protected
  - private

- 따로 설정하지 않으면 default는 public

```typescript
class Person {
  // public, protected, private
  public name: string;
  // ...
}
```

- \# 을 사용해서 private를 표현 할 수도 있다.
  - JS 표준으로 채택 된 문법

```typescript
class Person {
  #name: string;
  // ...
}
const person = new Person("test");
console.log(person.#name);
// 사용시에도 #을 붙임
```

- class 객체를 생성하지 않기 위해 생성자에 접근 범위를 설정하는 방법도 있다.

```typescript
class Person {
  protected constructor(name: string) {
    this.name = name;
  }
}
const person = new Person("test"); // Constructor가 Protected 이므로 Error
// Person class는 다른 class를 만들기 위한 용도로 사용하기 때문에 객체를 만들 필요가 없을 경우에 사용.
```

- 접근 범위를 설정하는 키워드나 readonly 같은 것을 사용하면 자동으로 멤버 변수가 된다.

```typescript
class Person {
  public age: number;
  constructor(public name: string, age: number) {
    this.age = age;
    // name의 경우 접근 범위가 설정 되어 있기 때문에 자동으로 멤버 변수가 되고 this.name 같이 설정하지 않아도 자동으로 초기화 해준다.
  }
}

const person = new Person("test", 11); // name 은 자동 초기화
```

## getter, setter

```typescript
class Person {
  private _name: string = "";

  get name(): string {
    return this._name;
  }

  set name(newName: string) {
    this._name = newName;
  }
}

let person = new Person();
person.name = "test"; // call setter
console.log(person.name); // getter
```

## static

- static 키워드를 통해 정적 멤버 변수와 정적 메서드를 정의 할 수 있음.

```typescript
class Person {
  static adultAge = 20;
  // constructor() {}
  static getIsAdult() {
    return Person.adultAge;
  }
}

console.log(Person.adultAge);
```

## abstract

- 객체로 만들 수 없음

```typescript
abstract class Person {
  // constructor
  sayHello() {
    console.log("hello");
  }
  abstract sayHello2(): void;
}

class Programmer extends Person {
  sayHello() {
    super.sayHello();
  }
  // abstract method는 반드시 정의 해줘야 한다.
  sayHello2() {
    console.log("hello2");
  }
}
```

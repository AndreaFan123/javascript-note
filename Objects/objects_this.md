## Table of contents

- [Method in a Object](#method-in-a-object-物件內的方法)
- [What is this](#so-what-is-this)
- [this is free](#this-是自由的)
- [When this will become undefined](#when-this-will-become-undefined)
- [Global context](#in-global-context-全域執行環境下)
- [Arrow function](#arrow-function-has-no-this-箭頭函式沒有自己的-this)

# Method in a Object 物件內的方法

- 這裡的方法指得是物件內的 actions，因此一個物件內最重要的兩個元素：屬性(property) 跟方法(method)。
- 如何增加方法呢？透過函式來表示一個 action，如下：

```javascript
// 建立方式 1：

let user = {
  name: "John",
  age: 30,
};

// 賦予 user 一個 sayHi() 的 action
// 這裡的函式是函式表達式
user.sayHi = function () {
  console.log("Hello!!");
};

// 執行 sayHi()
user.sayHi(); // 'Hello!!'
```

```javascript
// 建立方式 2:
let user = {
  ...
}

// 這裡使用函式陳述式
function sayHi() {
  console.log('Hello!!');
}
// 再把上述函式作為一個方法加入到 user 物件
user.sayHi = sayHi;
user.sayHi(); // 'Hello!!'
```

```javascript
// 建立方式 3:
let user = {
    name: 'John',
    age: 30,
    sayHi: function() {
        console.log('Hello!!')
    }
}

// 簡寫
let user = {
    name: 'John',
    age: 30,
    sayHi() {
        console.log('Hello!!'),
    }
};

user.sayHi(); //'Hello!!'
```

# So what is `this`?

- `this` 代表著在執行環境下，擁有取得物件資料的關鍵字。

```javascript
let user = {
  name: "John",
  age: 30,
  sayHi: function () {
    console.log(`Hello!! My name is ${this.name}`);
  },
};

user.sayHi(); // Hello!! My name is John
```

- 上述的 `this` 代表 `user`，當然我們也可以直接使用 `user.name`，但若之後我們做了淺拷貝，例如：`user2 = user;` 並且更動了 `user` 物件值，就會導致取用的錯誤，例如：

```javascript
// use this
let user = {
  name: "John",
  age: 30,
  sayHi: function () {
    console.log(`Hello!! My name is ${this.name}`);
  },
};

user2 = user;
user = null;

user2.sayHi(); // Hello!! My name is John

// without using this
let user = {
  name: "John",
  age: 30,
  sayHi: function () {
    console.log(`Hello!! My name is ${user.name}`);
  },
};

user2 = user;
user = null;

user2.sayHi();
// TypeError: Cannot read properties of null (reading 'name')
```

# `this` 是自由的

- 在 JS 的世界裡，`this` 可以在除了物件以外的世界自由活動，這裡區分 `Global context`(全域執行環境) & `function context`(函式執行環境)

> **Note**
>
> The value of `this` is evaluated during the run-time, depending on the context.
>
> `this` 的值將在執行時，取決於執行環境。

```javascript
let father = {
  name: "John",
  age: 30,
  sayHi() {
    console.log(`Hello, I am ${this.name}`);
  },
};

let mother = {
  name: "Sally",
  age: 33,
  sayHi() {
    console.log(`Hello, I am ${this.name}`);
  },
};

father.sayHi(); // Hello, I am John
mother.sayHi(); // Hello, I am Sally
```

# When `this` will become `undefined`?

- 若不在嚴謹模式下的函式，`this` 依舊指向全域物件，但是在嚴謹模式下，若沒有在執行上下文內指定任何值，就會是 `undefined`

```javascript
// without 'use strict'
let user = {
  name: "John",
  age: 30,
  sayHi() {
    console.log(this);
    console.log(this.name);
  },
};

let sayHello = user.sayHi;
sayHello();

// console.log(this) : Window {0: global, window: Window, self: Window, document: document, name: '', location: Location, …}
// console.log(this.name) : undefined
```

> **Note**
>
> 上述程式碼，`this.name` 會出現 `undefined` 的原因是，只有透過 `user` 把方法 `sayHi` 指派給 `sayHello`，這裡的 `sayHello` 執行後，因為沒有物件內的屬性，所以是 `undefined`
>
> 可以透過複製 `user2 = user`，讓 `user2` 可以取得 `user` 的屬性與方法，然後直接執行 `user2.sayHi()`。

```javascript
// use 'use strict'

"use strict";
let user = {
  name: "John",
  age: 30,
  sayHi() {
    console.log(this);
    console.log(this.name);
  },
};

let sayHello = user.sayHi;
sayHello();

// console.log(this) : undefined
// console.log(this.name) : TypeError: Cannot read properties of undefined (reading 'name')
```

# In global context 全域執行環境下

- 在全域的執行環境下，不論有無使用嚴格模式，這裡的 `this` 都指向全域物件。
- `this === window`

```javascript
console.log(this === window); // true

a = 37;
console.log(window.a); // 37

this.b = "Hello";
console.log(window.b); // 'Hello'
console.log(b); // 'Hello'
```

# Arrow function has no `this` 箭頭函式沒有自己的 `this`

- 箭頭函式的 `this` 取決於外部函式。
- 原因在於我們知道如果在函式執行環境下，沒有使用嚴格模式的話，`this === undefined`，在 JS 中，有需要可能是函式內仍須執行另一個函式的狀況，例如：`forEach`，如果在 `forEach()` 使用函式陳述式的話，就會導致 `this === undefined`
- 下面的 `this.place` 是在一個函式陳述式的執行環境下，所以為 `undefined`

```javascript
let books = {
  place: "library",
  titles: ["Harry Potter", "CSS secrets", "Rework"],
  showBooks() {
    this.titles.forEach(function (title) {
      console.log(`${this.place} has ${title}`);
    });
  },
};

books.showBooks();

// undefined has Harry Potter
// undefined has CSS secrets
// undefined has Rework
```

- 若改成箭頭函式，它的 `this` 就取決於 `showBooks` 執行環境的屬性
- 下面的程式碼中，`this.place === book.place`

```javascript
let books = {
  place: "library",
  titles: ["Harry Potter", "CSS secrets", "Rework"],
  showBooks() {
    this.titles.forEach((title) => {
      console.log(`${this.place} has ${title}`);
    });
  },
};

books.showBooks();

// library has Harry Potter
// library has CSS secrets
// library has Rework
```

# Tasks

Following tasks are from [javascript.info tasks](https://javascript.info/object-methods#tasks)

## Task 1:

Here the function makeUser returns an object.
What is the result of accessing its ref? Why?

```javascript
function makeUser() {
  return {
    name: "John",
    ref: this,
  };
}

let user = makeUser();

alert(user.ref.name);
```

## Task 2: Create a calculator

Create an object `calculator` with three methods:

- `read()` prompts for two values and saves them as object properties with names a and b respectively.
- `sum()` returns the sum of saved values.
- `mul()` multiplies saved values and returns the result.

```javascript
let calculator = {
  // ... your code ...
};

calculator.read();
alert(calculator.sum());
alert(calculator.mul());
```

## Task 3: Chaining

There’s a `ladder` object that allows to go up and down:

```javascript
let ladder = {
  step: 0,
  up() {
    this.step++;
  },
  down() {
    this.step--;
  },
  showStep: function () {
    // shows the current step
    alert(this.step);
  },
};
```

Now, if we need to make several calls in sequence, can do it like this:

```javascript
ladder.up();
ladder.up();
ladder.down();
ladder.showStep(); // 1
ladder.down();
ladder.showStep(); // 0
```

Modify the code of `up`, `down` and `showStep` to make the calls chainable, like this:

```javascript
ladder.up().up().down().showStep().down().showStep(); // shows 1 then 0
```

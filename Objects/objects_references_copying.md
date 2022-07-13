## Table of contents

# Objects vs. primitives

1. 物件是傳址： (call by reference || pass by reference)。
2. 原始型別： (string, number, bigint ,boolean, null, undefined, symbol ) 是傳值 (call by value || pass by value)。

# 參考與複製 References & copying

1. 針對原始型別的複製，如下：

```javascript
let a = "Apple";
let b = a;

// 請問 a 和 b 是一樣的嗎？
console.log(b === a); // true

// 這裡意味著 a 和 b 分別存有 Apple 這個值
```

- 原始型別的複製，並不會因為被複製的一方值改變了而有所改變，因為從頭到尾，`a` 和 `b` 都是獨立的擁有自己的值。

```javascript
let a = "Apple";
let b = a;

b = "Banana";

console.log(a); // Apple
console.log(b); // Banana
```

2. 物件的部分不同，物件的複製(假設 `a` 和 `b`)，當我們複製物件 `a = b`，我們是讓 B 擁有可以獲得物件 `a` 內屬性的參考位置，如下：

```javascript
let a = {
  name: "John",
},

let b = a; // copy by referencing
console.log(b === a) // true
```

- 但如果我們透過 `b` 來改變屬性 `name` 的值，這邊可以很明顯地發現，`a` 的值也改變了，可以等於說兩個物件都有可以取得屬性的參考位置，只要有一方改變屬性的值，兩方的值都會改變。

```javascript
let a = {
  name: "John",
},

let b = a;
b.name = "Peter";

console.log(a); // {name: "Peter"}
```

- 既然物件的直接複製，會造成上述狀況，那要如何避免改變某一方的屬性呢？
  - 直接定義一個空物件，並把原本物件內的屬性指派出去。
  - 直接定義兩個物件時，即使內容一樣，也是獨立的兩個物件，因此若不想影響物件內的屬性改變，最好的方式就是在定義一個新的物件。

```javascript
let a = {
  name: "John",
};

let b = {};

for (let key in a) {
  b[key] = a[key];
}

console.log(a); //{name: "John"}
console.log(b); //{name: "John"}

b.name = "Pete";

console.log(a); //{name: "John"}
console.log(b); //{name: "Pete"}
```

- 使用 ES6 的語法，可以更簡潔地複製物件 [Spread operator](https://javascript.info/rest-parameters-spread)

```javascript
let a = {
  name: "John",
};

// ES6 spread operator
let b = { ...a };

b.name = "Pete";

console.log(a); //{name: "John"}
console.log(b); //{name: "Pete"}
```

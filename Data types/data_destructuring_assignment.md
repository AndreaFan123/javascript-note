## Table of Contents

# Destructuring assignment 解構賦值

解構賦值使我們可以將陣列或物件拆解至一連串的變數。

## Array destructuring 陣列的解構賦值

```javascript
let arr = ["John", "Smith"];
let [firstName, lastName] = arr;

console.log(firstName); // John
console.log(lastName); // Smith
```

> **Note**
>
> 雖然稱為解構(destructuring)，但沒有任何破壞的意思，只是拆開陣列或物件，複製裡面的元素，原本的陣列或物件並沒有被修改。

```javascript
// 以下程式碼等同於上面的程式碼
let arr = ["John", "Smith];

let firtName = arr[0];
let lastName = arr[1];
```

### 使用逗號忽略元素

```javascript
// Case 1: 省略中間元素
let arr = ["Apple", "Banana", "Orange"];

let [first, , second] = arr;
console.log(first); // Apple
console.log(second); // Orange

// Case 2 : 省略最後的元素
let arr = ["Apple", "Banana", "Orange"];

let [first, second] = arr;
console.log(first); // Apple
console.log(second); // Banana

// Case 3 : 省略第一個元素
let arr = ["apple", "Banana", "orange"];

let [, first, second] = arr;
console.log(first); // Banana
console.log(second); // Orange
```

### 等號右側可以是任何可迭代的對象

```javascript
let [a, b, c] = "abc";

console.log([a, b, c]);
// ["a", "b", "c"]

let [a, b] = "John Smith".split(" ");
console.log([a, b]);
// ["John", "Smith"]
```

### 可以賦值給等號左側

```javascript
let father = {};

[father.name, father.surname] = "John Smith".split(" ");

console.log(father);
// {name: "John", surname: "Smith"}
console.log(father.name);
// John
console.log(father.surname);
// Smith
```

### ...rest 其餘

透過 `...rest` 來存取剩餘的元素，rest 的值就代表陣列中剩餘的數值，但不一定要使用 `rest` 可以使用其他名詞，只要確保前面有三個點。

```javascript
let [ele1, ele2, ...rest] = ["a", "b", "d", "e", "f", "g"];

console.log(rest[0]); // d
console.log(rest.length);
```

### Default 默認值

陣列內變數若過少，在解構賦值中不會報錯，缺少的對應變數會變成 `undefined`。

```javascript
let [firstName, lastName] = ["John"];

console.log(firstName); // John
console.log(lastName); // undefined
```

這個狀況下，我們可以透過 `=` 等號來給予默認值。

```javascript
let [firstName, lastName = "Doe"] = ["John"];

console.log(firstName); // John
console.log(lastName); // Doe
```

默認值的情境中，也可以是函式調用，不過該函式只有在變數未被賦值的狀況下才會被調用。如下述程式碼，由於 `firstName` 有對應的 `John`，若運行此程式碼只會顯示 `Your surName?`

```javascript
let [firstName = prompt("Your name"), lastName = prompt("Your surname?")] = [
  "John",
];

console.log(firstName); // John
console.log(lastName); // Your answer
```

---

## Object destructuring 物件的解構賦值

基本表示方式： `let {x, y} = {a:..., b:...}`

```javascript
let user = {
  firstName: "John",
  weight: 69,
  height: 179,
};

let { firstName, weight, height } = user;

console.log(firstName); // John
console.log(weight); // 69
console.log(height); // 179

// 非解構賦值的方式，透過 user 抓取物件的值
let user = {
  firstName: "John",
  weight: 69,
  height: 179,
};

console.log(user.firstName); // John
console.log(user.weight); // 69
console.log(user.height); // 179
```

等號左側的順序並不影響結果，如下：

```javascript
let user = {
  firstName: "John",
  weight: 69,
  height: 179,
};

let { weight, firstName, height } = user;

console.log(user.firstName); // John
console.log(user.weight); // 69
console.log(user.height); // 179
```

### 透過冒號來設置變數名稱

以下面的例子來說，屬性 `firstName` 被賦值給變數 `f`，以此類推。

```javascript
let user = {
  firstName: "John",
  weight: 69,
  height: 179,
};

let { firstName: f, weight: w, height: h } = user;

console.log(f); // John
console.log(w); // 69
console.log(h); // 179
```

### 等號來設定默認值

和陣列一樣，可以透過等號來設定默認值。

```javascript
let user = {
  firstName: "John",
  weight: 69,
};

let { firstName, weight, height = 179 } = user;

console.log(firstName); // John
console.log(weight); // 69
console.log(height); // 179
```

和陣列一樣，也可以調用函式，但僅會針對未提供對應值的狀況下才會被調用。

### ...rest 剩餘

基本上和陣列操作一樣。

## 巢狀解構

```javascript
let user = {
  fullName: { firstName: "John", lastName: "Smith" },
  age: 34,
  children: ["Ted", "Mike"],
};

let {
  fullName: { firstName, lastName },
  age,
  children: [child1, child2],
} = user;

console.log(firstName); // John
console.log(lastName); // Smith
console.log(age); // 34
console.log(child1); // Ted
console.log(child2); // Mike
```

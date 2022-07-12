## Table of contents:

- [Function Declaration vs. Function Expression](#function-declaration--function-statement-函式宣告--函式陳述式)
- [Local Variables vs. Outer Variables](#local-variables-局部變數-vs-outer-variables-全域變數)
- [Parameters vs. Arguments](#paremeters-vs-arguments-參數-vs-引數)
- [Function Naming convention](#函式命名的規範)
- [Tasks](#tasks)

# Function Declaration / Function Statement vs. Function Expression (函式陳述式 vs. 函式表達式)

## 觀念釐清：

1. 在 JS 內，函式是值，如果當作值讓其他使用，不加()，那就會顯示`sayHi`本身的樣子，但如果加上()，則是會執行函式本身。

```javascript
function sayHi() {
  console.log("Hello world");
}
console.log(sayHi); // [Function: sayHi]
sayHi(); // Hello world
```

2. 正因為在 JS 內，函式是值，因此可以複製並指派給另外一個變數。

```javascript
function sayHi() {
  console.log("Hello world");
}

let sayHello = sayHi;
sayHello(); // Hello world
```

## 寫法：

```javascript
// 函式陳述式
function sayHi() {
  console.log("Hello world");
}
sayHi(); // Hello world

// 函式表達式
var sayHi = function () {
  console.log("Hello world");
};
sayHi();
```

## 函式陳述式特性：

1. 本身不會回傳值。
2. 在電腦創造階段，整段函式就會被寫進記憶體，因此在函式前呼叫不會出錯，如下圖：

```javascript
sayHi(); // Hello world

function sayHi() {
  console.log("Hello world");
}
```

3. 拆解

```javascript
// 創造階段 creation phase
function sayHi(){...}

// 執行階段 execution phase
sayHi()
```

## 函式表達式特性：

1. 本身會回傳值。
2. 在電腦創造階段僅會將變數寫進記憶體，因此在執行階段時，若把呼叫放函式前會出錯 (會變成記憶體內僅有變數名稱，但到執行時，並沒有任何可執行的函式在記憶體內，接著才會將真正地的函式寫進記憶體，但此時不能重新宣告變數)

```javascript
sayHi(); // error

var sayHi = function () {
  console.log("Hello world");
};
```

3. 拆解

```javascript
// 創造階段 create phase
var sayHi; // 將變數 sayHi 寫進記憶體

// 執行階段 execution phase
sayHi(); // 執行 sayhi() 但沒有東西

sayHi = function () {
  console.log("Hello world");
}; // 重新宣告 sayHi 等於這段程式，但沒有意義
```

# Local Variables 局部變數 vs. Outer Variables 全域變數

1. 前者是在函式內被定義的變數，後者是在函式外被定義的變數。

```javascript
let user = "Jonn"; // outer variables

function sayHi() {
  let user = "Ben"; // local variable
  alert(user);
}
```

2. 局部變數僅有函式本身可以調用

```javascript
function sayHi() {
  let user = "Ben";
  alert(user);
}
sayHi(); // Ben
alert(user); // error, can't access local variable
```

3. 函式可以完全調用全域變數，且可以在內部修改值，但要注意執行前後的值的變化。

```javascript
let user = "Jonn"; // outer variables

function sayHi() {
  let user = "Ben"; // local variable
  alert(user);
}

alert(user); // John ---A
sayHi(); // Ben ---B
alert(user); // Ben ---C
```

- 腦由上往下逐一讀取時，讀到 A 時，此時的 `user` 是全域變數的。
- 接著讀到 `sayHi()` 時，就返回上方去執行 `say()`，而原本的 `user`值被指派另外一個 `Ben` 的值。
- 最後階段，因為記憶體內的 `user` 值已經從 `John` 改為 `Ben` 所以會是 `Ben`。

> 只有在沒有局部變數時，函式才會調用全域變數，建議減少全域變數，盡可能在函式內定義局部變數。

# Paremeters vs, arguments (參數 vs. 引數)

## 兩者定義：

- parameters(參數)：函式括號內的值，代表即將代入韓式的實值
- arguments(引數)：呼叫函式時要傳遞給函式的值

```javascript
// 這裡的 name 是 parameter
function sayHiTo(name) {
  console.log(`Hello, nice to meet you ${name}`);
}

sayHiTo("Ben"); // 這裡的值是 argument
```

## 使用情境：

1. 一般狀況下，parameter 和 argument 的數量要相等，如果：
   - argument 比 parameter 多，多出來的沒作用。
   - parameter 比 argument 多，多的會變 `undefined`
2. 可以指定默認的參數，當沒有相對的 argument 給予時，就會以默認值呈現。

```javascript
function sayHiTo(name, age = 18) {
  console.log(`Hello, I am ${name}, I am ${age} years old`);
}

sayHiTo("Ben"); // Hello, I am Ben, I am 18 years old
```

- 若不想指定參數的話，則可以在函式內進行條件的篩選

```javascript
function sayHiTo(name, age=19) {
  if(name === undefined) {
      name = "Ben"
  }
    console.log(`Hello I am ${name}, I am ${age} years old`)
}
sayHiTo() Hello I am Ben, I am 19 years old
```

# 函式命名的規範

## 觀念釐清：

1. 函式本身為 action，因此通常以動詞開頭，內部執行的函式內容也最好與該名稱有連結，例如：

- `getNum` : 獲得一個數字，內部函式的 action 最好也與該名稱符合。
- `getNums` : 獲得多個數字
- `checkNum` : 檢查數字
- `createNum` : 建立數字

# Tasks

> 這裡的題目都來自 [javascript.info](https://zh.javascript.info/function-basics#tasks)

## Taks 1 : Is "else" required?

The following function returns true if the parameter age is greater than 18.
Otherwise it asks for a confirmation and returns its result:

```javascript
function checkAge(age) {
  if (age > 18) {
    return true;
  } else {
    // ...
    return confirm("Did parents allow you?");
  }
}
```

Will the function work differently if else is removed?

```javascript
function checkAge(age) {
  if (age > 18) {
    return true;
  }
  // ...
  return confirm("Did parents allow you?");
}
```

### SOLUTION :

這兩個函式執行起來是一樣的，只要 `if` 的結果是 `falsy`，就會執行 `Did parents allow you` 這段函式。

---

## Task 2 : Rewrite the function using '?' or '||'

The following function returns true if the parameter age is greater than 18.
Otherwise it asks for a confirmation and returns its result.

```javascript
function checkAge(age) {
  if (age > 18) {
    return true;
  } else {
    return confirm("Did parents allow you?");
  }
}
```

Rewrite it, to perform the same, but without if, in a single line.
Make two variants of checkAge:

1. Using a question mark operator ?
2. Using OR ||

### SOLUTION :

```javascript
// Using ?
function checkAge(age) {
  return age > 18 ? true : confirm("Did parents allow you?");
}

const result = checkAge(19);
console.log(result);

// Using ||
function checkAge(age) {
  return age > 18 || confirm("Did parents allow you?");
}

const result = checkAge(1);
console.log(result);
```

---

## Task 3 : Function min(a, b)

Write a function min(a,b) which returns the least of two numbers a and b. For instance:

```javascript
min(2, 5) == 2;
min(3, -1) == -1;
min(1, 1) == 1;
```

### SOLUTION :

```javascript
function min(a, b) {
  if (a < b) {
    return a;
  } else {
    return b;
  }
}

let result1 = min(2, 5);
let result2 = min(3, -1);
let result3 = min(1, 1);

console.log(result1);
console.log(result2);
console.log(result3);

// Ternary operator
function min(a, b) {
  return a < b ? a : b;
}
```

---

## Task 4 : Function pow(x,n)

Write a function pow(x,n) that returns x in power n. Or, in other words, multiplies x by itself n times and returns the result.

```javascript
pow(3, 2) = 3 * 3 = 9
pow(3, 3) = 3 * 3 * 3 = 27
pow(1, 100) = 1 * 1 * ...* 1 = 1
```

Create a web-page that prompts for x and n, and then shows the result of pow(x,n).

> P.S. In this task the function should support only natural values of n: integers up from 1.

### SOLUTION :

```javascript
function pow(x, n = 1) {
  let result = x;
  for (let i = 1; i < n; i++) {
    result *= x;
  }
  return result;
}

let result1 = pow(3, 2);
let result2 = pow(3, 3);
let result3 = pow(1, 100);

console.log(result1); // 9
console.log(result2); // 27
console.log(result3); // 1

// Using Math.pow()
function pow(x, n) {
  console.log(Math.pow(x, n));
}

pow(3, 2); // 9
pow(3, 3); // 27
pow(1, 100); // 1
```

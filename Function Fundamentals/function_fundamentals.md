# Function DEclaration / Function Statement 函式宣告 / 函式陳述式

```javascript
function sayHi() {
  console.log("Hello world");
}

sayHi(); // Hello world
```

## 特性

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

## Table of contents

# More Array methods 更多的陣列方法

## 新增及移除的方法：

- 之前有提過關於新增跟刪除陣列元素的方法，這邊簡單複習一下：
  - 新增元素到陣列的尾端
    - `push()`
  - 移除陣列尾端的元素
    - `pop()`
  - 新增元素到陣列的首部
    - `shift()`
  - 移除陣列首個元素
    - `unshift()`

## `Splice()` 多功能的陣列方法：

> **Note**
>
> 如果要移除陣列內隨便一個位置的元素，由於陣列是物件，用 `delete` 來移除似乎是個不錯的選擇，但若使用 `delete` 可以發現陣列的長度會維持不變，這是因為 `delete` 雖移除了 `key`，但依舊維持陣列的長度，理想上來說我們希望移除了某個元素，整個陣列的位置及長度有相應的改變。

```javascript
let arr = ["a", "b", 23];
delete arr[1]; // remove "b"
console.log(arr[1]); // undefined (not null!!)

console.log(arr.length); // 3
```

- `splice()` 的語法：`splice(start, delete count, add element)`
  - `0` : 從索引位置 0 開始刪除
  - `2` : 刪除 2 個元素
  - `"Good", "day"`: 增加 `"Good"` 和 `"day"`

```javascript
let arr = ["Hello", "my", "pretty", "world"];
arr.splice(0, 2, "Good", "day");

console.log(arr);
// [ 'Good', 'day', 'pretty', 'world' ]
```

- `splice()` 可以回傳被刪除的值

```javascript
let arr = ["Hello", "my", "pretty", "world"];
let removed = arr.splice(0, 2, "Good", "day");

console.log(arr);
// [ 'Good', 'day', 'pretty', 'world' ]

console.log(removed);
// [ 'Hello', 'my' ]
```

- 可以在不刪除的狀況下，新增元素
  - `1`: 從索引位置 1 之後開始
  - `0`: 不刪除任何元素
  - `"Good", "day"`: 加入這兩個元素

```javascript
let arr = ["Hello", "my", "pretty", "world"];
arr.splice(1, 0, "Good", "day");

console.log(arr);
// [ 'Hello', 'Good', 'day', 'my', 'pretty', 'world' ]
```

- 從後面加入元素也是可以的
  - `-1`: 索引位置 `-1`(最後一個元素 `world`)開始
  - `0`: 不刪除任何元素
  - `"PRETTY"`: 在 `world` 之前加入 `PRETTY`

```javascript
let arr = ["Hello", "my", "pretty", "world"];
arr.splice(-1, 0, "PRETTY");

console.log(arr);
// [ 'Hello', 'my', 'pretty', 'PRETTY', 'world' ]
```

---

## `slice()`

- 語法：`slice(start, end)`

  - 正常順序：包含 start 不包含 end

  ```javascript
  let arr = ["h", "e", "t", "b"];

  console.log(arr.slice(1, 3));

  // ["e", "t"]
  ```

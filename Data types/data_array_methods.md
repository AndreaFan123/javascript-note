## Table of contents

- [Array methods](#array-methods-陣列的相關用法)
- [performance](#performance-方法之間的效能差異)

# Array Methods 陣列的相關用法

- 佇列(Queue)：先進先出( First in, first out, aka FIFO)、橫向概念，像排公車，隊伍中的第一個，會先進公車，這裏假設進去公車後，隊伍就少一個，而位置將會依據往前移動。
  - `push`: 在尾端增加一個元素。
  - `shift`：取出第一個元素，其餘地往前移。

```javascript
// push: Add an element to the end
let arr = ["apple", "banana", "peach", "mango"];

arr.push("cherry");

console.log(arr);

// [ 'apple', 'banana', 'peach', 'mango', 'cherry' ]
```

```javascript
// shift: Remove first element
let arr = ["apple", "banana", "peach", "mango"];

arr.shift();

console.log(arr);
// [ 'banana', 'peach', 'mango' ]
```

> **Note** What is [queue](<https://en.wikipedia.org/wiki/Queue_(abstract_data_type)>)

- 堆疊(stack)：先進後出 (First in, last out, aka FILO)，縱向概念，像一堆書籍，最底層的書是最先放的，但是是最後才能取出的。
  - `push`: 在尾端增加一個元素。
  - `pop`: 從尾端取出一個元素。

```javascript
// pop: Remove the last element from the end.
let arr = ["apple", "banana", "peach", "mango"];

arr.pop();

console.log(arr);

// [ 'apple', 'banana', 'peach' ]
```

> **Note** What is [Stack](<https://en.wikipedia.org/wiki/Stack_(abstract_data_type)>)

> **Note** 在 JS，陣列是可以具有佇列(queue)和堆疊(stack)性質的資料類型，又稱[雙端佇列(dequeue: double-ended queue)](https://en.wikipedia.org/wiki/Double-ended_queue)

## Add / remove an element to the end of an array 新增或移除尾端的元素

- `push` : Add
- `pop`: Remove

## Add / remove an element to the beginning of an array 新增或移除第一個元素

- `unshift`: Add
- `shift`: Remove

# Performance 方法之間的效能差異

- 基本上 `push` 和 `pop` 效能比 `shift` 和 `unshift` 好，原因是因為，當我們把元素增加的尾端，整個陣列不需要因為增加而改變，但後者就會經歷以下階段：

1. 移除/新增索引值 0
2. 剩餘的值全往左邊/右邊移動後，重新賦予新的索引值
3. 更新陣列長度

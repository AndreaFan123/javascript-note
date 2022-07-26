## Table of contents

- [Array methods](#array-methods-陣列的相關用法)
- [performance](#performance-方法之間的效能差異)

# Data types that are mutable 可變的資料型態

在 JavaScript 的世界裡，只有 **物件** 和 **陣列** 是可變的，原始型別是不可變的。

## What is mutable and immutable? 什麼是可變與不可變？

可變的意思在這邊指得是一個物件或一個陣列的狀態可以在建立後被改變，反之，不可變則是建立後其狀態是不可變的。

## JavaScript data types

兩種資料型態，一種是原始型別(value type)；另一種是物件型別(reference type)

- 原始型別(Primitive value):
  - 數字 Numbers
  - 布林值 Boolean
  - 字串 String
  - Null
  - undefined
  - symbol
- 物件型別 (Object Type)
  - 陣列 Array
  - 物件 Object
  - 函式 Function
  - 日期 Date
  - 正則表達式 Regex(Regular Expression)

## Call by value vs. call by reference 傳值 vs. 傳參考

當原始型態在建立資料時，每一筆資料都將被電腦儲存在靜態記憶體配置(stack)，並擁有不同的記憶體位置，即使我們複製的資料，隨後更動了資料，都不影響各自的資料。

```javascript
let myName = "Andy";
let herName = myName;

console.log(myName); // Andy
console.log(herName); // Andy

herName = "Abby";

console.log(myName); // Andy
console.log(herName); // Abby
```

當物件型別被建立時，他們被儲存在動態記憶體配置(Heap)內，正確來說，物件還是會被建立在靜態記憶體，但物件內的值會在動態記憶體，可以想成物件型別的值都會被儲存在動態記憶體的參考位置，因此當更新其中的一個值，舊的值就會被新的值取代。

```javascript
let me = { name: "Andy" };
let anotherMe = me;

console.log(me); // {name: "Andy"}
console.log(anotherMe); // {name: "Andy"}

anotherMe.name = "Abby";

console.log(me); // {name: "Abby"}
console.log(anotherMe); // {name: "Abby"}
```

## Table of contents

# Garbage collection (垃圾回收)

1. JS 中的主要的隨機存取記憶體的概念為 **Reachability**

   > **Note**
   >
   > [Garbage collector](<https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)>) 來了解。

2. 何謂 Reachability ?

- 可訪問或取用的值，會暫存在記憶體內，例如：
  - 正在被執行的函式內的局部變數或參數。
  - 巢狀函式內的局部變數或參數。
  - 全域變數及一些其他內部的值。
  - 以上稱為 **Root**
- 若一個值可以透過 Root 取得其他值，就是 Reachable。

```javascript
let user = {
  name: "John",
};

// 全域變數 user 可以取得這個物件 {name: "John"}，此物件的屬性(name) 擁有一個原始型別的值 (John)
// user --> object
```

如果上述的 `user` 被重新指派一個新的值 `null`，原先的 `{name: "John"}` 就會被回收。

---

```javascript
let user1 = {
  name: "John",
};

let user2 = user1;
console.log(user1); // { name: 'John' }
console.log(user2); // { name: 'John' }

// user1 --
//        | --> {name : "John"}
// user2 --
```

此時，如果將 `user1` 指派給 `null`，但為了讓 `user2` 依舊可以取得物件值，`user1` 的物件會被回收，但 `user2` 的會被保留。

等等！！但物件不是 **Copy by reference** ? 照理說如果 `user1` 為 `null` 的話，不是表示 `{name : "John"}` 整個變成 `null`，那 `user2` 取得的不是應該是 `null` 嗎？

> **Important**
>
> 這邊的 `Copy by reference` 僅限於 `user1` 或 `user2` 取得屬性值後更動，才會影響。

```javascript
// 直接將 user1 改為 null
let user1 = {
  name: "John",
};

let user2 = user1;

user1 = null;
console.log(user1); // null
console.log(user2); // {name : "John"}
```

```javascript
// user1 改變屬性 name 的值
let user1 = {
  name: "John",
};

let user2 = user1;

user1.name = "Pete";
console.log(user1); // {name : "Pete"}
console.log(user2); // {name : "Pete"}
```

> 我的想法，應該是當複製了一個整體的物件給 `user2` 時，它也就擁有的取得該物件記憶體位置的權限，此權限不會因為 `user1` 改變而取消 `user2` 的權限，除非 `user2` 也被指派為 `null`。

---

# Interlinked objects (物件的連接)

1. 這裡的概念假設一個較為複雜的物件，如果只是單單刪除了其中一個子物件，物件彼此還是可以連接的，除非為 `null` 底下的子物件們才會被回收。

```javascript
function marry(man, woman) {
  woman.husband = man;
  man.wife = woman;

  return {
    father: man,
    mother: woman,
  };
}

let family = marry(
  {
    name: "John",
  },
  {
    name: "Ann",
  }
);

delete family.father;
delete family.mother.husband;

console.log(family); // { mother: { name: 'Ann' }}
```

上述刪除了子物件 `father ` 和 物件 `mother` 內的屬性 `husband`，這裡表示 `Object John` 目前是 `unreachable`，被回收的狀態。

如果僅刪除其中一個，我們依舊可以獲得資料，如下：

- Case 1: 僅刪除 `family.father`，還是可以取得 `husband`

```javascript
delete family.father;

console.log(family);
/*{
  mother:  {
    name: 'Ann',
    husband: { name: 'John', wife: {...} }
  }
}*/
```

- Case 2: 僅刪除 `family.mother.husband`，還是可以取得 `father` 的資料。

```javascript
delete family.mother.husband;

console.log(family);

/*
{
  father: { name: 'John', wife: { name: 'Ann' } },
  mother: { name: 'Ann' }
}
*/
```

結論：

> **Important**
>
> Outgoing references do not matter. Only incoming ones can make an object reachable.
>
> 傳出的 reference 不重要，只有傳入的 reference 才可以使物件具備可達性。

若只刪除其中一個，依然有傳入的 reference，只有當刪除全部傳入的 references 時，才能消除物件的可達性。

# Root is being null

根物件若為 `null`，即使子物件相互關聯，也無法構成可達性。

# Internal algorithm 內部演算法

可參考 [Tracing garbage collection](https://en.wikipedia.org/wiki/Tracing_garbage_collection) 和 [MDN-記憶體管理](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Memory_Management) 來了解更細節。

1. 垃圾回收的最基本演算法稱為：標記-清除 (mark-sweep)
2. 從根部開始做標記並存入記憶體，一路遍歷所有的 `references` 標記並存入記憶體。
3. 再遍歷所有的已標記的 `references` 底下的物件並標記其 `references` 直到沒有為止。
4. 倘若有一個完全單獨的物件，是無法與其相互連接，那個這個單獨的物件就會被回收掉。

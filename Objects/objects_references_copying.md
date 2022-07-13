## Table of contents

- [Objects vs. primitives](#objects-vs-primitives-物件-vs-原始型別)
- [References & copying](#references--copying-參考與複製)
- [Deep cloning vs. shadow cloning](#deep-cloning-vs-shadow-cloning-深拷貝-vs-淺拷貝)

# Objects vs. primitives (物件 vs. 原始型別)

1. 物件是傳址： (call by reference || pass by reference)。
2. 原始型別： (string, number, bigint ,boolean, null, undefined, symbol ) 是傳值 (call by value || pass by value)。

# References & copying (參考與複製 )

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

- 但如果我們透過 `b` 來改變屬性 `name` 的值，這邊可以很明顯地發現，`a` 的值也改變了，這是因為兩個物件都有可以取得屬性的參考位置，只要有一方改變屬性的值，兩方的值都會改變。

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

> **Note**
>
> 原始型別的複製就如同倉儲主管，為了要讓底下的員工都可以按照時間內進入工作地點，所以他必須要複製同樣的鑰匙給每一位員工。
>
> 但物件的複製就像倉儲主管對上其他部門主管，部門主管可以進入到他的辦公室取得鑰匙進入，所以如果部門主管萬一把其中一把鑰匙弄丟了，倉儲主管也就面臨同樣的狀況。
>
> 因此最好的狀況下，把物件透過 spread operator 複製一份，這樣就可以避免不小心修改到原本的物件。

# Deep cloning vs. shadow cloning (深拷貝 vs. 淺拷貝)

深拷貝就如同建立兩個獨立的物件，反之，淺拷貝如同只是複製出一個新的物件，但依舊是共享記憶體空間。

- 若物件屬性本身也是物件的話，即使我們僅把 `sizes` 取出來指派給 `clone`，依舊會維持著 copy by reference 的特性，換句話說，有一方修改屬性的話，物件屬性都會改變。

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

let clone = user;
console.log(clone === user); // true
```

- 那麽使用上述說的 `spread operator` 是不是就可以達到建立兩個獨立的物件呢？

> **Warning**
>
> 可以，但遇到這種巢狀的物件，基本上都還是淺拷貝(還是會影響屬性值)。

```javascript
et user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone ={...user};
clone.name = "Pete";
clone.sizes.width = 40; // 進入第二層物件修改 width 的值

console.log(user);
//{ name: 'John', sizes: { height: 182, width: 40 } }
console.log(clone);
{ name: 'Pete', sizes: { height: 182, width: 40 } }
```

- 如何真正地達到深拷貝呢？
  - 透過使用 `JSON.stringify(obj) & JSON.parse(string)`，確保產出一個新物件。
  - 可以看到當透過 `clone` 物件改變 `width` 時，原本的 `user` 物件屬性不變，由此可知深拷貝成功囉！

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

let clone = JSON.parse(JSON.stringify(user));

console.log(clone);
//{ name: 'John', sizes: { height: 182, width: 50 }}

clone.sizes.width = 40;
console.log(user);
//{ name: 'John', sizes: { height: 182, width: 50 }}

console.log(clone);
//{ name: 'John', sizes: { height: 182, width: 40 }}
```

- 透過 JS 的 `lodash` 內的 `_.cloneDeep(obj)` API 來進行深拷貝。

```javascript
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

let clone = _.cloneDeep(user);
```

> **Note**
>
> [Lodash](https://lodash.com/)
>
> [Lodash cloneDeep doc](https://lodash.com/docs/4.17.15#cloneDeep)

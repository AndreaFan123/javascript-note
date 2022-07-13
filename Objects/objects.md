## Table of contents

- [Objects](#objects-物件)
- [DotNotation](#dot-notation-點符號)
- [Square brackets](#square-brackets-中括號)
- [Properties shorthand](#properties-shorthand)
- [Test property existence](#test-property-existence-by-using-in)
- [Order in an object](#order-in-an-object)
- [Tasks](#tasks)

# Objects 物件：

透過 `{}` 建立物件，內部以 `鍵:值` `(key:value)` 形式呈現。

```javascript
let father = {
  name: "John Smith",
  age: 45,
};
```

- 物件 `father` 內有兩個屬性，一個為 `name`，一個為 `age`。

---

## Dot notation 點符號

1. 如果只有單層，可使用 `dot notation` (一個點) 或 `[]` 來取得屬性。

```javascript
let father = {
  name: "John Smith",
  age: 45,
};

console.log(father.name) === console.log(father["name"]);
console.log(father.age) === console.log(father["age"]);
```

2. 物件內的最後一個值需要加上逗點。
3. 如果屬性不只一個單詞，需要加上引號 `" "` 或 `' '`

```javascript
let father = {
  name: "John Smith",
  age: 45,
  "like dancing": true,
};
```

---

## Square brackets 中括號

1. 要取得如上述的 `like dacing` 多單詞、變數或更複雜的內容，不能使用 `dot notation`，要使用 `[]`。

```javascript
// 取得"like dancing"屬性
let father = {
  name: "John Sminth",
  age: 45,
  "like dancing": true,
};

const likeDancing = father["like dancing"];
console.log(likeDancing); // true
```

```javascript
// 變數應用
let fruit = prompt("Which fruit did you buy this morning?");

let father = {
  name: "John Smith",
  age: 45,
  [fruit]: 5,
};

console.log(father[fruit]); // 這邊的 fruit 會根據輸入的值來決定
```

> **Note**
>
> 如果屬性是簡單已知的狀況下，使用 `dot notation` 即可。

---

## Properties shorthand

1. 當我們想要建立多個角色時，除了上述那樣逐一建立外，透過函式可以快速達到建立多個物件。

```javascript
function createFamily(name, age) {
  return {
    name: name, // name,
    age: age, // age,
  };
}

let father = createFamily("John Smith", 45);
let mother = createFamily("Aria Smith", 40);
let sister = createFamily("Alison Smith", 15);

console.log(father); // { name: 'John Smith', age: 45 }
console.log(mother); // { name: 'Aria Smith', age: 40 }
console.log(sister); // { name: 'Alison Smith', age: 15 }
```

2. 如果屬性與值是相同的名稱，可以省略 `:name` 和 `:age`，以 `name` 和 `age` 表示即可。
3. 物件內，屬性名稱沒有特別規範既有的字不能當作屬性，如： `let`, `for` 或 `return`，如果屬性為數字，將會自動被轉換為字串，如：`0: "Faild" === "0": "faild"`

---

## Test property existence by using `in`

1. 如果想要知道一個物件的屬性存不存在，可以使用 `in` 來檢測。

```javascript
function createFamily(name, age) {
  return {
    name: name,
    age: age,
  };
}

let father = createFamily("John Smith", 45);
let mother = createFamily("Aria Smith", 40);
let sister = createFamily("Alison Smith", 15);

console.log("age" in father); // true
console.log("name" in mother); // true
console.log("food" in sister); // false
```

---

## Use `for...in` to find out all keys from an object

```javascript
let father = {
  name: "John Smith",
  age: 45,
  address: "2 State St,Ellsworth, Massachusetts",
  company: "Beta",
  "like dancing": true,
};

for (let props in father) {
  console.log(props);
}

// name
// age
// address
// company
// like dancing
```

---

## Order in an object

1. 如果是數字，則會以 `ascendant` 上升排序為主，其餘則依照物件內創造順序排序，例如電話號碼：

```javascript
// 數字
let codes = {
  49: "Germany",
  41: "Switzerland",
  44: "UK",
  1: "USA",
};

for (code in codea) {
  console.log(code);
}
// 1, 41, 44, 49

// 字串
let food = {
  fish: 3,
  alpple: 2,
  banana: 9,
};

for (f in food) {
  console.log(f);
}
// fish, apple, banana
```

> **Note**  
> `Interger property` (整數屬性)，是指當作為字串的數字，轉換成數字後再轉換回字串，依舊是相等的。例如：
>
> `"49"` 轉換成整數後再轉換成字串依舊是 `49`
>
> `"+49"` 轉換成整數為 `49` 再轉換成字串，就不等於原來的 `"+49"`
>
> `"1.2"` 轉換成整數為 `1` 再轉換成字串，不等於原來的 `"1.2"`

---

# Tasks

Tasks are from [javascript.info](https://javascript.info/object#tasks)

## Task 1: Hello Object

Write the code, one line for each action:

- Create an empty object `user`.
- Add the property `name` with the value `John`.
- Add the property `surname` with the value `Smith`.
- Change the value of the `name` to `Pete`.
- Remove the property `name` from the object.

### SOLUTION :

```javascript
let user = {};
user.name = "John";
user.surname = "Smith";
user.name = "Pete";
delete user.name;
```

---

## Task 2: Check for emptiness

Write the function `isEmpty(obj)` which returns `true` if the object has no properties, `false` otherwise. Should work like that:

```javascript
let schedule = {};

console.log(isEmpty(schedule)); // true

schedule["8:30"] = "get up";

console.log(isEmpty(schedule)); // false
```

### SOLUTION :

```javascript
function isEmpty(props) {
  for (let prop in props) {
    return `It's not an empty object`; // false
  }
  return `It's an empty object`; // true
}

let props1 = {};
let props2 = { name: "John" };
let result1 = isEmpty(props1);
let result2 = isEmpty(props2);
console.log(result1); // It's an empty object // false
console.log(result2); // It's not an empty object // true
```

> **Note**
>
> 這邊的函式名稱為 `isEmpty`:
>
> 若結果為 `false != isEmpty`。
>
> 若結果為 `true === isEmpty`

---

## Task 3: Sum object properties

We have an object storing salaries of our team:

```javascript
let salaries = {
  John: 100,
  Ann: 160,
  Pete: 130,
};
```

Write the code to sum all salaries and store in the variable `sum`. Should be `390` in the example above. If `salaries` is empty, then the result must be `0`.

### SOLUTION :

```javascript
let salaries = {
  John: 100,
  Ann: 160,
  Pete: 130,
};

// define sum as 0
let sum = 0;
for (let prop in salaries) {
  sum += salaries[prop];
}
console.log(sum); //390
```

---

## Task 4: Multiply numeric property values by 2

Create a function `multiplyNumeric(obj)` that multiplies all numeric property values of `obj` by `2`.

For instance:

```javascript
// before the call
let menu = {
  width: 200,
  height: 300,
  title: "My menu",
};

multiplyNumeric(menu);

// after the call
menu = {
  width: 400,
  height: 600,
  title: "My menu",
};
```

> Please note that multiplyNumeric does not need to return anything. It should modify the object in-place. P.S. Use typeof to check for a number here.

### SOLUTION :

```javascript
function multiplyNumeric(obj) {
  for (let prop in obj) {
    if (typeof obj[prop] === "number") {
      obj[prop] *= 2;
    }
  }
}

multiplyNumeric(menu);
console.log(menu);
```

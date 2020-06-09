# 函式的型別

> [函式是 JavaScript 中的一等公民](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/ch2.html)

## 函式宣告

在 JavaScript 中，有兩種常見的定義函式的方式——函式宣告（Function Declaration）和函式表示式（Function Expression）：

```javascript
// 函式宣告（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函式表示式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

一個函式有輸入和輸出，要在 TypeScript 中對其進行約束，需要把輸入和輸出都考慮到，其中函式宣告的型別定義較簡單：

```typescript
function sum(x: number, y: number): number {
    return x + y;
}
```

注意，**輸入多餘的（或者少於要求的）引數，是不被允許的**：

```typescript
function sum(x: number, y: number): number {
    return x + y;
}
sum(1, 2, 3);

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

```typescript
function sum(x: number, y: number): number {
    return x + y;
}
sum(1);

// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```

## 函式表示式

如果要我們現在寫一個對函式表示式（Function Expression）的定義，可能會寫成這樣：

```typescript
let mySum = function (x: number, y: number): number {
    return x + y;
};
```

這是可以透過編譯的，不過事實上，上面的程式碼只對等號右側的匿名函式進行了型別定義，而等號左邊的 `mySum`，是透過賦值操作進行型別推論而推斷出來的。如果需要我們手動給 `mySum` 新增型別，則應該是這樣：

```typescript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

注意不要混淆了 TypeScript 中的 `=>` 和 ES6 中的 `=>`。

在 TypeScript 的型別定義中，`=>` 用來表示函式的定義，左邊是輸入型別，需要用括號括起來，右邊是輸出型別。

在 ES6 中，`=>` 叫做箭頭函式，應用十分廣泛，可以參考 [ES6 中的箭頭函式](http://es6.ruanyifeng.com/#docs/function#箭頭函式)。

## 用介面定義函式的形狀

我們也可以使用介面的方式來定義一個函式需要符合的形狀：

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

## 可選引數

前面提到，輸入多餘的（或者少於要求的）引數，是不允許的。那麼如何定義可選的引數呢？

與介面中的可選屬性類似，我們用 `?` 表示可選的引數：

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

需要注意的是，可選引數必須接在必需引數後面。換句話說，**可選引數後面不允許再出現必需引數了**：

```typescript
function buildName(firstName?: string, lastName: string) {
    if (firstName) {
        return firstName + ' ' + lastName;
    } else {
        return lastName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName(undefined, 'Tom');

// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```

## 引數預設值

在 ES6 中，我們允許給函式的引數新增預設值，**TypeScript 會將添加了預設值的引數識別為可選引數**：

```typescript
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

此時就不受「可選引數必須接在必需引數後面」的限制了：

```typescript
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

> 關於預設引數，可以參考 [ES6 中函式引數的預設值](http://es6.ruanyifeng.com/#docs/function#函式引數的預設值)。

## 剩餘引數

ES6 中，可以使用 `...rest` 的方式獲取函式中的剩餘引數（rest 引數）：

```javascript
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

事實上，`items` 是一個數組。所以我們可以用陣列的型別來定義它：

```typescript
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

注意，rest 引數只能是最後一個引數，關於 rest 引數，可以參考 [ES6 中的 rest 引數](http://es6.ruanyifeng.com/#docs/function#rest引數)。

## 過載

過載允許一個函式接受不同數量或型別的引數時，作出不同的處理。

比如，我們需要實現一個函式 `reverse`，輸入數字 `123` 的時候，輸出反轉的數字 `321`，輸入字串 `'hello'` 的時候，輸出反轉的字串 `'olleh'`。

利用聯合型別，我們可以這麼實現：

```typescript
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

然而這樣有一個缺點，就是不能夠精確的表達，輸入為數字的時候，輸出也應該為數字，輸入為字串的時候，輸出也應該為字串。

這時，我們可以使用過載定義多個 `reverse` 的函式型別：

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

上例中，我們重複定義了多次函式 `reverse`，前幾次都是函式定義，最後一次是函式實現。在編輯器的程式碼提示中，可以正確的看到前兩個提示。

注意，TypeScript 會優先從最前面的函式定義開始匹配，所以多個函式定義如果有包含關係，需要優先把精確的定義寫在前面。

## 參考

* [Functions](http://www.typescriptlang.org/docs/handbook/functions.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Functions.html)）
* [Functions \# Function Types](http://www.typescriptlang.org/docs/handbook/interfaces.html#function-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Interfaces.html#函式型別)）
* [JS 函數語言程式設計指南](https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/)
* [ES6 中的箭頭函式](http://es6.ruanyifeng.com/#docs/function#箭頭函式)
* [ES6 中函式引數的預設值](http://es6.ruanyifeng.com/#docs/function#函式引數的預設值)
* [ES6 中的 rest 引數](http://es6.ruanyifeng.com/#docs/function#rest引數)
* [上一章：陣列的型別](type-of-array.md)
* [下一章：型別斷言](type-assertion.md)


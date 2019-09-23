# 內建物件

JavaScript 中有很多[內建物件][]，它們可以直接在 TypeScript 中當做定義好了的型別。

內建物件是指根據標準在全域性作用域（Global）上存在的物件。這裡的標準是指 ECMAScript 和其他環境（比如 DOM）的標準。

## ECMAScript 的內建物件

ECMAScript 標準提供的內建物件有：

`Boolean`、`Error`、`Date`、`RegExp` 等。

我們可以在 TypeScript 中將變數定義為這些型別：

```ts
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

更多的內建物件，可以檢視 [MDN 的文件][內建物件]。

而他們的定義檔案，則在 [TypeScript 核心函式庫的定義檔案][]中。

## DOM 和 BOM 的內建物件

DOM 和 BOM 提供的內建物件有：

`Document`、`HTMLElement`、`Event`、`NodeList` 等。

TypeScript 中會經常用到這些型別：

```ts
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

它們的定義檔案同樣在 [TypeScript 核心函式庫的定義檔案][]中。

## TypeScript 核心函式庫的定義檔案

[TypeScript 核心函式庫的定義檔案][]中定義了所有瀏覽器環境需要用到的型別，並且是預置在 TypeScript 中的。

當你在使用一些常用的方法的時候，TypeScript 實際上已經幫你做了很多型別判斷的工作了，比如：

```ts
Math.pow(10, '2');

// index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

上面的例子中，`Math.pow` 必須接受兩個 `number` 型別的引數。事實上 `Math.pow` 的型別定義如下：

```ts
interface Math {
    /**
     * Returns the value of a base expression taken to a specified power.
     * @param x The base value of the expression.
     * @param y The exponent value of the expression.
     */
    pow(x: number, y: number): number;
}
```

再舉一個 DOM 中的例子：

```ts
document.addEventListener('click', function(e) {
    console.log(e.targetCurrent);
});

// index.ts(2,17): error TS2339: Property 'targetCurrent' does not exist on type 'MouseEvent'.
```

上面的例子中，`addEventListener` 方法是在 TypeScript 核心函式庫中定義的：

```ts
interface Document extends Node, GlobalEventHandlers, NodeSelector, DocumentEvent {
    addEventListener(type: string, listener: (ev: MouseEvent) => any, useCapture?: boolean): void;
}
```

所以 `e` 被推斷成了 `MouseEvent`，而 `MouseEvent` 是沒有 `targetCurrent` 屬性的，所以報錯了。

注意，TypeScript 核心函式庫的定義中不包含 Node.js 部分。

## 用 TypeScript 寫 Node.js

Node.js 不是內建物件的一部分，如果想用 TypeScript 寫 Node.js，則需要引入第三方宣告檔案：

```bash
npm install @types/node --save-dev
```

## 參考

- [內建物件][]
- [TypeScript 核心函式庫的定義檔案][]

[內建物件]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects
[TypeScript 核心函式庫的定義檔案]: https://github.com/Microsoft/TypeScript/tree/master/src/lib

---

- [上一章：宣告檔案](declaration-files.md)
- [下一章：進階](../advanced/README.md)

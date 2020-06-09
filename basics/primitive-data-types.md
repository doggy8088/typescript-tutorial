# 原始資料型別

JavaScript 的型別分為兩種：原始資料型別（[Primitive data types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)）和物件型別（Object types）。

原始資料型別包括：布林值、數值、字串、`null`、`undefined` 以及 [ES6 中的新型別 `Symbol`](http://es6.ruanyifeng.com/#docs/symbol)。

本節主要介紹**前五種**原始資料型別在 TypeScript 中的應用。

## 布林值

布林值是最基礎的資料型別，在 TypeScript 中，使用 `boolean` 定義布林值型別：

```typescript
let isDone: boolean = false;

// 編譯透過
// 後面約定，未強調編譯錯誤的程式碼片段，預設為編譯透過
```

注意，使用建構函式 `Boolean` 創造的物件**不是**布林值：

```typescript
let createdByNewBoolean: boolean = new Boolean(1);

// Type 'Boolean' is not assignable to type 'boolean'.
//   'boolean' is a primitive, but 'Boolean' is a wrapper object. Prefer using 'boolean' when possible.
```

事實上 `new Boolean()` 返回的是一個 `Boolean` 物件：

```typescript
let createdByNewBoolean: Boolean = new Boolean(1);
```

直接呼叫 `Boolean` 也可以返回一個 `boolean` 型別：

```typescript
let createdByBoolean: boolean = Boolean(1);
```

在 TypeScript 中，`boolean` 是 JavaScript 中的基本型別，而 `Boolean` 是 JavaScript 中的建構函式。其他基本型別（除了 `null` 和 `undefined`）一樣，不再贅述。

## 數值

使用 `number` 定義數值型別：

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二進位制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八進位制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

編譯結果：

```javascript
var decLiteral = 6;
var hexLiteral = 0xf00d;
// ES6 中的二進位制表示法
var binaryLiteral = 10;
// ES6 中的八進位制表示法
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity;
```

其中 `0b1010` 和 `0o744` 是 [ES6 中的二進位制和八進位制表示法](http://es6.ruanyifeng.com/#docs/number#二進位制和八進位制表示法)，它們會被編譯為十進位制數字。

## 字串

使用 `string` 定義字串型別：

```typescript
let myName: string = 'Tom';
let myAge: number = 25;

// 範本字串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

編譯結果：

```javascript
var myName = 'Tom';
var myAge = 25;
// 範本字串
var sentence = "Hello, my name is " + myName + ".\nI'll be " + (myAge + 1) + " years old next month.";
```

其中 ````` 用來定義 [ES6 中的範本字串](http://es6.ruanyifeng.com/#docs/string#範本字串)，`${expr}` 用來在範本字串中嵌入表示式。

## 空值

JavaScript 沒有空值（Void）的概念，在 TypeScript 中，可以用 `void` 表示沒有任何返回值的函式：

```typescript
function alertName(): void {
    alert('My name is Tom');
}
```

宣告一個 `void` 型別的變數沒有什麼用，因為你只能將它賦值為 `undefined` 和 `null`：

```typescript
let unusable: void = undefined;
```

## Null 和 Undefined

在 TypeScript 中，可以使用 `null` 和 `undefined` 來定義這兩個原始資料型別：

```typescript
let u: undefined = undefined;
let n: null = null;
```

與 `void` 的區別是，`undefined` 和 `null` 是所有型別的子型別。也就是說 `undefined` 型別的變數，可以賦值給 `number` 型別的變數：

```typescript
// 這樣不會報錯
let num: number = undefined;
```

```typescript
// 這樣也不會報錯
let u: undefined;
let num: number = u;
```

而 `void` 型別的變數不能賦值給 `number` 型別的變數：

```typescript
let u: void;
let num: number = u;

// Type 'void' is not assignable to type 'number'.
```

## 參考

* [Basic Types](http://www.typescriptlang.org/docs/handbook/basic-types.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Basic%20Types.html)）
* [Primitive data types](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)
* [ES6 中的新型別 `Symbol`](http://es6.ruanyifeng.com/#docs/symbol)
* [ES6 中的二進位制和八進位制表示法](http://es6.ruanyifeng.com/#docs/number#二進位制和八進位制表示法)
* [ES6 中的範本字串](http://es6.ruanyifeng.com/#docs/string#範本字串)
* [上一章：基礎](./)
* [下一章：任意值](any.md)


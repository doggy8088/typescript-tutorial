# Hello TypeScript

我們從一個簡單的例子開始。

將以下程式碼複製到 `hello.ts` 中：

```typescript
function sayHello(person: string) {
    return 'Hello, ' + person;
}

let user = 'Tom';
console.log(sayHello(user));
```

然後執行

```bash
tsc hello.ts
```

這時候會產生一個編譯好的檔案 `hello.js`：

```javascript
function sayHello(person) {
    return 'Hello, ' + person;
}
var user = 'Tom';
console.log(sayHello(user));
```

TypeScript 中，使用 `:` 指定變數的型別，`:` 的前後有沒有空格都可以。

上述例子中，我們用 `:` 指定 `person` 引數型別為 `string`。但是編譯為 js 之後，並沒有什麼檢查的程式碼被插入進來。

**TypeScript 只會進行靜態檢查，如果發現有錯誤，編譯的時候就會報錯。**

> `let` 是 ES6 中的關鍵字，和 `var` 類似，用於定義一個區域性變數，可以參閱 [let 和 const 命令](http://es6.ruanyifeng.com/#docs/let)。

下面嘗試把這段程式碼編譯一下：

```typescript
function sayHello(person: string) {
    return 'Hello, ' + person;
}

let user = [0, 1, 2];
console.log(sayHello(user));
```

編輯器中會提示錯誤，編譯的時候也會出錯：

```bash
index.ts(6,22): error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

但是還是建立了 js 檔案：

```javascript
function sayHello(person) {
    return 'Hello, ' + person;
}
var user = [0, 1, 2];
console.log(sayHello(user));
```

**TypeScript 編譯的時候即使報錯了，還是會產生編譯結果**，我們仍然可以使用這個編譯之後的檔案。

如果要在報錯的時候終止 js 檔案的產生，可以在 `tsconfig.json` 中配置 `noEmitOnError` 即可。關於 `tsconfig.json`，請參閱[官方手冊](http://www.typescriptlang.org/docs/handbook/tsconfig-json.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/tsconfig.json.html)）。

* [上一章：安裝 TypeScript](get-typescript.md)
* [下一章：基礎](../basics/)


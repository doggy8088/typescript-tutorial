# 型別斷言

型別斷言（Type Assertion）可以用來手動指定一個值的型別。

## 語法

```ts
<型別>值
```

或

```ts
值 as 型別
```

在 tsx 語法（React 的 jsx 語法的 ts 版）中必須用後一種。

## 例子：將一個聯合型別的變數指定為一個更加具體的型別

[之前提到過](union-types.md#訪問聯合型別的屬性或方法)，當 TypeScript 不確定一個聯合型別的變數到底是哪個型別的時候，我們**只能訪問此聯合型別的所有型別裡共有的屬性或方法**：

```ts
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

而有時候，我們確實需要在還不確定型別的時候就訪問其中一個型別的屬性或方法，比如：

```ts
function getLength(something: string | number): number {
    if (something.length) {
        return something.length;
    } else {
        return something.toString().length;
    }
}

// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
// index.ts(3,26): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

上例中，獲取 `something.length` 的時候會報錯。

此時可以使用型別斷言，將 `something` 斷言成 `string`：

```ts
function getLength(something: string | number): number {
    if ((<string>something).length) {
        return (<string>something).length;
    } else {
        return something.toString().length;
    }
}
```

型別斷言的用法如上，在需要斷言的變數前加上 `<Type>` 即可。

**型別斷言不是型別轉換，斷言成一個聯合型別中不存在的型別是不允許的**：

```ts
function toBoolean(something: string | number): boolean {
    return <boolean>something;
}

// index.ts(2,10): error TS2352: Type 'string | number' cannot be converted to type 'boolean'.
//   Type 'number' is not comparable to type 'boolean'.
```

## 參考

- [TypeScript Deep Dive / Type Assertion](https://basarat.gitbooks.io/typescript/content/docs/types/type-assertion.html)
- [Advanced Types # Type Guards and Differentiating Types](http://www.typescriptlang.org/docs/handbook/advanced-types.html#type-guards-and-differentiating-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#型別保護與區分類型（type-guards-and-differentiating-types）)）

---

- [上一章：函式的型別](type-of-function.md)
- [下一章：宣告檔案](declaration-files.md)

# 聯合型別

聯合型別（Union Types）表示取值可以為多種型別中的一種。

## 簡單的例子

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = true;

// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
//   Type 'boolean' is not assignable to type 'number'.
```

聯合型別使用 `|` 分隔每個型別。

這裡的 `let myFavoriteNumber: string | number` 的含義是，允許 `myFavoriteNumber` 的型別是 `string` 或者 `number`，但是不能是其他型別。

## 訪問聯合型別的屬性或方法

當 TypeScript 不確定一個聯合型別的變數到底是哪個型別的時候，我們**只能訪問此聯合型別的所有型別裡共有的屬性或方法**：

```ts
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

上例中，`length` 不是 `string` 和 `number` 的共有屬性，所以會報錯。

訪問 `string` 和 `number` 的共有屬性是沒問題的：

```ts
function getString(something: string | number): string {
    return something.toString();
}
```

聯合型別的變數在被賦值的時候，會根據型別推論的規則推斷出一個型別：

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 編譯時報錯

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```

上例中，第二行的 `myFavoriteNumber` 被推斷成了 `string`，訪問它的 `length` 屬性不會報錯。

而第四行的 `myFavoriteNumber` 被推斷成了 `number`，訪問它的 `length` 屬性時就報錯了。

## 參考

- [Advanced Types # Union Types](http://www.typescriptlang.org/docs/handbook/advanced-types.html#union-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#聯合型別)）

---

- [上一章：型別推論](type-inference.md)
- [下一章：物件的型別——介面](type-of-object-interfaces.md)

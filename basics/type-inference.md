# 型別推論

如果沒有明確的指定型別，那麼 TypeScript 會依照型別推論（Type Inference）的規則推斷出一個型別。

## 什麼是型別推論

以下程式碼雖然沒有指定型別，但是會在編譯的時候報錯：

```ts
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

事實上，它等價於：

```ts
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

TypeScript 會在沒有明確的指定型別的時候推測出一個型別，這就是型別推論。

**如果定義的時候沒有賦值，不管之後有沒有賦值，都會被推斷成 `any` 型別而完全不被型別檢查**：

```ts
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

## 參考

- [Type Inference](http://www.typescriptlang.org/docs/handbook/type-inference.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Type%20Inference.html)）

---

- [上一章：任意值](any.md)
- [下一章：聯合型別](union-types.md)

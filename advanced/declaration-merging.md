# 宣告合併

如果定義了兩個相同名字的函式、介面或類別，那麼它們會合併成一個型別：

## 函式的合併

[之前學習過](../basics/type-of-function.md#過載)，我們可以使用過載定義多個函式型別：

```ts
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

## 介面的合併

介面中的屬性在合併時會簡單的合併到一個介面中：

```ts
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```

相當於：

```ts
interface Alarm {
    price: number;
    weight: number;
}
```

注意，**合併的屬性的型別必須是唯一的**：

```ts
interface Alarm {
    price: number;
}
interface Alarm {
    price: number;  // 雖然重複了，但是型別都是 `number`，所以不會報錯
    weight: number;
}
```

```ts
interface Alarm {
    price: number;
}
interface Alarm {
    price: string;  // 型別不一致，會報錯
    weight: number;
}

// index.ts(5,3): error TS2403: Subsequent variable declarations must have the same type.  Variable 'price' must be of type 'number', but here has type 'string'.
```

介面中方法的合併，與函式的合併一樣：

```ts
interface Alarm {
    price: number;
    alert(s: string): string;
}
interface Alarm {
    weight: number;
    alert(s: string, n: number): string;
}
```

相當於：

```ts
interface Alarm {
    price: number;
    weight: number;
    alert(s: string): string;
    alert(s: string, n: number): string;
}
```

## 類別的合併

類別的合併與介面的合併規則一致。

## 參考

- [Declaration Merging](http://www.typescriptlang.org/docs/handbook/declaration-merging.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Declaration%20Merging.html)）

---

- [上一章：泛型](generics.md)
- [下一章：擴充套件閱讀](further-reading.md)

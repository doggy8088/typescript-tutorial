# 元組

數組合並了相同型別的物件，而元組（Tuple）合併了不同型別的物件。

元組起源於函式程式語言（如 F#），這些語言中會頻繁使用元組。

## 簡單的例子

定義一對值分別為 `string` 和 `number` 的元組：

```ts
let tom: [string, number] = ['Tom', 25];
```

當賦值或訪問一個已知索引的元素時，會得到正確的型別：

```ts
let tom: [string, number];
tom[0] = 'Tom';
tom[1] = 25;

tom[0].slice(1);
tom[1].toFixed(2);
```

也可以只賦值其中一項：

```ts
let tom: [string, number];
tom[0] = 'Tom';
```

但是當直接對元組型別的變數進行初始化或者賦值的時候，需要提供所有元組型別中指定的項。

```ts
let tom: [string, number];
tom = ['Tom', 25];
```

```ts
let tom: [string, number];
tom = ['Tom'];

// Property '1' is missing in type '[string]' but required in type '[string, number]'.
```

## 越界的元素

當新增越界的元素時，它的型別會被限制為元組中每個型別的聯合型別：

```ts
let tom: [string, number];
tom = ['Tom', 25];
tom.push('male');
tom.push(true);

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

## 參考

- [Basic Types # Tuple](http://www.typescriptlang.org/docs/handbook/basic-types.html#tuple)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Basic%20Types.html#元組-tuple)）

---

- [上一章：字串字面量型別](string-literal-types.md)
- [下一章：列舉](enum.md)

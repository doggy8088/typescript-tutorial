# 型別別名

型別別名用來給一個型別起個新名字。

## 簡單的例子

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

上例中，我們使用 `type` 建立型別別名。

型別別名常用於聯合型別。

## 參考

* [Advanced Types \# Type Aliases](http://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#型別別名)）
* [上一章：進階](./)
* [下一章：字串字面量型別](string-literal-types.md)


# 陣列的型別

在 TypeScript 中，陣列型別有多種定義方式，比較靈活。

## 「型別 + 方括號」表示法

最簡單的方法是使用「型別 + 方括號」來表示陣列：

```typescript
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

陣列的項中**不允許**出現其他的型別：

```typescript
let fibonacci: number[] = [1, '1', 2, 3, 5];

// Type 'string' is not assignable to type 'number'.
```

陣列的一些方法的引數也會根據陣列在定義時約定的型別進行限制：

```typescript
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// Argument of type '"8"' is not assignable to parameter of type 'number'.
```

上例中，`push` 方法只允許傳入 `number` 型別的引數，但是卻傳了一個 `"8"` 型別的引數，所以報錯了。這裡 `"8"` 是一個字串字面量型別，會在後續章節中詳細介紹。

## 陣列泛型

我們也可以使用陣列泛型（Array Generic） `Array<elemType>` 來表示陣列：

```typescript
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

關於泛型，可以參考[泛型](../advanced/generics.md)一章。

## 用介面表示陣列

介面也可以用來描述陣列：

```typescript
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

`NumberArray` 表示：只要索引的型別是數字時，那麼值的型別必須是數字。

雖然介面也可以用來描述陣列，但是我們一般不會這麼做，因為這種方式比前兩種方式複雜多了。

不過有一種情況例外，那就是它常用來表示類別陣列。

## 類別陣列

類別陣列（Array-like Object）不是陣列型別，比如 `arguments`：

```typescript
function sum() {
    let args: number[] = arguments;
}

// Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
```

上例中，`arguments` 實際上是一個類別陣列，不能用普通的陣列的方式來描述，而應該用介面：

```typescript
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
```

在這個例子中，我們除了約束當索引的型別是數字時，值的型別必須是數字之外，也約束了它還有 `length` 和 `callee` 兩個屬性。

事實上常用的類別陣列都有自己的介面定義，如 `IArguments`, `NodeList`, `HTMLCollection` 等：

```typescript
function sum() {
    let args: IArguments = arguments;
}
```

其中 `IArguments` 是 TypeScript 中定義好了的型別，它實際上就是：

```typescript
interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}
```

關於內建物件，可以參考[內建物件](built-in-objects.md)一章。

## any 在陣列中的應用

一個比較常見的做法是，用 `any` 表示陣列中允許出現任意型別：

```typescript
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

## 參考

* [Basic Types \# Array](http://www.typescriptlang.org/docs/handbook/basic-types.html#array)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Basic%20Types.html#陣列)）
* [Interfaces \# Indexable Types](http://www.typescriptlang.org/docs/handbook/interfaces.html#indexable-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Interfaces.html#陣列型別)）
* [上一章：物件的型別——介面](type-of-object-interfaces.md)
* [下一章：函式的型別](type-of-function.md)


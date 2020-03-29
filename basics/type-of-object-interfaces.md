# 物件的型別——介面

在 TypeScript 中，我們使用介面（Interfaces）來定義物件的型別。

## 什麼是介面

在面嚮物件語言中，介面（Interfaces）是一個很重要的概念，它是對行為的抽象，而具體如何行動需要由類別（classes）去實現（implement）。

TypeScript 中的介面是一個非常靈活的概念，除了可用於[對類別的一部分行為進行抽象](../advanced/class-and-interfaces.md#類別實現介面)以外，也常用於對「物件的形狀（Shape）」進行描述。

## 簡單的例子

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

上面的例子中，我們定義了一個介面 `Person`，接著定義了一個變數 `tom`，它的型別是 `Person`。這樣，我們就約束了 `tom` 的形狀必須和介面 `Person` 一致。

介面一般首字母大寫。[有的程式語言中會建議介面的名稱加上 `I` 字首](https://msdn.microsoft.com/en-us/library/8bc1fexb%28v=vs.71%29.aspx)。

定義的變數比介面少了一些屬性是不允許的：

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom'
};

// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
//   Property 'age' is missing in type '{ name: string; }'.
```

多一些屬性也是不允許的：

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```

可見，**賦值的時候，變數的形狀必須和介面的形狀保持一致**。

## 可選屬性

有時我們希望不要完全匹配一個形狀，那麼可以用可選屬性：

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

可選屬性的含義是該屬性可以不存在。

這時**仍然不允許新增未定義的屬性**：

```ts
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// examples/playground/index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```

## 任意屬性

有時候我們希望一個介面允許有任意的屬性，可以使用如下方式：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

使用 `[propName: string]` 定義了任意屬性取 `string` 型別的值。

需要注意的是，**一旦定義了任意屬性，那麼確定屬性和可選屬性的型別都必須是它的型別的子集**：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

上例中，任意屬性的值允許是 `string`，但是可選屬性 `age` 的值卻是 `number`，`number` 不是 `string` 的子屬性，所以報錯了。

另外，在報錯資訊中可以看出，此時 `{ name: 'Tom', age: 25, gender: 'male' }` 的型別被推斷成了 `{ [x: string]: string | number; name: string; age: number; gender: string; }`，這是聯合型別和介面的結合。

## 只讀屬性

有時候我們希望物件中的一些欄位只能在建立的時候被賦值，那麼可以用 `readonly` 定義只讀屬性：

```ts
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;

// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

上例中，使用 `readonly` 定義的屬性 `id` 初始化後，又被賦值了，所以報錯了。

**注意，只讀的約束存在於第一次給物件賦值的時候，而不是第一次給只讀屬性賦值的時候**：

```ts
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};

tom.id = 89757;

// index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
//   Property 'id' is missing in type '{ name: string; gender: string; }'.
// index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

上例中，報錯資訊有兩處，第一處是在對 `tom` 進行賦值的時候，沒有給 `id` 賦值。

第二處是在給 `tom.id` 賦值的時候，由於它是只讀屬性，所以報錯了。

## 參考 

- [Interfaces](http://www.typescriptlang.org/docs/handbook/interfaces.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Interfaces.html)）

---

- [上一章：聯合型別](union-types.md)
- [下一章：陣列的型別](type-of-array.md)

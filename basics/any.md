# 任意值

任意值（Any）用來表示允許賦值為任意型別。

## 什麼是任意值型別

如果是一個普通型別，在賦值過程中改變型別是不被允許的：

```typescript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

但如果是 `any` 型別，則允許被賦值為任意型別。

```typescript
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```

## 任意值的屬性和方法

在任意值上訪問任何屬性都是允許的：

```typescript
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
```

也允許呼叫任何方法：

```typescript
let anyThing: any = 'Tom';
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```

可以認為，**宣告一個變數為任意值之後，對它的任何操作，返回的內容的型別都是任意值**。

## 未宣告型別的變數

**變數如果在宣告的時候，未指定其型別，那麼它會被識別為任意值型別**：

```typescript
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```

等價於

```typescript
let something: any;
something = 'seven';
something = 7;

something.setName('Tom');
```

## 參考

* [Basic Types \# Any](http://www.typescriptlang.org/docs/handbook/basic-types.html#any)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Basic%20Types.html#任意值)）
* [上一章：原始資料型別](primitive-data-types.md)
* [下一章：型別推論](type-inference.md)


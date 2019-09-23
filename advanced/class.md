# 類別

傳統方法中，JavaScript 透過建構函式實現類別的概念，透過原型鏈實現繼承。而在 ES6 中，我們終於迎來了 `class`。

TypeScript 除了實現了所有 ES6 中的類別的功能以外，還添加了一些新的用法。

這一節主要介紹類別的用法，下一節再介紹如何定義類別的型別。

## 類別的概念

雖然 JavaScript 中有類別的概念，但是可能大多數 JavaScript 程式設計師並不是非常熟悉類別，這裡對類別相關的概念做一個簡單的介紹。

- 類別(Class)：定義了一件事物的抽象特點，包含它的屬性和方法
- 物件（Object）：類別的實例，透過 `new` 產生
- 面向物件（OOP）的三大特性：封裝、繼承、多型
- 封裝（Encapsulation）：將對資料的操作細節隱藏起來，只暴露對外的介面。外界呼叫端不需要（也不可能）知道細節，就能透過對外提供的介面來訪問該物件，同時也保證了外界無法任意更改物件內部的資料
- 繼承（Inheritance）：子類別繼承父類別，子類別除了擁有父類別的所有特性外，還有一些更具體的特性
- 多型（Polymorphism）：由繼承而產生了相關的不同的類別，對同一個方法可以有不同的響應。比如 `Cat` 和 `Dog` 都繼承自 `Animal`，但是分別實現了自己的 `eat` 方法。此時針對某一個實例，我們無需瞭解它是 `Cat` 還是 `Dog`，就可以直接呼叫 `eat` 方法，程式會自動判斷出來應該如何執行 `eat`
- 存取器（getter & setter）：用以改變屬性的讀取和賦值行為
- 修飾符（Modifiers）：修飾符是一些關鍵字，用於限定成員或型別的性質。比如 `public` 表示公有屬性或方法
- 抽象類別（Abstract Class）：抽象類別是供其他類別繼承的基底類別，抽象類別不允許被實例化。抽象類別中的抽象方法必須在子類別中被實現
- 介面（Interfaces）：不同類別之間公有的屬性或方法，可以抽象成一個介面。介面可以被類別實現（implements）。一個類別只能繼承自另一個類別，但是可以實現多個介面

## ES6 中類別的用法

下面我們先回顧一下 ES6 中類別的用法，更詳細的介紹可以參考 [ECMAScript 6 入門 - Class]。

### 屬性和方法

使用 `class` 定義類別，使用 `constructor` 定義建構函式。

透過 `new` 產生新實例的時候，會自動呼叫建構函式。

```js
class Animal {
    constructor(name) {
        this.name = name;
    }
    sayHi() {
        return `My name is ${this.name}`;
    }
}

let a = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

### 類別的繼承

使用 `extends` 關鍵字實現繼承，子類別中使用 `super` 關鍵字來呼叫父類別的建構函式和方法。

```js
class Cat extends Animal {
    constructor(name) {
        super(name); // 呼叫父類別的 constructor(name)
        console.log(this.name);
    }
    sayHi() {
        return 'Meow, ' + super.sayHi(); // 呼叫父類別的 sayHi()
    }
}

let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

### 存取器

使用 getter 和 setter 可以改變屬性的賦值和讀取行為：

```js
class Animal {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return 'Jack';
    }
    set name(value) {
        console.log('setter: ' + value);
    }
}

let a = new Animal('Kitty'); // setter: Kitty
a.name = 'Tom'; // setter: Tom
console.log(a.name); // Jack
```

### 靜態方法

使用 `static` 修飾符修飾的方法稱為靜態方法，它們不需要實例化，而是直接透過類別來呼叫：

```js
class Animal {
    static isAnimal(a) {
        return a instanceof Animal;
    }
}

let a = new Animal('Jack');
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

## ES7 中類別的用法

ES7 中有一些關於類別的提案，TypeScript 也實現了它們，這裡做一個簡單的介紹。

### 實例屬性

ES6 中實例的屬性只能透過建構函式中的 `this.xxx` 來定義，ES7 提案中可以直接在類別裡面定義：

```js
class Animal {
    name = 'Jack';

    constructor() {
        // ...
    }
}

let a = new Animal();
console.log(a.name); // Jack
```

### 靜態屬性

ES7 提案中，可以使用 `static` 定義一個靜態屬性：

```js
class Animal {
    static num = 42;

    constructor() {
        // ...
    }
}

console.log(Animal.num); // 42
```

## TypeScript 中類別的用法

### public private 和 protected

TypeScript 可以使用三種訪問修飾符（Access Modifiers），分別是 `public`、`private` 和 `protected`。

- `public` 修飾的屬性或方法是公有的，可以在任何地方被訪問到，預設所有的屬性和方法都是 `public` 的
- `private` 修飾的屬性或方法是私有的，不能在宣告它的類別的外部訪問
- `protected` 修飾的屬性或方法是受保護的，它和 `private` 類似，區別是它在子類別中也是允許被訪問的

下面舉一些例子：

```ts
class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';
console.log(a.name); // Tom
```

上面的例子中，`name` 被設定為了 `public`，所以直接訪問實例的 `name` 屬性是允許的。

很多時候，我們希望有的屬性是無法直接存取的，這時候就可以用 `private` 了：

```ts
class Animal {
    private name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';

// index.ts(9,13): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
// index.ts(10,1): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

需要注意的是，TypeScript 編譯之後的程式碼中，並沒有限制 `private` 屬性在外部的可及性。

上面的例子編譯後的程式碼是：

```js
var Animal = (function () {
    function Animal(name) {
        this.name = name;
    }
    return Animal;
}());
var a = new Animal('Jack');
console.log(a.name);
a.name = 'Tom';
```

使用 `private` 修飾的屬性或方法，在子類別中也是不允許訪問的：

```ts
class Animal {
    private name;
    public constructor(name) {
        this.name = name;
    }
}

class Cat extends Animal {
    constructor(name) {
        super(name);
        console.log(this.name);
    }
}

// index.ts(11,17): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

而如果是用 `protected` 修飾，則允許在子類別中訪問：

```ts
class Animal {
    protected name;
    public constructor(name) {
        this.name = name;
    }
}

class Cat extends Animal {
    constructor(name) {
        super(name);
        console.log(this.name);
    }
}
```

當建構函式修飾為 `private` 時，該類別不允許被繼承或者實例化：

```ts
class Animal {
    public name;
    private constructor (name) {
        this.name = name;
  }
}
class Cat extends Animal {
    constructor (name) {
        super(name);
    }
}

let a = new Animal('Jack');

// index.ts(7,19): TS2675: Cannot extend a class 'Animal'. Class constructor is marked as private.
// index.ts(13,9): TS2673: Constructor of class 'Animal' is private and only accessible within the class declaration.
```

當建構函式修飾為 `protected` 時，該類別只允許被繼承：

```ts
class Animal {
    public name;
    protected constructor (name) {
        this.name = name;
  }
}
class Cat extends Animal {
    constructor (name) {
        super(name);
    }
}

let a = new Animal('Jack');

// index.ts(13,9): TS2674: Constructor of class 'Animal' is protected and only accessible within the class declaration.
```

修飾符還可以使用在建構函式引數中，等同於類別中定義該屬性，使程式碼更簡潔。

```ts
class Animal {
    // public name: string;
    public constructor (public name) {
        this.name = name;
    }
}
```

### readonly

只讀屬性關鍵字，只允許出現在屬性宣告或索引簽名中。

```ts
class Animal {
    readonly name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';

// index.ts(10,3): TS2540: Cannot assign to 'name' because it is a read-only property.
```

注意如果 `readonly` 和其他訪問修飾符同時存在的話，需要寫在其後面。

```ts
class Animal {
    // public readonly name;
    public constructor(public readonly name) {
        this.name = name;
    }
}
```

### 抽象類別

`abstract` 用於定義抽象類別和其中的抽象方法。

什麼是抽象類別？

首先，抽象類別是不允許被實例化的：

```ts
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
    public abstract sayHi();
}

let a = new Animal('Jack');

// index.ts(9,11): error TS2511: Cannot create an instance of the abstract class 'Animal'.
```

上面的例子中，我們定義了一個抽象類別 `Animal`，並且定義了一個抽象方法 `sayHi`。在實例化抽象類別的時候報錯了。

其次，抽象類別中的抽象方法必須被子類別實現：

```ts
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
    public abstract sayHi();
}

class Cat extends Animal {
    public eat() {
        console.log(`${this.name} is eating.`);
    }
}

let cat = new Cat('Tom');

// index.ts(9,7): error TS2515: Non-abstract class 'Cat' does not implement inherited abstract member 'sayHi' from class 'Animal'.
```

上面的例子中，我們定義了一個類別 `Cat` 繼承了抽象類別 `Animal`，但是沒有實現抽象方法 `sayHi`，所以編譯報錯了。

下面是一個正確使用抽象類別的例子：

```ts
abstract class Animal {
    public name;
    public constructor(name) {
        this.name = name;
    }
    public abstract sayHi();
}

class Cat extends Animal {
    public sayHi() {
        console.log(`Meow, My name is ${this.name}`);
    }
}

let cat = new Cat('Tom');
```

上面的例子中，我們實現了抽象方法 `sayHi`，編譯通過了。

需要注意的是，即使是抽象方法，TypeScript 的編譯結果中，仍然會存在這個類別，上面的程式碼的編譯結果是：

```js
var __extends = (this && this.__extends) || function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() { this.constructor = d; }
    d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
};
var Animal = (function () {
    function Animal(name) {
        this.name = name;
    }
    return Animal;
}());
var Cat = (function (_super) {
    __extends(Cat, _super);
    function Cat() {
        _super.apply(this, arguments);
    }
    Cat.prototype.sayHi = function () {
        console.log('Meow, My name is ' + this.name);
    };
    return Cat;
}(Animal));
var cat = new Cat('Tom');
```

## 類別的型別

給類別加上 TypeScript 的型別很簡單，與介面類似：

```ts
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    sayHi(): string {
      return `My name is ${this.name}`;
    }
}

let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

## 參考

- [Classes](http://www.typescriptlang.org/docs/handbook/classes.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Classes.html)）
- [ECMAScript 6 入門 - Class]

[ECMAScript 6 入門 - Class]: http://es6.ruanyifeng.com/#docs/class

---

- [上一章：列舉](enum.md)
- [下一章：類別與介面](class-and-interfaces.md)

# 類別與介面

[之前學習過](../basics/type-of-object-interfaces.md)，介面（Interfaces）可以用於對「物件的形狀（Shape）」進行描述。

這一章主要介紹介面的另一個用途，對類別的一部分行為進行抽象。

## 類別實現介面

實現（implements）是面向物件中的一個重要概念。一般來講，一個類別只能繼承自另一個類別，有時候不同類別之間可以有一些共有的特性，這時候就可以把特性提取成介面（interfaces），用 `implements` 關鍵字來實現。這個特性大大提高了面向物件的靈活性。

舉例來說，門是一個類別，防盜門是門的子類別。如果防盜門有一個報警器的功能，我們可以簡單的給防盜門新增一個報警方法。這時候如果有另一個類別，車，也有報警器的功能，就可以考慮把報警器提取出來，作為一個介面，防盜門和車都去實現它：

```ts
interface Alarm {
    alert();
}

class Door {
}

class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}

class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
}
```

一個類別可以實現多個介面：

```ts
interface Alarm {
    alert();
}

interface Light {
    lightOn();
    lightOff();
}

class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

上例中，`Car` 實現了 `Alarm` 和 `Light` 介面，既能報警，也能開關車燈。

## 介面繼承介面

介面與介面之間可以是繼承關係：

```ts
interface Alarm {
    alert();
}

interface LightableAlarm extends Alarm {
    lightOn();
    lightOff();
}
```

上例中，我們使用 `extends` 使 `LightableAlarm` 繼承 `Alarm`。

## 介面繼承類別

介面也可以繼承類別：

```ts
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

## 混合型別

[之前學習過](../basics/type-of-function.md#介面中函式的定義)，可以使用介面的方式來定義一個函式需要符合的形狀：

```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

有時候，一個函式還可以有自己的屬性和方法：

```ts
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

## 參考

- [Interfaces](http://www.typescriptlang.org/docs/handbook/interfaces.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Interfaces.html)）

---

- [上一章：類別](class.md)
- [下一章：泛型](generics.md)

# 列舉

列舉（Enum）型別用於取值被限定在一定範圍內的場景，比如一週只能有七天，顏色限定為紅綠藍等。

## 簡單的例子

列舉使用 `enum` 關鍵字來定義：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

列舉成員會被賦值為從 `0` 開始遞增的數字，同時也會對列舉值到列舉名進行反向對映：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

事實上，上面的例子會被編譯為：

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

## 手動賦值

我們也可以給列舉項手動賦值：

```ts
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

上面的例子中，未手動賦值的列舉項會接著上一個列舉項遞增。

如果未手動賦值的列舉項與手動賦值的重複了，TypeScript 是不會察覺到這一點的：

```ts
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 3); // true
console.log(Days["Wed"] === 3); // true
console.log(Days[3] === "Sun"); // false
console.log(Days[3] === "Wed"); // true
```

上面的例子中，遞增到 `3` 的時候與前面的 `Sun` 的取值重複了，但是 TypeScript 並沒有報錯，導致 `Days[3]` 的值先是 `"Sun"`，而後又被 `"Wed"` 覆蓋了。編譯的結果是：

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 3] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

所以使用的時候需要注意，最好不要出現這種覆蓋的情況。

手動賦值的列舉項可以不是數字，此時需要使用型別斷言來讓 tsc 無視型別檢查 (編譯出的 js 仍然是可用的)：

```ts
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 7] = "Sun";
    Days[Days["Mon"] = 8] = "Mon";
    Days[Days["Tue"] = 9] = "Tue";
    Days[Days["Wed"] = 10] = "Wed";
    Days[Days["Thu"] = 11] = "Thu";
    Days[Days["Fri"] = 12] = "Fri";
    Days[Days["Sat"] = "S"] = "Sat";
})(Days || (Days = {}));
```

當然，手動賦值的列舉項也可以為小數或負數，此時後續未手動賦值的項的遞增步長仍為 `1`：

```ts
enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true
```

## 常數項和計算所得項

列舉項有兩種型別：常數項（constant member）和計算所得項（computed member）。

前面我們所舉的例子都是常數項，一個典型的計算所得項的例子：

```ts
enum Color {Red, Green, Blue = "blue".length};
```

上面的例子中，`"blue".length` 就是一個計算所得項。

上面的例子不會報錯，但是**如果緊接在計算所得項後面的是未手動賦值的項，那麼它就會因為無法獲得初始值而報錯**：

```ts
enum Color {Red = "red".length, Green, Blue};

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

下面是常數項和計算所得項的完整定義，部分參考自[中文手冊 - 列舉]：

當滿足以下條件時，列舉成員被當作是常數：

- 不具有初始化函式並且之前的列舉成員是常數。在這種情況下，當前列舉成員的值為上一個列舉成員的值加 `1`。但第一個列舉元素是個例外。如果它沒有初始化方法，那麼它的初始值為 `0`。
- 列舉成員使用常數列舉表示式初始化。常數列舉表示式是 TypeScript 表示式的子集，它可以在編譯階段求值。當一個表示式滿足下面條件之一時，它就是一個常數列舉表示式：
  - 數字字面量
  - 參考之前定義的常數列舉成員（可以是在不同的列舉型別中定義的）如果這個成員是在同一個列舉型別中定義的，可以使用非限定名來參考
  - 帶括號的常數列舉表示式
  - `+`, `-`, `~` 一元運算子應用於常數列舉表示式
  - `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^` 二元運算子，常數列舉表示式做為其一個操作物件。若常數列舉表示式求值後為 NaN 或 Infinity，則會在編譯階段報錯

所有其它情況的列舉成員被當作是需要計算得出的值。

## 常數列舉

常數列舉是使用 `const enum` 定義的列舉型別：

```ts
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

常數列舉與普通列舉的區別是，它會在編譯階段被刪除，並且不能包含計算成員。

上例的編譯結果是：

```js
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

假如包含了計算成員，則會在編譯階段報錯：

```ts
const enum Color {Red, Green, Blue = "blue".length};

// index.ts(1,38): error TS2474: In 'const' enum declarations member initializer must be constant expression.
```

## 外部列舉

外部列舉（Ambient Enums）是使用 `declare enum` 定義的列舉型別：

```ts
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

之前提到過，`declare` 定義的型別只會用於編譯時的檢查，編譯結果中會被刪除。

上例的編譯結果是：

```js
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

外部列舉與宣告語句一樣，常出現在宣告檔案中。

同時使用 `declare` 和 `const` 也是可以的：

```ts
declare const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

編譯結果：

```js
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

> TypeScript 的列舉型別的概念[來源於 C#][C# Enum]。

## 參考

- [Enums](http://www.typescriptlang.org/docs/handbook/enums.html)（[中文版][中文手冊 - 列舉]）
- [C# Enum][]

[中文手冊 - 列舉]: https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Enums.html
[C# Enum]: https://msdn.microsoft.com/zh-cn/library/sbbt4032.aspx

---

- [上一章：元組](tuple.md)
- [下一章：類別](class.md)

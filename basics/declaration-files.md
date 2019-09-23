# 宣告檔案

當使用第三方函式庫時，我們需要參考它的宣告檔案，才能獲得對應的程式碼自動完成、介面提示等功能。

## 新語法索引

由於本章涉及大量新語法，故在本章開頭列出新語法的索引，方便大家在使用這些新語法時能快速查詢到對應的講解：

- [`declare var`](#declare-var) 宣告全域性變數
- [`declare function`](#declare-function) 宣告全域性方法
- [`declare class`](#declare-class) 宣告全域性類別
- [`declare enum`](#declare-enum) 宣告全域性列舉型別
- [`declare namespace`](#declare-namespace) 宣告（含有子屬性的）全域性物件
- [`interface` 和 `type`](#interface-he-type) 宣告全域性型別
- [`export`](#export) 匯出變數
- [`export namespace`](#export-namespace) 匯出（含有子屬性的）物件
- [`export default`](#export-default) ES6 預設匯出
- [`export =`](#export-1) commonjs 匯出模組
- [`export as namespace`](#export-as-namespace) UMD 函式庫宣告全域性變數
- [`declare global`](#declare-global) 擴充套件全域性變數
- [`declare module`](#declare-module) 擴充套件模組
- [`/// <reference />`](#san-xie-xian-zhi-ling) 三斜線指令

## 什麼是宣告語句

假如我們想使用第三方函式庫 jQuery，一種常見的方式是在 html 中透過 `<script>` 標籤引入 jQuery，然後就可以使用全域性變數 `$` 或 `jQuery` 了。

我們通常這樣獲取一個 `id` 是 `foo` 的元素：

```js
$('#foo');
// or
jQuery('#foo');
```

但是在 ts 中，編譯器並不知道 `$` 或 `jQuery` 是什麼東西[<sup>1</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/01-jquery)：

```ts
jQuery('#foo');
// ERROR: Cannot find name 'jQuery'.
```

這時，我們需要使用 `declare var` 來定義它的型別[<sup>2</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/02-declare-var)：

```ts
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

上例中，`declare var` 並沒有真的定義一個變數，只是定義了全域性變數 `jQuery` 的型別，僅僅會用於編譯時的檢查，在編譯結果中會被刪除。它編譯結果是：

```js
jQuery('#foo');
```

除了 `declare var` 之外，還有其他很多種宣告語句，將會在後面詳細介紹。

## 什麼是宣告檔案

通常我們會把宣告語句放到一個單獨的檔案（`jQuery.d.ts`）中，這就是宣告檔案[<sup>3</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/03-jquery-d-ts)：

```ts
// src/jQuery.d.ts

declare var jQuery: (selector: string) => any;
```

```ts
// src/index.ts

jQuery('#foo');
```

宣告檔案必需以 `.d.ts` 為字尾。

一般來說，ts 會解析專案中所有的 `*.ts` 檔案，當然也包含以 `.d.ts` 結尾的檔案。所以當我們將 `jQuery.d.ts` 放到專案中時，其他所有 `*.ts` 檔案就都可以獲得 `jQuery` 的型別定義了。

```plain
/path/to/project
├── src
|  ├── index.ts
|  └── jQuery.d.ts
└── tsconfig.json
```

假如仍然無法解析，那麼可以檢查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，確保其包含了 `jQuery.d.ts` 檔案。

這裡只演示了全域性變數這種模式的宣告檔案，假如是透過模組匯入的方式使用第三方函式庫的話，那麼引入宣告檔案又是另一種方式了，將會在後面詳細介紹。

### 第三方宣告檔案

當然，jQuery 的宣告檔案不需要我們定義了，社群已經幫我們定義好了：[jQuery in DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/jquery/index.d.ts)。

我們可以直接下載下來使用，但是更推薦的是使用 `@types` 統一管理第三方函式庫的宣告檔案。

`@types` 的使用方式很簡單，直接用 npm 安裝對應的宣告模組即可，以 jQuery 舉例：

```bash
npm install @types/jquery --save-dev
```

可以在[這個頁面](https://microsoft.github.io/TypeSearch/)搜尋你需要的宣告檔案。

## 書寫宣告檔案

當一個第三方函式庫沒有提供宣告檔案時，我們就需要自己書寫宣告檔案了。前面只介紹了最簡單的宣告檔案內容，而真正書寫一個宣告檔案並不是一件簡單的事，以下會詳細介紹如何書寫宣告檔案。

在不同的場景下，宣告檔案的內容和使用方式會有所區別。

函式庫的使用場景主要有以下幾種：

- [全域性變數](#quan-ju-bian-liang)：透過 `<script>` 標籤引入第三方函式庫，注入全域性變數
- [npm 包](#npm-bao)：透過 `import foo from 'foo'` 匯入，符合 ES6 模組規範
- [UMD 函式庫](#umd-ku)：既可以透過 `<script>` 標籤引入，又可以透過 `import` 匯入
- [直接擴充套件全域性變數](#zhi-jie-kuo-zhan-quan-ju-bian-liang)：透過 `<script>` 標籤引入後，改變一個全域性變數的結構
- [在 npm 包或 UMD 函式庫中擴充套件全域性變數](#zai-npm-bao-huo-umd-ku-zhong-kuo-zhan-quan-ju-bian-liang)：參考 npm 包或 UMD 函式庫後，改變一個全域性變數的結構
- [模組外掛](#mo-kuai-cha-jian)：透過 `<script>` 或 `import` 匯入後，改變另一個模組的結構

### 全域性變數

全域性變數是最簡單的一種場景，之前舉的例子就是透過 `<script>` 標籤引入 jQuery，注入全域性變數 `$` 和 `jQuery`。

使用全域性變數的宣告檔案時，如果是以 `npm install @types/xxx --save-dev` 安裝的，則不需要任何配置。如果是將宣告檔案直接存放於當前專案中，則建議和其他原始碼一起放到 `src` 目錄下（或者對應的原始碼目錄下）：

```plain
/path/to/project
├── src
|  ├── index.ts
|  └── jQuery.d.ts
└── tsconfig.json
```

如果沒有生效，可以檢查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，確保其包含了 `jQuery.d.ts` 檔案。

全域性變數的宣告檔案主要有以下幾種語法：

- [`declare var`](#declare-var) 宣告全域性變數
- [`declare function`](#declare-function) 宣告全域性方法
- [`declare class`](#declare-class) 宣告全域性類別
- [`declare enum`](#declare-enum) 宣告全域性列舉型別
- [`declare namespace`](#declare-namespace) 宣告（含有子屬性的）全域性物件
- [`interface` 和 `type`](#interface-he-type) 宣告全域性型別

#### `declare var`

在所有的宣告語句中，`declare var` 是最簡單的，如之前所學，它能夠用來定義一個全域性變數的型別。與其類似的，還有 `declare let` 和 `declare const`，使用 `let` 與使用 `var` 沒有什麼區別：

```ts
// src/jQuery.d.ts

declare let jQuery: (selector: string) => any;
```

```ts
// src/index.ts

jQuery('#foo');
// 使用 declare let 定義的 jQuery 型別，允許修改這個全域性變數
jQuery = function(selector) {
    return document.querySelector(selector);
};
```

而當我們使用 `const` 定義時，表示此時的全域性變數是一個常量，不允許再去修改它的值了[<sup>4</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/04-declare-const-jquery)：

```ts
// src/jQuery.d.ts

declare const jQuery: (selector: string) => any;

jQuery('#foo');
// 使用 declare const 定義的 jQuery 型別，禁止修改這個全域性變數
jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: Cannot assign to 'jQuery' because it is a constant or a read-only property.
```

一般來說，全域性變數都是禁止修改的常量，所以大部分情況都應該使用 `const` 而不是 `var` 或 `let`。

需要注意的是，宣告語句中只能定義型別，切勿在宣告語句中定義具體的實現[<sup>5</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/05-declare-jquery-value)：

```ts
declare const jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: An implementation cannot be declared in ambient contexts.
```

#### `declare function`

`declare function` 用來定義全域性函式的型別。jQuery 其實就是一個函式，所以也可以用 `function` 來定義：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
```

```ts
// src/index.ts

jQuery('#foo');
```

在函式型別的宣告語句中，函式過載也是支援的[<sup>6</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/06-declare-function)：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare function jQuery(domReadyCallback: () => any): any;
```

```ts
// src/index.ts

jQuery('#foo');
jQuery(function() {
    alert('Dom Ready!');
});
```

#### `declare class`

當全域性變數是一個類別的時候，我們用 `declare class` 來定義它的型別[<sup>7</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/07-declare-class)：

```ts
// src/Animal.d.ts

declare class Animal {
    name: string;
    constructor(name: string);
    sayHi(): string;
}
```

```ts
// src/index.ts

let cat = new Animal('Tom');
```

同樣的，`declare class` 語句也只能用來定義型別，不能用來定義具體的實現，比如定義 `sayHi` 方法的具體實現則會報錯：

```ts
// src/Animal.d.ts

declare class Animal {
    name: string;
    constructor(name: string);
    sayHi() {
        return `My name is ${this.name}`;
    };
    // ERROR: An implementation cannot be declared in ambient contexts.
}
```

#### `declare enum`

使用 `declare enum` 定義的列舉型別也稱作外部列舉（Ambient Enums），舉例如下[<sup>8</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/08-declare-enum)：

```ts
// src/Directions.d.ts

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
```

```ts
// src/index.ts

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

與其他全域性變數的型別宣告一致，`declare enum` 僅用來定義型別，而不是具體的值。

`Directions.d.ts` 僅僅會用於編譯時的檢查，宣告檔案裡的內容在編譯結果中會被刪除。它編譯結果是：

```js
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

其中 `Directions` 是由第三方函式庫定義好的全域性變數。

#### `declare namespace`

`namespace` 是 ts 早期時為了解決模組化而創造的關鍵字，中文稱為名稱空間。

由於歷史遺留原因，在早期還沒有 ES6 的時候，ts 提供了一種模組化方案，使用 `module` 關鍵字表示內部模組。但由於後來 ES6 也使用了 `module` 關鍵字，ts 為了相容 ES6，使用 `namespace` 替代了自己的 `module`，更名為名稱空間。

隨著 ES6 的廣泛應用，現在已經不建議再使用 ts 中的 `namespace`，而推薦使用 ES6 的模組化方案了，故我們不再需要學習 `namespace` 的使用了。

`namespace` 被淘汰了，但是在宣告檔案中，`declare namespace` 還是比較常用的，它用來表示全域性變數是一個物件，包含很多子屬性。

比如 `jQuery` 是一個全域性變數，它是一個物件，提供了一個 `jQuery.ajax` 方法可以呼叫，那麼我們就應該使用 `declare namespace jQuery` 來宣告這個擁有多個子屬性的全域性變數。

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

```ts
// src/index.ts

jQuery.ajax('/api/get_something');
```

注意，在 `declare namespace` 內部，我們直接使用 `function ajax` 來宣告函式，而不是使用 `declare function ajax`。類似的，也可以使用 `const`, `class`, `enum` 等語句[<sup>9</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/09-declare-namespace)：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
    const version: number;
    class Event {
        blur(eventType: EventType): void
    }
    enum EventType {
        CustomClick
    }
}
```

```ts
// src/index.ts

jQuery.ajax('/api/get_something');
console.log(jQuery.version);
const e = new jQuery.Event();
e.blur(jQuery.EventType.CustomClick);
```

##### 巢狀的名稱空間

如果物件擁有深層的層級，則需要用巢狀的 `namespace` 來宣告深層的屬性的型別[<sup>10</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/10-declare-namespace-nesting)：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
    namespace fn {
        function extend(object: any): void;
    }
}
```

```ts
// src/index.ts

jQuery.ajax('/api/get_something');
jQuery.fn.extend({
    check: function() {
        return this.each(function() {
            this.checked = true;
        });
    }
});
```

假如 `jQuery` 下僅有 `fn` 這一個屬性（沒有 `ajax` 等其他屬性或方法），則可以不需要巢狀 `namespace`[<sup>11</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/11-declare-namespace-dot)：

```ts
// src/jQuery.d.ts

declare namespace jQuery.fn {
    function extend(object: any): void;
}
```

```ts
// src/index.ts

jQuery.fn.extend({
    check: function() {
        return this.each(function() {
            this.checked = true;
        });
    }
});
```

#### `interface` 和 `type`

除了全域性變數之外，可能有一些型別我們也希望能暴露出來。在型別宣告檔案中，我們可以直接使用 `interface` 或 `type` 來宣告一個全域性的介面或型別[<sup>12</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/12-interface)：

```ts
// src/jQuery.d.ts

interface AjaxSettings {
    method?: 'GET' | 'POST'
    data?: any;
}
declare namespace jQuery {
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

這樣的話，在其他檔案中也可以使用這個介面或型別了：

```ts
// src/index.ts

let settings: AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/post_something', settings);
```

`type` 與 `interface` 類似，不再贅述。

##### 防止命名衝突

暴露在最外層的 `interface` 或 `type` 會作為全域性型別作用於整個專案中，我們應該儘可能的減少全域性變數或全域性型別的數量。故最好將他們放到 `namespace` 下[<sup>13</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/13-avoid-name-conflict)：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    interface AjaxSettings {
        method?: 'GET' | 'POST'
        data?: any;
    }
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

注意，在使用這個 `interface` 的時候，也應該加上 `jQuery` 字首：

```ts
// src/index.ts

let settings: jQuery.AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/post_something', settings);
```

#### 宣告合併

假如 jQuery 既是一個函式，可以直接被呼叫 `jQuery('#foo')`，又是一個物件，擁有子屬性 `jQuery.ajax()`（事實確實如此），那麼我們可以組合多個宣告語句，它們會不衝突的合併起來[<sup>14</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/14-declaration-merging)：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

```ts
// src/index.ts

jQuery('#foo');
jQuery.ajax('/api/get_something');
```

關於宣告合併的更多用法，可以檢視[宣告合併](../advanced/declaration-merging.md)章節。

### npm 包

一般我們透過 `import foo from 'foo'` 匯入一個 npm 包，這是符合 ES6 模組規範的。

在我們嘗試給一個 npm 包建立宣告檔案之前，需要先看看它的宣告檔案是否已經存在。一般來說，npm 包的宣告檔案可能存在於兩個地方：

1. 與該 npm 包繫結在一起。判斷依據是 `package.json` 中有 `types` 欄位，或者有一個 `index.d.ts` 宣告檔案。這種模式不需要額外安裝其他包，是最為推薦的，所以以後我們自己建立 npm 包的時候，最好也將宣告檔案與 npm 包繫結在一起。
2. 釋出到 `@types` 裡。我們只需要嘗試安裝一下對應的 `@types` 包就知道是否存在該宣告檔案，安裝命令是 `npm install @types/foo --save-dev`。這種模式一般是由於 npm 包的維護者沒有提供宣告檔案，所以只能由其他人將宣告檔案釋出到 `@types` 裡了。

假如以上兩種方式都沒有找到對應的宣告檔案，那麼我們就需要自己為它寫宣告檔案了。由於是透過 `import` 語句匯入的模組，所以宣告檔案存放的位置也有所約束，一般有兩種方案：

1. 建立一個 `node_modules/@types/foo/index.d.ts` 檔案，存放 `foo` 模組的宣告檔案。這種方式不需要額外的配置，但是 `node_modules` 目錄不穩定，程式碼也沒有被儲存到儲存庫中，無法回溯版本，有不小心被刪除的風險，故不太建議用這種方案，一般只用作臨時測試。
2. 建立一個 `types` 目錄，專門用來管理自己寫的宣告檔案，將 `foo` 的宣告檔案放到 `types/foo/index.d.ts` 中。這種方式需要配置下 `tsconfig.json` 中的 `paths` 和 `baseUrl` 欄位。

目錄結構：

```plain
/path/to/project
├── src
|  └── index.ts
├── types
|  └── foo
|     └── index.d.ts
└── tsconfig.json
```

`tsconfig.json` 內容：

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "baseUrl": "./",
        "paths": {
            "*": ["types/*"]
        }
    }
}
```

如此配置之後，透過 `import` 匯入 `foo` 的時候，也會去 `types` 目錄下尋找對應的模組的宣告檔案了。

注意 `module` 配置可以有很多種選項，不同的選項會影響模組的匯入匯出模式。這裡我們使用了 `commonjs` 這個最常用的選項，後面的課程也都預設使用的這個選項。

不管採用了以上兩種方式中的哪一種，我都**強烈建議**大家將書寫好的宣告檔案（透過給第三方函式庫發 pull request，或者直接提交到 `@types` 裡）釋出到開源社群中，享受了這麼多社群的優秀的資源，就應該在力所能及的時候給出一些回饋。只有所有人都參與進來，才能讓 ts 社群更加繁榮。

npm 包的宣告檔案主要有以下幾種語法：

- [`export`](#export) 匯出變數
- [`export namespace`](#export-namespace) 匯出（含有子屬性的）物件
- [`export default`](#export-default) ES6 預設匯出
- [`export =`](#export-1) commonjs 匯出模組

#### `export`

npm 包的宣告檔案與全域性變數的宣告檔案有很大區別。在 npm 包的宣告檔案中，使用 `declare` 不再會宣告一個全域性變數，而只會在當前檔案中宣告一個區域性變數。只有在宣告檔案中使用 `export` 匯出，然後在使用方 `import` 匯入後，才會應用到這些型別宣告。

`export` 的語法與普通的 ts 中的語法類似，區別僅在於宣告檔案中禁止定義具體的實現[<sup>15</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/15-export)：

```ts
// types/foo/index.d.ts

export const name: string;
export function getName(): string;
export class Animal {
    constructor(name: string);
    sayHi(): string;
}
export enum Directions {
    Up,
    Down,
    Left,
    Right
}
export interface Options {
    data: any;
}
```

對應的匯入和使用模組應該是這樣：

```ts
// src/index.ts

import { name, getName, Animal, Directions, Options } from 'foo';

console.log(name);
let myName = getName();
let cat = new Animal('Tom');
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
let options: Options = {
    data: {
        name: 'foo'
    }
};
```

##### 混用 `declare` 和 `export`

我們也可以使用 `declare` 先宣告多個變數，最後再用 `export` 一次性匯出。上例的宣告檔案可以等價的改寫為[<sup>16</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/16-declare-and-export)：

```ts
// types/foo/index.d.ts

declare const name: string;
declare function getName(): string;
declare class Animal {
    constructor(name: string);
    sayHi(): string;
}
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
interface Options {
    data: any;
}

export { name, getName, Animal, Directions, Options };
```

注意，與全域性變數的宣告檔案類似，`interface` 前是不需要 `declare` 的。

#### `export namespace`

與 `declare namespace` 類似，`export namespace` 用來匯出一個擁有子屬性的物件[<sup>17</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/17-export-namespace)：

```ts
// types/foo/index.d.ts

export namespace foo {
    const name: string;
    namespace bar {
        function baz(): string;
    }
}
```

```ts
// src/index.ts

import { foo } from 'foo';

console.log(foo.name);
foo.bar.baz();
```

#### `export default`

在 ES6 模組系統中，使用 `export default` 可以匯出一個預設值，使用方可以用 `import foo from 'foo'` 而不是 `import { foo } from 'foo'` 來匯入這個預設值。

在型別宣告檔案中，`export default` 用來匯出預設值的型別[<sup>18</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/18-export-default)：

```ts
// types/foo/index.d.ts

export default function foo(): string;
```

```ts
// src/index.ts

import foo from 'foo';

foo();
```

注意，只有 `function`、`class` 和 `interface` 可以直接預設匯出，其他的變數需要先定義出來，再預設匯出[<sup>19</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/19-export-default-enum-error)：

```ts
// types/foo/index.d.ts

export default enum Directions {
// ERROR: Expression expected.
    Up,
    Down,
    Left,
    Right
}
```

上例中 `export default enum` 是錯誤的語法，需要使用 `declare enum` 定義出來，然後使用 `export default` 匯出：

```ts
// types/foo/index.d.ts

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

export default Directions;
```

針對這種預設匯出，我們一般會將匯出語句放在整個宣告檔案的最前面[<sup>20</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/20-export-default-enum)：

```ts
// types/foo/index.d.ts

export default Directions;

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
```

#### `export =`

在 commonjs 規範中，我們用以下方式來匯出一個模組：

```js
// 整體匯出
module.exports = foo;
// 單個匯出
exports.bar = bar;
```

在 ts 中，針對這種模組匯出，有多種方式可以匯入，第一種方式是 `const ... = require`：

```js
// 整體匯入
const foo = require('foo');
// 單個匯入
const bar = require('foo').bar;
```

第二種方式是 `import ... from`，注意針對整體匯出，需要使用 `import * as` 來匯入：

```ts
// 整體匯入
import * as foo from 'foo';
// 單個匯入
import { bar } from 'foo';
```

第三種方式是 `import ... require`，這也是 ts 官方推薦的方式：

```ts
// 整體匯入
import foo = require('foo');
// 單個匯入
import bar = foo.bar;
```

對於這種使用 commonjs 規範的函式庫，假如要為它寫型別宣告檔案的話，就需要使用到 `export =` 這種語法了[<sup>21</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/21-export-equal)：

```ts
// types/foo/index.d.ts

export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

需要注意的是，上例中使用了 `export =` 之後，就不能再單個匯出 `export { bar }` 了。所以我們透過宣告合併，使用 `declare namespace foo` 來將 `bar` 合併到 `foo` 裡。

準確地講，`export =` 不僅可以用在宣告檔案中，也可以用在普通的 ts 檔案中。實際上，`import ... require` 和 `export =` 都是 ts 為了相容 AMD 規範和 commonjs 規範而創立的新語法，由於並不常用也不推薦使用，所以這裡就不詳細介紹了，感興趣的可以看[官方文件](https://www.typescriptlang.org/docs/handbook/modules.html#export--and-import--require)。

由於很多第三方函式庫是 commonjs 規範的，所以宣告檔案也就不得不用到 `export =` 這種語法了。但是還是需要再強調下，相比與 `export =`，我們更推薦使用 ES6 標準的 `export default` 和 `export`。

### UMD 函式庫

既可以透過 `<script>` 標籤引入，又可以透過 `import` 匯入的函式庫，稱為 UMD 函式庫。相比於 npm 包的型別宣告檔案，我們需要額外宣告一個全域性變數，為了實現這種方式，ts 提供了一個新語法 `export as namespace`。

#### `export as namespace`

一般使用 `export as namespace` 時，都是先有了 npm 包的宣告檔案，再基於它新增一條 `export as namespace` 語句，即可將宣告好的一個變數宣告為全域性變數，舉例如下[<sup>22</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/22-export-as-namespace)：

```ts
// types/foo/index.d.ts

export as namespace foo;
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

當然它也可以與 `export default` 一起使用：

```ts
// types/foo/index.d.ts

export as namespace foo;
export default foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

### 直接擴充套件全域性變數

有的第三方函式庫擴充套件了一個全域性變數，可是此全域性變數的型別卻沒有相應的更新過來，就會導致 ts 編譯錯誤，此時就需要擴充套件全域性變數的型別。比如擴充套件 `String` 型別[<sup>23</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/23-merge-global-interface)：

```ts
interface String {
    prependHello(): string;
}

'foo'.prependHello();
```

透過宣告合併，使用 `interface String` 即可給 `String` 新增屬性或方法。

也可以使用 `declare namespace` 給已有的名稱空間新增型別宣告[<sup>24</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/24-merge-global-namespace)：

```ts
// types/jquery-plugin/index.d.ts

declare namespace JQuery {
    interface CustomOptions {
        bar: string;
    }
}

interface JQueryStatic {
    foo(options: JQuery.CustomOptions): string;
}
```

```ts
// src/index.ts

jQuery.foo({
    bar: ''
});
```

### 在 npm 包或 UMD 函式庫中擴充套件全域性變數

如之前所說，對於一個 npm 包或者 UMD 函式庫的宣告檔案，只有 `export` 匯出的型別宣告才能被匯入。所以對於 npm 包或 UMD 函式庫，如果匯入此函式庫之後會擴充套件全域性變數，則需要使用另一種語法在宣告檔案中擴充套件全域性變數的型別，那就是 `declare global`。

#### `declare global`

使用 `declare global` 可以在 npm 包或者 UMD 函式庫的宣告檔案中擴充套件全域性變數的型別[<sup>25</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/25-declare-global)：

```ts
// types/foo/index.d.ts

declare global {
    interface String {
        prependHello(): string;
    }
}

export {};
```

```ts
// src/index.ts

'bar'.prependHello();
```

注意即使此宣告檔案不需要匯出任何東西，仍然需要匯出一個空物件，用來告訴編譯器這是一個模組的宣告檔案，而不是一個全域性變數的宣告檔案。

### 模組外掛

有時透過 `import` 匯入一個模組外掛，可以改變另一個原有模組的結構。此時如果原有模組已經有了型別宣告檔案，而外掛模組沒有型別宣告檔案，就會導致型別不完整，缺少外掛部分的型別。ts 提供了一個語法 `declare module`，它可以用來擴充套件原有模組的型別。

#### `declare module`

如果是需要擴充套件原有模組的話，需要在型別宣告檔案中先參考原有模組，再使用 `declare module` 擴充套件原有模組[<sup>26</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/26-declare-module)：

```ts
// types/moment-plugin/index.d.ts

import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}
```

```ts
// src/index.ts

import * as moment from 'moment';
import 'moment-plugin';

moment.foo();
```

`declare module` 也可用於在一個檔案中一次性宣告多個模組的型別[<sup>27</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/27-multiple-declare-module)：

```ts
// types/foo-bar.d.ts

declare module 'foo' {
    export interface Foo {
        foo: string;
    }
}

declare module 'bar' {
    export function bar(): string;
}
```

```ts
// src/index.ts

import { Foo } from 'foo';
import * as bar from 'bar';

let f: Foo;
bar.bar();
```

### 宣告檔案中的依賴

一個宣告檔案有時會依賴另一個宣告檔案中的型別，比如在前面的 `declare module` 的例子中，我們就在宣告檔案中匯入了 `moment`，並且使用了 `moment.CalendarKey` 這個型別：

```ts
// types/moment-plugin/index.d.ts

import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}
```

除了可以在宣告檔案中透過 `import` 匯入另一個宣告檔案中的型別之外，還有一個語法也可以用來匯入另一個宣告檔案，那就是三斜線指令。

#### 三斜線指令

與 `namespace` 類似，三斜線指令也是 ts 在早期版本中為了描述模組之間的依賴關係而創造的語法。隨著 ES6 的廣泛應用，現在已經不建議再使用 ts 中的三斜線指令來宣告模組之間的依賴關係了。

但是在宣告檔案中，它還是有一定的用武之地。

類似於宣告檔案中的 `import`，它可以用來匯入另一個宣告檔案。與 `import` 的區別是，當且僅當在以下幾個場景下，我們才需要使用三斜線指令替代 `import`：

- 當我們在**書寫**一個全域性變數的宣告檔案時
- 當我們需要**依賴**一個全域性變數的宣告檔案時

##### **書寫**一個全域性變數的宣告檔案

這些場景聽上去很拗口，但實際上很好理解——在全域性變數的宣告檔案中，是不允許出現 `import`, `export` 關鍵字的。一旦出現了，那麼他就會被視為一個 npm 包或 UMD 函式庫，就不再是全域性變數的宣告檔案了。故當我們在書寫一個全域性變數的宣告檔案時，如果需要參考另一個函式庫的型別，那麼就必須用三斜線指令了[<sup>28</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/28-triple-slash-directives)：

```ts
// types/jquery-plugin/index.d.ts

/// <reference types="jquery" />

declare function foo(options: JQuery.AjaxSettings): string;
```

```ts
// src/index.ts

foo({});
```

三斜線指令的語法如上，`///` 後面使用 xml 的格式添加了對 `jquery` 型別的依賴，這樣就可以在宣告檔案中使用 `JQuery.AjaxSettings` 型別了。

注意，三斜線指令必須放在檔案的最頂端，三斜線指令的前面只允許出現單行或多行註釋。

##### **依賴**一個全域性變數的宣告檔案

在另一個場景下，當我們需要依賴一個全域性變數的宣告檔案時，由於全域性變數不支援透過 `import` 匯入，當然也就必須使用三斜線指令來引入了[<sup>29</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/29-triple-slash-directives-global)：

```ts
// types/node-plugin/index.d.ts

/// <reference types="node" />

export function foo(p: NodeJS.Process): string;
```

```ts
// src/index.ts

import { foo } from 'node-plugin';

foo(global.process);
```

在上面的例子中，我們透過三斜線指引入了 `node` 的型別，然後在宣告檔案中使用了 `NodeJS.Process` 這個型別。最後在使用到 `foo` 的時候，傳入了 `node` 中的全域性變數 `process`。

由於引入的 `node` 中的型別都是全域性變數的型別，它們是沒有辦法透過 `import` 來匯入的，所以這種場景下也只能透過三斜線指令來引入了。

以上兩種使用場景下，都是由於需要書寫或需要依賴全域性變數的宣告檔案，所以必須使用三斜線指令。在其他的一些不是必要使用三斜線指令的情況下，就都需要使用 `import` 來匯入。

##### 拆分宣告檔案

當我們的全域性變數的宣告檔案太大時，可以透過拆分為多個檔案，然後在一個入口檔案中將它們一一引入，來提高程式碼的可維護性。比如 `jQuery` 的宣告檔案就是這樣的：

```ts
// node_modules/@types/jquery/index.d.ts

/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />

export = jQuery;
```

其中用到了 `types` 和 `path` 兩種不同的指令。它們的區別是：`types` 用於宣告對另一個函式庫的依賴，而 `path` 用於宣告對另一個檔案的依賴。

上例中，`sizzle` 是與 `jquery` 平行的另一個函式庫，所以需要使用 `types="sizzle"` 來宣告對它的依賴。而其他的三斜線指令就是將 `jquery` 的宣告拆分到不同的檔案中了，然後在這個入口檔案中使用 `path="foo"` 將它們一一引入。

##### 其他三斜線指令

除了這兩種三斜線指令之外，還有其他的三斜線指令，比如 `/// <reference no-default-lib="true"/>`, `/// <amd-module />` 等，但它們都是廢棄的語法，故這裡就不介紹了，詳情可見[官網](http://www.typescriptlang.org/docs/handbook/triple-slash-directives.html)。

### 自動產生宣告檔案

如果函式庫的原始碼本身就是由 ts 寫的，那麼在使用 `tsc` 指令碼將 ts 編譯為 js 的時候，新增 `declaration` 選項，就可以同時也產生 `.d.ts` 宣告檔案了。

我們可以在命令列中新增 `--declaration`（簡寫 `-d`），或者在 `tsconfig.json` 中新增 `declaration` 選項。這裡以 `tsconfig.json` 為例：

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "outDir": "lib",
        "declaration": true,
    }
}
```

上例中我們添加了 `outDir` 選項，將 ts 檔案的編譯結果輸出到 `lib` 目錄下，然後添加了 `declaration` 選項，設定為 `true`，表示將會由 ts 檔案自動產生 `.d.ts` 宣告檔案，也會輸出到 `lib` 目錄下。

執行 `tsc` 之後，目錄結構如下[<sup>30</sup>](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/30-auto-d-ts)：

```plain
/path/to/project
├── lib
|  ├── bar
|  |  ├── index.d.ts
|  |  └── index.js
|  ├── index.d.ts
|  └── index.js
├── src
|  ├── bar
|  |  └── index.ts
|  └── index.ts
├── package.json
└── tsconfig.json
```

在這個例子中，`src` 目錄下有兩個 ts 檔案，分別是 `src/index.ts` 和 `src/bar/index.ts`，它們被編譯到 `lib` 目錄下的同時，也會產生對應的兩個宣告檔案 `lib/index.d.ts` 和 `lib/bar/index.d.ts`。它們的內容分別是：

```ts
// src/index.ts

export * from './bar';

export default function foo() {
    return 'foo';
}
```

```ts
// src/bar/index.ts

export function bar() {
    return 'bar';
}
```

```ts
// lib/index.d.ts

export * from './bar';
export default function foo(): string;
```

```ts
// lib/bar/index.d.ts

export declare function bar(): string;
```

可見，自動產生的宣告檔案基本保持了原始碼的結構，而將具體實現去掉了，生成了對應的型別宣告。

使用 `tsc` 自動產生宣告檔案時，每個 ts 檔案都會對應一個 `.d.ts` 宣告檔案。這樣的好處是，使用方不僅可以在使用 `import foo from 'foo'` 匯入預設的模組時獲得型別提示，還可以在使用 `import bar from 'foo/lib/bar'` 匯入一個子模組時，也獲得對應的型別提示。

除了 `declaration` 選項之外，還有幾個選項也與自動產生宣告檔案有關，這裡只簡單列舉出來，不做詳細示範了：

- `declarationDir` 設定產生 `.d.ts` 檔案的目錄
- `declarationMap` 對每個 `.d.ts` 檔案，都產生對應的 `.d.ts.map`（sourcemap）檔案
- `emitDeclarationOnly` 僅產生 `.d.ts` 檔案，不產生 `.js` 檔案

## 釋出宣告檔案

當我們為一個函式庫寫好了宣告檔案之後，下一步就是將它釋出出去了。

此時有兩種方案：

1. 將宣告檔案和原始碼放在一起
2. 將宣告檔案釋出到 `@types` 下

這兩種方案中優先選擇第一種方案。保持宣告檔案與原始碼在一起，使用時就不需要額外增加單獨的宣告檔案函式庫的依賴了，而且也能保證宣告檔案的版本與原始碼的版本保持一致。

僅當我們在給別人的儲存庫新增型別宣告檔案，但原作者不願意合併 pull request 時，才需要使用第二種方案，將宣告檔案釋出到 `@types` 下。

### 將宣告檔案和原始碼放在一起

如果宣告檔案是透過 `tsc` 自動產生的，那麼無需做任何其他配置，只需要把編譯好的檔案也釋出到 npm 上，使用方就可以獲取到型別提示了。

如果是手動寫的宣告檔案，那麼需要滿足以下條件之一，才能被正確的識別：

- 給 `package.json` 中的 `types` 或 `typings` 欄位指定一個型別宣告檔案地址
- 在專案根目錄下，編寫一個 `index.d.ts` 檔案
- 針對入口檔案（`package.json` 中的 `main` 欄位指定的入口檔案），編寫一個同名不同字尾的 `.d.ts` 檔案

第一種方式是給 `package.json` 中的 `types` 或 `typings` 欄位指定一個型別宣告檔案地址。比如：

```json
{
    "name": "foo",
    "version": "1.0.0",
    "main": "lib/index.js",
    "types": "foo.d.ts",
}
```

指定了 `types` 為 `foo.d.ts` 之後，匯入此函式庫的時候，就會去找 `foo.d.ts` 作為此函式庫的型別宣告檔案了。

`typings` 與 `types` 一樣，只是另一種寫法。

如果沒有指定 `types` 或 `typings`，那麼就會在根目錄下尋找 `index.d.ts` 檔案，將它視為此函式庫的型別宣告檔案。

如果沒有找到 `index.d.ts` 檔案，那麼就會尋找入口檔案（`package.json` 中的 `main` 欄位指定的入口檔案）是否存在對應同名不同字尾的 `.d.ts` 檔案。

比如 `package.json` 是這樣時：

```json
{
    "name": "foo",
    "version": "1.0.0",
    "main": "lib/index.js"
}
```

就會先識別 `package.json` 中是否存在 `types` 或 `typings` 欄位。發現不存在，那麼就會尋找是否存在 `index.d.ts` 檔案。如果還是不存在，那麼就會尋找是否存在 `lib/index.d.ts` 檔案。假如說連 `lib/index.d.ts` 都不存在的話，就會被認為是一個沒有提供型別宣告檔案的函式庫了。

有的函式庫為了支援匯入子模組，比如 `import bar from 'foo/lib/bar'`，就需要額外再編寫一個型別宣告檔案 `lib/bar.d.ts` 或者 `lib/bar/index.d.ts`，這與自動產生宣告檔案類似，一個函式庫中同時包含了多個型別宣告檔案。

### 將宣告檔案釋出到 `@types` 下

如果我們是在給別人的儲存庫新增型別宣告檔案，但原作者不願意合併 pull request，那麼就需要將宣告檔案釋出到 `@types` 下。

與普通的 npm 模組不同，`@types` 是統一由 [DefinitelyTyped][] 管理的。要將宣告檔案釋出到 `@types` 下，就需要給 [DefinitelyTyped][] 建立一個 pull-request，其中包含了型別宣告檔案，測試程式碼，以及 `tsconfig.json` 等。

pull-request 需要符合它們的規範，並且透過測試，才能被合併，稍後就會被自動釋出到 `@types` 下。

在 [DefinitelyTyped][] 中建立一個新的型別宣告，需要用到一些工具，[DefinitelyTyped][] 的文件中已經有了[詳細的介紹](https://github.com/DefinitelyTyped/DefinitelyTyped#create-a-new-package)，這裡就不贅述了，以官方文件為準。

如果大家有此類別需求，可以參考下筆者[提交的 pull-request](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/30336/files) 。

## 參考

- [Writing Declaration Files](http://www.typescriptlang.org/docs/handbook/writing-declaration-files.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/declaration%20files/Introduction.html)）
- [Triple-Slash Directives](http://www.typescriptlang.org/docs/handbook/triple-slash-directives.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Triple-Slash%20Directives.html)）
- [typeRoots or paths](https://github.com/Microsoft/TypeScript/issues/22217#issuecomment-369783776)
- [DefinitelyTyped][]

---

- [上一章：型別斷言](type-assertion.md)
- [下一章：內建物件](built-in-objects.md)

[DefinitelyTyped]: https://github.com/DefinitelyTyped/DefinitelyTyped/

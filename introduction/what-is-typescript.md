# 什麼是 TypeScript

首先，我對 TypeScript 的理解如下：

[TypeScript][] 是 JavaScript 的一個超集，主要提供了**型別系統**和**對 ES6 的支援**，它由 Microsoft 開發，程式碼[開源於 GitHub](https://github.com/Microsoft/TypeScript) 上。

其次參考[官網][TypeScript]的定義：

> TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Any browser. Any host. Any OS. Open source.

翻譯成中文即是：

> TypeScript 是 JavaScript 的型別的超集，它可以編譯成純 JavaScript。編譯出來的 JavaScript 可以執行在任何瀏覽器上。TypeScript 編譯工具可以執行在任何伺服器和任何系統上。TypeScript 是開源的。

## 為什麼選擇 TypeScript

[TypeScript 官網][TypeScript]列舉了一些優勢，不過我更願意自己總結一下：

### TypeScript 增加了程式碼的可讀性和可維護性

- 型別系統實際上是最好的文件，大部分的函式看看型別的定義就可以知道如何使用了
- 可以在編譯階段就發現大部分錯誤，這總比在執行時候出錯好
- 增強了編輯器和 IDE 的功能，包括程式碼自動完成、介面提示、跳轉到定義、重構等

### TypeScript 非常包容

- TypeScript 是 JavaScript 的超集，`.js` 檔案可以直接重新命名為 `.ts` 即可
- 即使不顯式的定義型別，也能夠自動做出[型別推論](../basics/type-inference.md)
- 可以定義從簡單到複雜的幾乎一切型別
- 即使 TypeScript 編譯報錯，也可以產生 JavaScript 檔案
- 相容第三方函式庫，即使第三方函式庫不是用 TypeScript 寫的，也可以編寫單獨的型別檔案供 TypeScript 讀取

### TypeScript 擁有活躍的社群

- 大部分第三方函式庫都有提供給 TypeScript 的型別定義檔案
- Google 開發的 Angular2 就是使用 TypeScript 編寫的
- TypeScript 擁抱了 ES6 規範，也支援部分 ESNext 草案的規範

### TypeScript 的缺點

任何事物都是有兩面性的，我認為 TypeScript 的弊端在於：

- 有一定的學習成本，需要理解介面（Interfaces）、泛型（Generics）、類別（Classes）、列舉型別（Enums）等前端工程師可能不是很熟悉的概念
- 短期可能會增加一些開發成本，畢竟要多寫一些型別的定義，不過對於一個需要長期維護的專案，TypeScript 能夠減少其維護成本
- 整合到建構流程需要一些工作量
- 可能和一些函式庫結合的不是很完美

大家可以根據自己團隊和專案的情況判斷是否需要使用 TypeScript。

[TypeScript]: http://www.typescriptlang.org/

---

- [上一章：簡介](README.md)
- [下一章：安裝 TypeScript](get-typescript.md)

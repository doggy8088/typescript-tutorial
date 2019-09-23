# TypeScript 新手入門 (正體中文版)

從 JavaScript 程式設計師的角度總結思考，循序漸進的理解 TypeScript。

## 正體中文翻譯

本書 [TypeScript 新手指南](https://willh.gitbook.io/typescript-tutorial/) 的原文來自於簡體中文的 [TypeScript 入门教程](https://github.com/xcatliu/typescript-tutorial/) 專案，並將其內容翻譯為正體中文版。如有正體中文翻譯不妥的地方，歡迎發 [Issue](https://github.com/doggy8088/typescript-tutorial/issues) 給我，或到 [Will 保哥的技術交流中心](https://www.facebook.com/will.fans/) 粉絲團留言給我，謝謝！🙂

---

## 關於本書

- [線上閱讀](https://ts.xcatliu.com/)（部署在 [GitBook](https://www.gitbook.com/book/xcatliu/typescript-tutorial/details) 上，可能需要翻牆）
- [線上閱讀（GitHub 版）](https://github.com/xcatliu/typescript-tutorial/blob/master/README.md)
- [GitHub 地址][GitHub]
- 作者：[xcatliu](https://github.com/xcatliu/)
- 官方 QQ 群：[加入 QQ 群 767142358](https://jq.qq.com/?_wv=1027&k=5nkkFCl)

本書是作者在學習 [TypeScript] 後整理的學習筆記。

隨著對 TypeScript 理解的加深和 TypeScript 社群的發展，本書也會做出相應的更新，歡迎大家 [Star 收藏][GitHub]。

- 發現文章內容有問題，可以直接在頁面下方評論
- 對專案的建議，可以[提交 issue](https://github.com/xcatliu/typescript-tutorial/issues/new) 向作者反饋
- 歡迎直接提交 pull-request 參與貢獻

## 為什麼要寫本書

TypeScript 雖然有[官方手冊][Handbook]及其[非官方中文版][中文手冊]，但是它每一章都希望能詳盡的描述一個概念，導致前面的章節就會包含很多後面才會學習到的內容，而有些本該一開始就瞭解的基礎知識卻在後面才會涉及。如果是初學者，可能需要閱讀多次才能理解。所以它更適合用來查閱，而不是學習。

與官方手冊不同，本書著重於從 JavaScript 程式設計師的角度總結思考，循序漸進的理解 TypeScript，希望能給大家一些幫助和啟示。

由於一些知識點與官方手冊重合度很高，本書會在相應章節推薦直接閱讀中文手冊。

## 關於 TypeScript

[TypeScript] 是 JavaScript 的一個超集，主要提供了**型別系統**和**對 ES6 的支援**，它由 Microsoft 開發，程式碼[開源於 GitHub](https://github.com/Microsoft/TypeScript) 上。

它的第一個版本釋出於 2012 年 10 月，經歷了多次更新後，現在已成為前端社群中不可忽視的力量，不僅在 Microsoft 內部得到廣泛運用，而且 Google 的 [Angular2](https://angular.io/) 也使用了 TypeScript 作為開發語言。

## 適合人群

本書適合以下人群

- 熟悉 JavaScript，至少閱讀過一遍[《JavaScript 高階程式設計》](https://book.douban.com/subject/10546125/)
- 瞭解 ES6，推薦閱讀 [ECMAScript 6 入門]
- 瞭解 Node.js，會用 npm 安裝及使用一些工具
- 想了解 TypeScript 或者想對 TypeScript 有更深的理解

本書**不適合**以下人群

- 沒有系統學習過 JavaScript
- 已經能夠很熟練的運用 TypeScript

## 評價

> 《TypeScript 入門課程》全面介紹了 TS 強大的型別系統，完整而簡潔，示例豐富，比官方文件更易讀，非常適合作為初學者學習 TS 的第一本書。
>
> —— [阮一峰](https://github.com/ruanyf)

## 目錄

- [前言](README.md)
- [簡介](introduction/README.md)
  - [什麼是 TypeScript](introduction/what-is-typescript.md)
  - [安裝 TypeScript](introduction/get-typescript.md)
  - [Hello TypeScript](introduction/hello-typescript.md)
- [基礎](basics/README.md)
  - [原始資料型別](basics/primitive-data-types.md)
  - [任意值](basics/any.md)
  - [型別推論](basics/type-inference.md)
  - [聯合型別](basics/union-types.md)
  - [物件的型別——介面](basics/type-of-object-interfaces.md)
  - [陣列的型別](basics/type-of-array.md)
  - [函式的型別](basics/type-of-function.md)
  - [型別斷言](basics/type-assertion.md)
  - [宣告檔案](basics/declaration-files.md)
  - [內建物件](basics/built-in-objects.md)
- [進階](advanced/README.md)
  - [型別別名](advanced/type-aliases.md)
  - [字串字面量型別](advanced/string-literal-types.md)
  - [元組](advanced/tuple.md)
  - [列舉](advanced/enum.md)
  - [類別](advanced/class.md)
  - [類別與介面](advanced/class-and-interfaces.md)
  - [泛型](advanced/generics.md)
  - [宣告合併](advanced/declaration-merging.md)
  - [擴充套件閱讀](advanced/further-reading.md)
- [工程](engineering/README.md)
  - [程式碼檢查](engineering/lint.md)
- [感謝](thanks/README.md)

## 版權許可

本書採用「保持署名—非商用」創意共享 4.0 許可證。

只要保持原作者署名和非商用，您可以自由地閱讀、分享、修改本書。

詳細的法律條文請參見[創意共享](http://creativecommons.org/licenses/by-nc/4.0/)網站。

## 相關資料

- [TypeScript 官網][TypeScript]
- [Handbook]（[中文版][中文手冊]）
- [ECMAScript 6 入門]

---

- [下一章：簡介](introduction/README.md)

[GitHub]: https://github.com/xcatliu/typescript-tutorial
[TypeScript]: http://www.typescriptlang.org/
[Handbook]: http://www.typescriptlang.org/docs/handbook/basic-types.html
[中文手冊]: https://zhongsp.gitbooks.io/typescript-handbook/content/
[ECMAScript 6 入門]: http://es6.ruanyifeng.com/

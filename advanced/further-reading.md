# 擴充套件閱讀

此處記錄了[官方手冊](http://www.typescriptlang.org/docs/handbook/basic-types.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/)）中包含，但是本書未涉及的概念。

我認為它們是一些不重要或者不屬於 TypeScript 的概念，所以這裡只給出一個簡單的釋義，詳細內容可以點選連結深入理解。

* [Never](http://www.typescriptlang.org/docs/handbook/basic-types.html#never)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Basic%20Types.html#never)）：永遠不存在值的型別，一般用於錯誤處理函式
* [Variable Declarations](http://www.typescriptlang.org/docs/handbook/variable-declarations.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Variable%20Declarations.html)）：使用 `let` 和 `const` 替代 `var`，這是 [ES6 的知識](http://es6.ruanyifeng.com/#docs/let)
* [`this`](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Functions.html#this)：箭頭函式的運用，這是 [ES6 的知識](http://es6.ruanyifeng.com/#docs/function)
* [Using Class Types in Generics](http://www.typescriptlang.org/docs/handbook/generics.html#using-class-types-in-generics)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Generics.html#在泛型裡使用類別型別)）：建立工廠函式時，需要參考建構函式的類別型別
* [Best common type](http://www.typescriptlang.org/docs/handbook/type-inference.html#best-common-type)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Type%20Inference.html#最佳通用型別)）：陣列的型別推論
* [Contextual Type](http://www.typescriptlang.org/docs/handbook/type-inference.html#contextual-type)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Type%20Inference.html#上下文型別)）：函式輸入的型別推論
* [Type Compatibility](http://www.typescriptlang.org/docs/handbook/type-compatibility.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Type%20Compatibility.html)）：允許不嚴格符合型別，只需要在一定規則下相容即可
* [Advanced Types](http://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#交叉型別（intersection-types）)）：使用 `&` 將多種型別的共有部分疊加成一種型別
* [Type Guards and Differentiating Types](http://www.typescriptlang.org/docs/handbook/advanced-types.html#type-guards-and-differentiating-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#型別保護與區分類型（type-guards-and-differentiating-types）)）：聯合型別在一些情況下被識別為特定的型別
* [Discriminated Unions](http://www.typescriptlang.org/docs/handbook/advanced-types.html#discriminated-unions)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#可辨識聯合（discriminated-unions）)）：使用 `|` 聯合多個介面的時候，透過一個共有的屬性形成可辨識聯合
* [Polymorphic `this` types](http://www.typescriptlang.org/docs/handbook/advanced-types.html#polymorphic-this-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#多型的this型別)）：父類別的某個方法返回 `this`，當子類別繼承父類別後，子類別的實例呼叫此方法，返回的 `this` 能夠被 TypeScript 正確的識別為子類別的實例。
* [Symbols](http://www.typescriptlang.org/docs/handbook/symbols.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Symbols.html)）：新原生型別，這是 [ES6 的知識](http://es6.ruanyifeng.com/#docs/symbol)
* [Iterators and Generators](http://www.typescriptlang.org/docs/handbook/iterators-and-generators.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Iterators%20and%20Generators.html)）：迭代器，這是 [ES6 的知識](http://es6.ruanyifeng.com/#docs/iterator)
* [Namespaces](http://www.typescriptlang.org/docs/handbook/namespaces.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Namespaces.html)）：避免全域性汙染，現在已被 [ES6 Module](http://es6.ruanyifeng.com/#docs/module) 替代
* [Decorators](http://www.typescriptlang.org/docs/handbook/decorators.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Decorators.html)）：修飾器，這是 [ES7 的一個提案](http://es6.ruanyifeng.com/#docs/decorator)
* [Mixins](http://www.typescriptlang.org/docs/handbook/mixins.html)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Mixins.html)）：一種程式設計模式，與 TypeScript 沒有直接關係，可以參考 [ES6 中 Mixin 模式的實現](http://es6.ruanyifeng.com/#docs/class#Mixin模式的實現)
* [上一章：宣告合併](declaration-merging.md)
* [下一章：工程](../engineering/)


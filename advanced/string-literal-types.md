# 字串字面量型別

字串字面量 \(String Literal\) 型別用來約束取值只能是某幾個字串中的一個。

## 簡單的例子

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 沒問題
handleEvent(document.getElementById('world'), 'dbclick'); // 報錯，event 不能為 'dbclick'

// index.ts(7,47): error TS2345: Argument of type '"dbclick"' is not assignable to parameter of type 'EventNames'.
```

上例中，我們使用 `type` 定了一個字串字面量型別 `EventNames`，它只能取三種字串中的一種。

注意，**型別別名與字串字面量型別都是使用 `type` 進行定義。**

## 參考

* [Advanced Types \# Type Aliases](http://www.typescriptlang.org/docs/handbook/advanced-types.html#string-literal-types)（[中文版](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/Advanced%20Types.html#字串字面量型別)）
* [上一章：型別別名](type-aliases.md)
* [下一章：元組](tuple.md)


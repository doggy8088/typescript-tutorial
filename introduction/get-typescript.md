# 安裝 TypeScript

TypeScript 的命令列工具安裝方法如下：

```bash
npm install -g typescript
```

以上命令會在全域性環境下安裝 `tsc` 命令，安裝完成之後，我們就可以在任何地方執行 `tsc` 命令了。

編譯一個 TypeScript 檔案很簡單：

```bash
tsc hello.ts
```

我們約定使用 TypeScript 編寫的檔案以 `.ts` 為字尾，用 TypeScript 編寫 React 時，以 `.tsx` 為字尾。

## 編輯器

TypeScript 最大的優勢便是增強了編輯器和 IDE 的功能，包括程式碼自動完成、介面提示、跳轉到定義、重構等。

主流的編輯器都支援 TypeScript，這裡我推薦使用 [Visual Studio Code](https://code.visualstudio.com/)。

它是一款開源，跨終端的輕量級編輯器，內建了 TypeScript 支援。

另外它本身也是[用 TypeScript 編寫的](https://github.com/Microsoft/vscode/)。

下載安裝：[https://code.visualstudio.com/](https://code.visualstudio.com/)

獲取其他編輯器或 IDE 對 TypeScript 的支援：

* [Sublime Text](https://github.com/Microsoft/TypeScript-Sublime-Plugin)
* [Atom](https://atom.io/packages/atom-typescript)
* [WebStorm](https://www.jetbrains.com/webstorm/)
* [Vim](https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support#vim)
* [Emacs](https://github.com/ananthakumaran/tide)
* [Eclipse](https://github.com/palantir/eclipse-typescript)
* [Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48593)
* [Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=48739)
* [上一章：什麼是 TypeScript](what-is-typescript.md)
* [下一章：Hello TypeScript](hello-typescript.md)


# 從 legacy code 中尋找出口（中）- HTML 篇

講完 CSS 的重構技巧之後，接下來會專注在 HTML 的重構技巧上。本系列的文章將不會談論 js 的重構部分，因為牽扯到較多的程式撰寫技巧，而且網路上類似的文章應該是不勝枚舉。

## 前言

其實 HTML 能夠重構的點並不多，主要就是標籤的正確使用以及顧慮 accessibility 等小細節。所以這篇文章會著重在如何使用正確的標籤以及語義化；基本的 accessiblity 認識跟 aria 標籤的使用，不僅對 screen reader 較友善，語義化的標籤使用也能夠在之後修改 HTML 時更有效率；最後是模板語言的技巧應用，這邊會用 rails 的 views 為例，不過概念的部分應該是相通的。

---

### 語義化

首先，我們先來看看 HTML 的 tag 有哪些吧

\["allcollection", "anchor", "area", "audio", "base", "body", "br", "button", "canvas", "collection", "content", "datalist", "details", "dialog", "directory", "div", "dlist", "document", "embed", "fieldset", "font", "form", "formcontrolscollection", "frame", "frameset", "head", "heading", "hr", "html", "iframe", "image", "input", "keygen", "label", "legend", "li", "link", "map", "marquee", "media", "menu", "meta", "meter", "mod", "object", "olist", "optgroup", "option", "optionscollection", "output", "paragraph", "param", "picture", "pre", "progress", "quote", "script", "select", "shadow", "slot", "source", "span", "style", "table", "tablecaption", "tablecell", "tablecol", "tablerow", "tablesection", "template", "textarea", "title", "track", "ulist", "unknown", "video"\]

```js
Object.getOwnPropertyNames(window) .filter(value => value.indexOf('HTML') >= 0).map(value => { const HTMLTagReg = /HTML([^Element$]+)/;  if(value.match(HTMLTagReg) !== null) { return value.match(HTMLTagReg)[1]; }

 return value; }) .map(value => value.toLowerCase()) .filter(value => value !== '') .sort()

```

### Accessibility

### 模板語言（已 rails views 為例）


## 工作內容

1. 初步設定 sudostrap

2. 設定 guideline 與設計師溝通

3. 修正頁面上的 layout 錯誤

## 發現的新東西

### [styleguide.io](http:/styleguide.io/)

最近在整理 Sudo 主站的 style guide，意外發現了這個網站。裡面整理了很多有關於撰寫 style guide 的注意事項，以及應該從哪裡下手。

裡面的內容超級多，還蠻值得一看的。不過目前最重要的就是：

#### 「先求有，再求好」

### Bootstrap 的使用情境

我們是否真的要使用 bootstrap？在開發前端頁面的時候，bootstrap 幾乎已經變成了前端首選框架之一了。但用下去之後在逐漸發現一些問題：

* class 的命名重複，使得開發時要被迫採用不同的命名規範（例如從 dash 改為使用底線命名 class）

* 樣式的些微不同造成大量覆寫 bootstrap 的 class，開發者幾乎要了解 bootstrap 的 class 做了什麼事，額外花了學習成本

* bootstrap 因為力求瀏覽器相容，常常會做其他設定保持表現一致。一樣讓開發者有時會搞不清狀況

* 很多用不到的無用 class。

bootstrap 的優點顯而易見，大量的客製化讓你幾乎不需要寫 CSS 就可以馬上做出介面能看的網頁。

但事實是，只要你有平台、網頁呈現、專屬於自己的 branding，用 bootstrap 在早期雖然是一大利器，卻在專案成長後變成拖垮開發效率跟維護難易度的殺手。

#### 「沒有不適合的框架」

當然，倒也不是完全地否定 bootstrap，畢竟一個 star 數已經快破十萬的前端框架，他的影響力不招自明。而且隨著 [bootstrap-sass     ](https://github.com/twbs/bootstrap-sass "bootstrap-sass")的推出，我們可以更容易地 import 一部份的 boostrap 模組，或是客製化我們想要的 class 命名。

更詳細的文章，可以參考：[bootstrap considered harmful](https://hiddedevries.nl/en/blog/2016-08-09-bootstrap-considered-harmful "bootstrap considered harmful")

### React styling

React 的生態圈越來越大，同時也越來越混亂。市面上流竄著各種 styling 的方式。e.g: [radium](https://github.com/FormidableLabs/radium), [css-module](react-css-modules), [class-name](https://github.com/JedWatson/classnames), [react-style](https://github.com/js-next/react-style)

我最喜歡的方式是用 css-module 搭配 classname 來開發，把 classname 編譯後可以保證不會和其他 class 撞名，也能夠充分發揮元件化的精神。而且 css-module 在 css-loader 裡面就有內建，不用再額外多裝 package。

所以之後的開發大概會像這樣。

```

ComponentName

  |- index.js

  |- style.css

  |- description.md

```

不過，真的很麻煩就是了...。

## Sudostrap

目前想到了一個名字叫做 **suki**，sudo + kit，在日文裡面是「喜歡」的意思。

我用了 [astrum ](http://astrum.nodividestudio.com/ "Astrum")當作 style guide 的 library。除了介面好看之外，設定也很容易，新增 component 也很只要在 cli 輸入指令即可。接下來就只要專注在寫文件跟 simple code 就好了！

關於 styleguide，我最近發現的想法是，**styleguide 的制定不應該只有單方（設計師、前端）決定，而是雙方一起加入討論過程，共同創造這份規範。**

下個禮拜會正式將 component 逐漸拆到 sudostrap 裡面並且努力撰寫文件。順便期待潘大的終極 styleguide 有朝一日能夠拿出來。


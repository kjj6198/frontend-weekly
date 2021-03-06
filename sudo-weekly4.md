---
title: 前端週刊
categories: 前端週刊
tags:
  - javascript
  - html
  - css
image: landing.png
desc: 這個禮拜不談「新」東西，多數會是關於網頁開發的重新思考。算是回過頭來漸漸補齊自己的技術債吧！比起前幾個禮拜所談的新東西，這一篇文章可能相對枯燥一點，當然篇幅也會比較長，但我認為這是必要的。

---

這個禮拜不談「新」東西，多數會是關於網頁開發的重新思考。算是回過頭來漸漸補齊自己的技術債吧！比起前幾個禮拜所談的新東西，這一篇文章可能相對枯燥一點，當然篇幅也會比較長，但我認為這是必要的。

這些東西在前端變化快速的時代下或許顯得不那麼重要，不過思考總是件好事。

## 語義化標籤

### 重新思考語義化這件事

會有這個想法，是看到 `instant article` 的 html 架構，他們規範 instant article 的架構必須符合他們的規範，而且結構的表達也很清楚。我在裡頭看到了好多我以前沒有注意過的 html tag。像是 `address` `figure` `caption` `summary` 。好奇之下查了一下[文件](http://www.w3schools.com/tags/tag_html.asp)等等。發現其實有很多語義化的標籤都已經支持目前主流的瀏覽器，spec 也寫得很清楚，但是目前主站多還是以 `div + class` 的方式做表達，雖然有些地方會套用 `header`，但我認為有更多適合的語義化標籤可以加入使用。除了減少不必要的 class 命名，也能夠提高 html 的易讀性跟 SEO，重點是，我們寫的是符合標準的 html。


而且 w3c 在 html5 致力推廣語義化的標籤，諸如 `nav` `header` `dd` `dt` 等等，並且將一些沒有意義的 tag 刪除或是不推薦使用如 `b` `font` `center` 等等。


### 對於 class 的思考

[語義化 css](http://www.w3cplus.com/css/semantic-css-with-intelligent-selectors.html)

用了那麼久的 class，我回去找了找 spec，發現了 w3c 對 class 的描述。

> There are no additional restrictions on the tokens authors can use in the class attribute, but authors are encouraged to use values that describe the nature of the content, rather than values that describe the desired presentation of the content.  -w3c


雖然 class 並沒有使用上的規定，但是鼓勵盡量將 class 用於表達元素內容而非描述元素的展現。也就是說那些 `col-md-*` 等表現性的 class 名稱其實在 w3c 的 spec 裡面是不符合規範的。但是會出現 unit class 的原因卻是因為我們沒有按照標準來而孵化出來的產物。雖然很多人擁護 unit class 的便利性跟節省開發的速度，但我認為比起開發上的時間節省，**身為網頁開發人員，我們要注重的更應該是頁面的表達跟使用者的互動**。

當然不得不承認的是，在理想上跟實際上總是會有差異。因為嚴格說起來，facebook 也沒有符合規範，他們是用`css module`的方式進行開發。

不過這裡想提出這樣子的想法跟大家討論。
雖然標準的定案到瀏覽器的實作，總是要花不少時間。

為什麼要符合標準？
1. 標準規範是由一大堆科學家研究過後、大量的討論過後所制定出來的規範，給大家遵守以便達到統一性。
2. 通常這些規範是 best practice。
3. 為什麼我們要為了節省開發時間而寫出不符標準的網頁本末倒置？



我們有更好的解決方案 > SASS
take id rethink
我不會反對用挖掘機清理災難現場，但我不會拿來整理自己的家。
給後代選擇器一個平反的機會，搭配 id 使用效果佳



```html
<!-- 元素展現 -->
<div class="margin-b-10">

</div>

<!-- 內容 -->
<div class="user_info">

</div>

```

自己的理解是這樣。

至於為什麼會造成這樣的原因，或許是因為在網頁發展初期，CSS 的支持性並不好，在語義化跟表現之間很難得到平衡，才會有當時純 style 的 `center` `width` 出現。但現在已經不是那個綁手綁腳的年代了。我們應該朝向語義化的年代前進，而且這也是`w3c`在推廣的事情，身為一個站在網路業尖端的優質網路招聘公司，就由我們來引領風騷吧！

對 `unit class` 難道沒有其他辦法解決嗎？我自己非常喜歡語義化的 `class`，而且很討厭 `row` `col` `text-center`，每次寫起來都會有罪惡感...。

至於用 unit class 的人，通常是因為不想花太多時間重寫 style 架構、節省開發時間、或是他根本不會 css，所以找個 `bootstrap` 直接套。但我們是前端工程師，我們有能力去組織 css。現在這個語義化年代，正是我們前端平反的好時機。

1. 推行標準的目的是為技術交流構建一個統一的上下文語境平台，提高溝通效率，避免雞同鴨講。
2. 同時標準跟規範的製定是經過一群資深開發者／科學家經過仔細研究及社區討論的，一套完整的一致的基礎架構系統是推進生態發展的必要條件。
3. 就Web語義化這件事情而言，如果你的HTML是基於標準來編寫的，意味著你的頁面具備更多的可能性。比如搜索引擎友好，多終端適配(不是響應式。。是指兼容各種閱讀設備、讀屏軟件等。參見microformats )，更智能的風格查錯能力。
4. 在前端開發體系裡，能體系專業性的地方不多。。拿程序複雜度而言，它跟大型後端系統差不止一個量級(前端的難度在於工程上)。。好不容易有一個能體現專業素養的領域(語義化Web)，為什麼我們不抓住機會為自己正名呢。。

我覺得這並不會增加維護成本，反而會在長期內減少冗餘樣式、不可預知的樣式覆蓋等維護問題。唯一的成本可能就是你在開始一個頁面之前，需要去抽像一下可複用的單元，然後用sass表達出來。不過我感覺如果你採用的是正確的工作流(面向語義)，這些事情都是比較順理成章的。

對於團隊開發如何確保面向語義，這確實是一個比較難解決的問題。除了需要提供一個清晰的團隊sass庫說明之外，貌似只能通過一些工程化手段解決了(加強code review流程、制定規範等等)。

### grid system 是個很棒的東西耶！

我承認 grid system 的確是個非常好用的模式，在實際場景也會遇到 layout 無法用語義化來表示的問題，但根據語義化的定義，為了寫出更好看的 HTML 跟易讀性，我們或許總有一天還是要把 grid system 拔掉。這會是個大工程，但在目前主站說大不大，說小不小的架構上，還是越早開始越好吧！

- 使用 susy
- @include @extend

## Page Visibility API

能夠知道目前 user 是否 focus 在這個 page 上。
在很多情境下，我們希望當使用者沒有聚焦在這個網頁上（可能跳去別的應用程式、切換分頁時），可以盡量減少不必的請求或操作。facebook 好像也是當你回到網頁時才會有訊息的提示聲音。
比較常見的情景是播放影片時，如果使用者跳出頁面，我們可以先自動讓影片停止，等使用者回來之後，影片在繼續播放。

### 回頭看看 js 的事件傳播

最近把犀牛書拿回來翻了翻，主要是為了釐清以前不是那麼明白的觀念。太多 js library 充斥在網路，我們是否已經忘記原生的 js 了？雖然高度的抽象化是科技趨於發達後一定會有的現象，但是了解一下內部的運作也是好的，對於之後寫 code 也會比較有概念！

#### js 事件傳播

js 的事件傳播主要分為兩大類型，`bubble` 跟 `capture`，大部分的傳播方式是用 `bubble`。什麼是 `bubble` 呢？在註冊對象元素的事件處理器被調用之後，事件就會開始往上漂浮（不包含某些元素的特定事件），然後再調用註冊於祖父元素上的處理器。這種現象會上升到 `document` 最後到達 `window`。

在實際應用上，我們時常看到：
```js
$(".abc").on('click', e => {
	
});

$(".ass").on('click', e => {
	
});

$(".asass").on('click', e => {
	
});

```

散亂的事件註冊在角落，不僅維護上困難，在尋找 code 的時候也沒有統一入口，非常難以除錯。於是我們可以利用 js 事件的冒泡特性，為 document 統一註冊事件。jQuery 的 on 第二個函數便提供了事件委託的功能，如下：

```js
$('document').on('click','.sass', e => {
 
});
```

這樣的好處不僅減少了散亂的事件註冊，還統一了入口。如果要新增事件，只要在 document 統一做擴充即可。我們還可以在 html 這樣寫：

```html
	<a  class="js_action" data-action="foo">
	<a  class="js_action" data-action="bar">
```

```js
  var actionList = {
  	foo: function(),
  	bar: function()
	}
	$('document').on('click','.js_action', e => {
		if(typeof e.target.dataset.action === 'function'){
			actionList[e.target.dataset.action]
		}
	})
```

像這樣，以後如果要增加新的 event handlder 只要在 acionList 新增就好，甚至搭配 extend 的方式，可以不用寫在 actionList 也沒關係。充分提高了擴充性


那麼，為什麼那麼少人使用 capture 呢？最大的原因在於因為可愛的 IE 無法使用。再來事件捕捉只適用於 addEventListener 的方式。
事件捕捉有點像是反過來，會先從祖父開始，依序向下船，直到**事件父元素的事件處理被被調用為止**，註冊於事件元素的事件處理器永遠不會被調用。

#### 事件取消

我們也常常看到類似 `e.preventDefault()` 的方法。其實像是舊版的瀏覽器並不支援，我們可以用一些比較 tricky 的方式來做取消。

```js
function cancelDefault(event) {
	var event = event || window.event;

	if(event.preventDefault) event.preventDefault()
	if(event.returnValue) event.returnValue = false
	return false
}

```


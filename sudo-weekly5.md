## 發現的新東西

## flow 

第一次聽到，是從 henry 的口中。第二次看到，是從看了 draftjs 的 source code。

當前端的 code 無可避免地增肥之後（尤其是 js 跟 css)，我們也應該尋找更有效的代碼管理方式。像是逐漸引入的 eslinter 跟 stylelint 都是。

而 flow 給我的感覺有點像 Typescript，但優點是你可以完全用寫 js 的方式，也不需要改變副檔名（typescript 要寫成 a.ts)。不過還是會害怕抽象化滲透，所以一樣會先觀望一段陣子再套用到主站上吧。

## draft

假日閒來無事、天氣燥熱，在咖啡廳喝咖啡、聽音樂、寫 code 也蠻愜意的。所以就趁著假日的時光研究了一下科科所說的[draft-js](https://facebook.github.io/draft-js/)

寫了兩天，只有一個字，爽！
所有的骯髒事（selection 兼容、檔案拖曳、編輯器內的樣式）draft-js 幾乎都幫你搞定了。當然因為這個 open source 還非常新（react conf 的時候才推出來），有時候噴錯也不知道從哪邊找起。但這的確是值得令人關注的 editor 解決方案。

### 目前編輯器會遇到的問題

#### 1. input 跟 textarea 是有極限的

- 他們只能渲染 plain text，如果想要對文字做加粗、斜體、標記等動作，需要花很大的功實作
- 當你的文字輸入到下一行時，textarea 高度不足不會自動幫你移位。你可能需要計算下一行文字的位置，然後做相關的操作。這也很費工，而且對使用者的體驗非常糟糕。

#### 2. [contenteditable is terrible](https://medium.engineering/why-contenteditable-is-terrible-122d8a40e480#.7cqtr19fk)

因為是將文字內容放在非 input 的 tag 裡面，每種瀏覽器的實作都不太一樣，也很容易就會有意想不到的問題發生。不過 contenteditable 的好處是他活得夠久，瀏覽器也都幾乎實作了，所以反而可以利用這樣的特性來解決當前編輯器遇到的問題。


draft 的概念其實也跟 react 很像，將 editor 跟 content 的狀態存放起來，而非依賴在 DOM 上面。而且他們的狀態不是用純 string 去儲存，而是用 Record(他們實作的資料結構)來代表每個節點。

而 API 文件也寫得挺清楚的，同時也完全實作 immutable 的概念。每次對編輯的內容做操作，就是完全更新狀態。剛開始會覺得很麻煩，我明明只是要一段 text，還要呼叫 getCurrentContent() 回傳的竟然還是一個 `Map` 在從裡面呼叫 getText()；或者我想要更動狀態時，還要做類似這樣的操作：`RichUtils.toggleInlineStyle(curState,'BOLD')`，然後再把新的 state 傳進去。看似麻煩，不過這樣的方式到後期的開發顯然是有好處的。
容易測試，易讀性高，容易修改、除錯。

facebook 也提供了蠻實用的[範例](https://github.com/facebook/draft-js) 看完之後可以更瞭解 draft 到底是怎樣運作的。

這邊是我看了官方文件之後實作的範例：[draft-example](https://github.com/kjj6198/redux-setup) 。還不算太完整，之後會再慢慢補上。

**reference**

[a guide to draftJS](https://medium.com/@adrianli/a-beginner-s-guide-to-draft-js-d1823f58d8cc#.3apelia5r)

## 重新思考 rails helper

在逐漸重構 css 跟 html 的過程中，常常會忘記 helper 的存在，最近寫一寫才恍然大悟，對啊！我不是有 helper 嗎？目前對於比較複雜的邏輯判斷已經會習慣性地去使用 helper，但在 view 裡頭，有沒有 helper 可以幫上忙的可能性？

雖然用 partial 也可以做到類似的事情，但跟 helper 做搭配會更強大啊！

而且因為 partial 有時候還要去對應路徑，而 helper 則是全域都可以使用。對高度重複的頁面來說或許用 helper 會更加簡單！

**例如我們很常用到的 tags。**

當初 sudo tags 改版的時候花了不少時間去更動，因為幾乎每個 tag 裡面都重寫了一次 each，class 命名有時候也不盡相同。

不過使用 partial 又好像太小題大作，畢竟 tag 的架構並沒有如此複雜。

```ruby
def tag_of_tags(tags, options = {}, html_options)

  raw tags.collect { |tag| content_tag(:span,tag.name, options, html_options)}
end
```

這樣一來，每次我需要用到 tag 的時候就只要

```html
<%= tag_of_tags(@post.tags) %>
```

當然也可以做一些調整，例如加入 namespace 來符合我們的使用情境。
```ruby
def tag_of_tags(tags, namespace,options = {}, html_options)

  raw tags.collect { |tag| content_tag(:span,tag.name, :class => namespace)
end
```

```html
<%= tag_of_tags(@post.tags, "sudo") %>
```
這樣一來每次 tag 要更新的時候，我只要安心修改 helper，其他地方全部都會生效。寫起來自然輕鬆！

以上只是一小部分的例子，舉凡 `navbar` `button` `dropdown` 等，幾乎是每個網站開發必備的元素。我們在新增 partial 的同時，說不定也可以搭配 helper 一起思考更多的可能性！

## 關於文件，我有話要說

最近因為想要讓 legacy 漸漸減少，會盡量地用重用性的角度來思考。

不過科科給了我一句話「太早談重用性，反而會掉入過度設計的陷阱」。仔細想過之後，的確如此吧！

但我認為，這樣的嘗試是好的，對自己來講是從更寬的角度來看自己的 code，也可以減少一些自己以前的盲點等等。

而想要讓 code review 變得輕鬆的辦法，就是寫文件。不管對 senior 也好對 junior 也好都是有幫助。雖然我的 code 現階段還沒有達到很高的水準，可是透過寫文件這件事情，code reviewer 可以快速理解我的想法，為什麼我要這麼做。

而不是讓 reviewer 從頭看我的 code，揣測我到底在幹嘛？像是 modal 對我來說就是一個很好的嘗試，科科很快地就指出其中的問題跟盲點。

這樣一來，對我來說，我可以從自己的想法裡面寫出品質更好的 code；而 reviewer 因為容易理解我的想法，給予 feedback 的意願也就相對提高了。

當然，可能目前寫得還不夠多，想法也還不夠成熟也說不定，但是從這樣的練習過程中，我想也會慢慢進步吧！

這是我為什麼很用心在寫文件的原因，因為對彼此來講都是節省時間。我省掉以後再看 code 時回想的時間，而 code reviewer 則是省下從頭開始理解的時間。

目前還在磨合期中，希望科科也可以提供建議！

### reference

[good commit message](https://speakerdeck.com/yinghau76/the-elements-of-good-commit-messages?utm_campaign=CodeTengu&utm_medium=email&utm_source=CodeTengu_44)

## javascipt test

[how to write test in existing javascript](https://rmurphey.com/blog/2014/07/13/unit-tests)

很多人都是從前任接手 code 的，這篇文章教你如何從現存的 code 當中進行 refactor。裡頭有很多想法值得一提：

- 不要怪罪前任，要知道代碼的由來通常是有歷史的（每個人都會寫爛 code）
- 如果沒有 unit test 也沒關係，至少從自己做起
- 用消極的態度、因為情緒而過度設計，反而會造成更多問題
- 永遠不要情緒化寫 code
- **don't use !important with anger**

目前也正在找尋適合主站的測試工具中，希望有一天我們可以達到超過...百分之五十的測試率。

## [日本工作三個月](http://wangyung.blogspot.tw/2016/05/blog-post.html?utm_content=bufferd71fd&utm_medium=social&utm_source=facebook.com&utm_campaign=buffer) 

很棒的工作分享！


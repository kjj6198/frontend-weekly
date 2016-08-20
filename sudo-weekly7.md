## 發現的新東西

### [別跟我說你懂 margin](http://www.hicss.net/do-not-tell-me-you-understand-margin/)

拿捏 margin 跟 padding 可以很容易地看出前端工程師對 CSS 的掌握程度。說真的超多眉眉角角的，不過看完文章發現原來 margin 的妙用這麼多。以後的實踐或許有比較容易的方法也說不定！

### [MutationObserver](http://javascript.ruanyifeng.com/dom/mutationobserver.html)

這個鮮為人知的 API，被喜歡翻 MSDN 的我挖掘到了，基本上已經支持目前主流的瀏覽器（IE11+)。

看完介紹會發現他很像事件，專門給 DOM 的變化使用的，不過不同的是，一般的事件是同步的，而 MutationObserver 是**非同步**的。

這可以幫助我們在 DOM 有變化的時候，做出相對應的反應。

利用這個特性，我們可以達到以下幾個應用：
1. lazy loading
2. initialize

因為在 HTML 實際被 render 之前，DOM 就已經先下載下來了，所以我們可以在 rendering 之前就改變 DOM 的內容。

```js
var observer =  new  MutationObserver ( function ( mutations ){ 
  for  ( var i = 0 ; i < mutations . length ; i ++){ 
    for  ( var j = 0 ; j < mutations [ i ]. addedNodes . length ; j ++ ){ 
      lazyLoad ( mutations [ i ]. addedNodes [ j ]); 
    } 
  } 
});

observer . observe ( document . documentElement ,  { 
  childList :  true , 
  subtree :  true 
});
```

### [astrum](http://astrum.nodividestudio.com/)

這是一個很炫砲的 style guide library。做得非常精緻。
最近像這些 styleguide 的 library 一直推陳出新，其實就像潘大所說的。寫文件是好的，但是一定要評估成本、其他人的熟悉成本、要建構的時間等等，不能只是因為又潮又炫就自己跳下去自幹。而是要讓寫文件這件事情逐漸被引入。

不過當作 side project 或是自己想玩玩看就另當別論了。

### 新的問題

每個框架都是為了解決某個問題而出現的。但使用了某個框架之後，好像逐漸又會萌生新的問題。我把一些自己在開發上常會碰到的問題整理了一下：

1. 文件切換

我覺得很麻煩的一點就是要一直在 component 之間跳來跳去，有時候還要切過去看看 action, reducer 之類的，久了很容易阻礙思考。

2. Webpack 打包過慢

要 bundle 的檔案越來越多，webpack 逐漸變得沒有那麼快了。每次寫完 code 還要等他 bundle，而且每次更新都會 reload 一次，說真的還挺麻煩的。

### SudoDashboard

其實只是想練習 dashboard 的 UI 跟 Websocket 的實作。不過感覺做起來還蠻實用的，最近空閒的時候應該會開始組織一下。還請潘大多多擔任設計指導。


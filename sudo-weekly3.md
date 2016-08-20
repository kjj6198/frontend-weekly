這玩意已經困擾我很久了，如果牽扯到滿版的圖片要怎麼切 layout 會比較好？總算有一個比較優雅的解法。

## 問題是什麼？

在 HTML 中，我們通常會用一個具有 `.container` 的 `class` 來包覆主要內容。而 container 的 css 大概會長得像這樣：

```css.container { width: 65%; margin-right: auto; margin-left: auto; }```

所以，如果在 container 中有圖片，最寬也只能到 container 的寬度，而無法佔滿整個螢幕。如果使用 positioning 的技巧，雖然很容易就線

```html<div class="container"> <p> 發有工功們格己同。一沒子消找外兒師生！文師百家治臺眼作大點清建率清中其時，演色如文？ 文次年落……好其我走不中星壓大長賽！充得此接好長樣各心像：大作識原賽。乎福長致半計小離如開經曾。 對名花可來家用冷們好方同館北代其如念經相驗海人地內到醫百不包分報上發血能心當頭怎非； 關不詩元比與樹！作可年開世，社心正公留是有斯。 </p> <img src="./imageA.jpg" alt=""></div>```

## 善用負 margin 的技巧

如果熟悉 bootstrap 的話，可以發現他的 grid 撰寫方式是以 padding 當作 gutter，在 row 裡加入負 margin 跟 container 的 padding 消除。這樣的好處是 row 跟 column 在做搭配的時候可以更有彈性，也不會有 padding 跟 margin 結合的 bug 出現。

所以，圖片也可以採用這樣的方式來實作滿版圖片的呈現。利用 負 margin 的技巧把圖片拉出 container 外。

那要怎麼拉？如果還沒有負 margin 相關概念，建議各位可以先看看這篇

* [別跟我說你懂 margin](http://www.hicss.net/do-not-tell-me-you-understand-margin/)
* [我知道你不知道的負 margin](http://www.hicss.net/i-know-you-do-not-know-the-negative-margin/)

通常 container 的寬度會是以 %數宣告，下列的範例用寬度 80% 來做介紹如果在 container 的寬度設定了 80%，而 margin 使用 auto 的話，左右會有 10% 的 margin。如果要把子元素拉過去的話，需要 10% 加上 2.5% 的父元素寬度。


```css  .full-width-image {     margin-left: -12.5%;     margin-right: -12.5%;  }
```
這樣一來就可以簡單實作出滿版的圖片。但馬上就可以發現，這樣子的 class 依賴於父元素的寬度，如果今天寬度不是用 %數決定，或是用固定寬度來決定的話，這個 class 馬上就破功了。

### 不需要知道父元素寬度的方法

不需要知道父元素的寬度，代表我們使用負 margin 的時候來 pull 子元素的時候，要直接依照寬度來決定，最方便的方式就是使用 vw 屬性。先將圖片寬度設定為 `100vw`，並且使用 positioning 的方式置中，最後再使用負margin 拉回來。

三個步驟如下：

1. 設置寬度為 100vw
2. position: relative, left: 50%
3. margin-left: -50%;

```css
.full-width-image { position: relative; left: 50%; width: 100vw; margin-left: -50%;}
```

## 結論

這篇文章介紹了兩種使用負 margin 來實作滿版圖片的技巧。有些前端工程師都聽過負 margin 這個名詞，卻都只是看過幾篇文章或理論，還沒有真正實作過。

但負 margin 的原理其實不難，了解之後甚至可以實現很多原本很難呈現的 layout。

### reference

* [別跟我說你懂 margin](http://www.hicss.net/do-not-tell-me-you-understand-margin/)* [我知道你不知道的負 margin](http://www.hicss.net/i-know-you-do-not-know-the-negative-margin/)* [margin 深入理解](http://www.planabc.net/2007/03/18/css_attribute_margin/)

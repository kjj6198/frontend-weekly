## 從 SASS 到 PostCSS

大約在一年前，PostCSS 開始竄紅在前端生態圈裡，不外乎就是所謂的 preprocessor 的特性、高度客製化自己的 plugin、搶先使用 cssnext 的功能，還能夠搭配各種建構工具（gulp, webpack），用起來非常輕鬆寫意。

網路上也不乏各種比較 SASS 跟 PostCSS 的文章，最常聽見的說法不外乎有下列這幾種：

* PostCSS 的 plugin 使用 javascipt 撰寫，能夠用 JS 控制 CSS，感覺很棒吧！
* SASS 代表一旦你使用它，就要接受他的全部
* 兩者同時使用並不互斥

### 變數

雖然剛開始看到 PostCSS 的時候還挺興奮的，但隨即思考了一下：「真的有必要馬上把 SASS 取代掉嗎？」。PostCSS 的優點在於你能夠選擇你想要的 plugin，需要時再使用就好。舉變數的功能為例好了，[postcss-simple-vars](https://github.com/postcss/postcss-simple-vars) 能夠模擬 SASS 的變數宣告及使用行為。對我來說怎麼看都彆扭，因為 SASS 除了變數本身宣告之外，還有 map, list 的型別，而且具備了相當完整的 API 操作。例如：取值、判斷式、迴圈功能等。

```css
$colors: (
  main: #abc,
  sub: #bac,
  word: #333
);

.container {
  background-color: map-get($colors, $main);
  color: map-get($colors, word);
}

```

就算使用 css spec 的 var 也是一樣，沒有 map 或 list 取值的功能。

```
:root {
  --wordColor: #333;
  --bgColor: #fafafa;
}
body {
  background-color: var(--wordColor);
  color: var(--bgColor);
}
```

（o.s：而且這樣子寫其實有點醜~~很醜~~）

或者，SASS 的 @function 也能夠進一步將 `map-get`做包裝

```css


$colors: (

 main: #abc,

 sub: #bac,

 word: #333

);
/* alias method for getting color from $colors map

/// @param {$key} the key you want to choose

///

/// eg:

 color: c($word); 
*/

@function c($key) {
  @if map-has-key($colors, $key) {
    @return map-get($colors, $key);
  }
  @else {
    @error "Unknown key #{$key}";
  }
}


.container {

 background-color: map-get($colors, $main);

 color: map-get($colors, word);

}
```

因為 PostCSS 生態圈廣泛的原因，不少獨立開發者的插件很可能因為沒有在維護，或是因為疏忽而導致編譯錯誤有小 bug 等等的問題，相對之下 SASS 本身具有的功能相對完整的多。

### mixins 跟 function

相對應的 plugin 有  [postcss-mixins](https://github.com/postcss/postcss-mixins) 跟 [postcss-functions](https://github.com/andyjansson/postcss-functions)

雖然模擬的 mixin 的行為，但如果要搭配判斷式使用的話，又要花一番功夫。

```css
@mixin state($state,$namespace: '') {
  @if ($namespace != ''){
   .#{$namespace}-#{$state} {
     text-transform: uppercase;
   } 
  }
  @else {
    .${state} {
      text-transform: uppercase;
    }
  }
}
```

而 `function` 的部分也一樣，如果使用純 CSS 搭配 PostCSS 撰寫，沒有辦法使用 SASS 原生的 function。雖然能夠用 js 自定義 function，這一點其實還蠻吸引人的，但如果要模擬 SASS 相對應 function 的話，又要在重造一次輪子，不免顯得有些麻煩。

### 相較於 SASS 還不成熟

相對於 SASS 來說，PostCSS 其實還算蠻新的工具，雖然生態圈很廣泛，插件也多，但目前版本仍在快速變動中，也還有很多 issue 沒有解完，SASS 因為本身是使用 Ruby 撰寫，雖然速度會比 PostCSS 慢一些（好吧，應該是很多），但其穩定跟完善的 API 跟型別、語法，都是 PostCSS 還無法達到的程度。

## PostCSS 的優勢

來說說 PostCSS 的優勢吧！目前我最喜歡搭配使用的功能有 autoprefixer cssnano。autoprefixer 能夠幫你處理 CSS 麻煩的前綴，以往是使用 mixins 來解決，現在完全交給 PostCSS 處理就可以了，相對起來乾淨簡潔多了；cssnano 則是幫你處理 CSS minify，搭配 gulp 使用你可以只要安裝 gulp-postcss gulp-cssnano gulp-postcss gulp-sass 就可以進行 css 編譯跟最小化的動作了。

除了上述的插件之外，我認為很棒的插件還有

* `postcss-sorting`：根據定義的規則排序你的 css properties
* `precss`：包含許多 sass-like 的功能
* `stylelint`：lint 你的 CSS
* `stylefmt`：根據 stylelint 的規則幫你 format css code
* `doiuse`：幫你偵測目前 CSS 的瀏覽器支援度

### 為什麼我不敢離開 SASS：

搭配 PostCSS 的確非常方便，但我不認為兩者同時套用能夠減少日常開發，畢竟一旦出錯，就得花時間去研究底下的運行機制。有可能 PostCSS 編譯完之後再給 SASS 編譯會報錯；又或者某個套件的 bug 導致檔案沒有完整編譯，某段的 CSS 代碼沒有生效等等，都是潛在的問題，目前的開發我也只有使用 `autoprefixer` `cssnano` `stylelint` 來幫助簡化、檢查 CSS 而已。

## 結論：

或許最後 SASS 會完全被 PostCSS 打敗也說不定，但 SASS 本身完整、成熟的架構跟語法，還是讓我不想要輕易完全使用 PostCSS 的最大主因，哪天等 PostCSS 成熟到能夠完全獨立於 SASS 的時候，我可能就會轉移過去了吧！就像當初在學 CSS 的時候，也是猶豫了很久才開始學習 SASS 那樣。不過，這兩者（sass, postCSS）是可以共存、互補的。

既然享受了 PostCSS 高度彈性化的優點，我們接下來也不得不關心抽象化滲透，因為插件有可能因為時代的變遷，或是開發者不在維護的關係而出現錯誤。這些插件分別看來功能微小，但是全部加起來之後能夠省下的時間也是很巨大的。；而 SASS 完整的語法、變數系統雖然需要花一段時間學習，卻能夠用統一的語法大幅提升 CSS 維護度。

在這個組件化盛行的年代，前端工程的開發逐漸傾向於維護模組化的檔案（react, css-modules 等），最後可能不需要那麼多繁雜的操作（此指 sass 的 function、變數等），而是回歸到最原始的純 css 也說不定。


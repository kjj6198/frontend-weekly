## 工作內容

1. 公司影片上版
2. 公司靈魂人物製作（動畫切換效果尚未完成）
3. 工作地點上版

## 這禮拜發現的新東西

這個禮拜在思考關於重構的東西，在前端還可以怎麼實踐。css 的部分已經有大致的雛形出來了，把相關的註解跟文件寫完之後再和大家分享心得。

### Get your `../../` out

[reference](http://davidboyne.co.uk/2016/04/29/react-webpack-gem.html)

css 部分重構完之後，再來就是要對 js 下手了。目前比較困擾的幾個點有：

1. component 初期的部分比較亂
2. 有些 flux、有些 redux

再來是我認為最麻煩的，就是路徑管理的問題了，因為檔案裡面有太多類似的 `../..` 出現，導致之後要對檔案做搬遷的時候還要全部重改一次，感覺非常的費工費時，也是讓人不敢輕易重構一大原因。

那時候第一個想法是，「不然我把全部的檔案存成一張路由表好了。」，不是 import 檔案本身，而是每次要 import 時，都到這個路由表去拿路徑，不但方便，而且每次對檔案做搬遷時，我只要維護好這張路由表就了。不過這個想法還沒有實作過，所以可能不是好方法也說不定...。


再來發現的第二個工具是`modulesDirectories`，主要是幫你把 root 定義好，每次呼叫 component 時都是以 root 當作起點而非檔案本身。

以 sudo 的檔案為例

```js
require('../ui/comment_reply.js');
import h from '../utils/sudo_helper/dom.js';
const actionList = {
	reply: 'reply',
	edit: 'edit'
};

import jobsIndex from '../reducers/pages/JobsIndex.js';
import App from '../containers/jobs_index.js';
import configureStore from '../stores/JobSearchStore.js';

var Checkbox = require('../components/checkbox');
var Slider = require('../components/slider.js');
var TextInput = require('../components/text_input.js');
```
 加入 `modulesDirectories` 之後，我們可以這樣寫：

```js
require('ui/comment_reply.js');
import h from 'utils/sudo_helper/dom.js'

import jobIndex from 'reducers/pages/JobIndex';
import App from 'containers/jobs_index';
import configureStore from 'stores/JobSearchStore';
```

這樣 component 搬遷的時候，不會因為相對路徑改變的關係，而還要全部重寫一次，重構的意願也相對提高了。

### [idiomatic-css](https://github.com/necolas/idiomatic-css/tree/master/translations/zh-TW)

最近一直在研究如何讓 css 的易讀性更高，並且更好做維護，目前如果搭配 webpack 的話，常見的方法有：

1. css-module
2. postCss
3. radium

這些工具搭配 react 一起使用，其實可以很有效地解決 namespace 的問題。

不過主站目前想要先解決的是變數的統一命名，及 css 的規範。這部分規範好了之後，css 的管理相對也會變得簡單許多。

### [code smell in css](http://csswizardry.com/2012/11/code-smells-in-css/)

這篇文章介紹了很多關於 css 的 `smell code`。幾個比較重要的大概念：

1. 避免 magic number
2. 盡量讓 class 是 immutable
3. 利用權重的技巧，而不是過度使用 important
4. selector 盡量簡單

順便來複習一下 [selector](http://www.w3schools.com/cssref/css_selectors.asp) 的語法吧！除了用 class 來當做 selector 之外還有很多其他實用的 selector 可以使用！
像是 input 就是一個非常好使用 attribute selector 的例子：

```css
.sudo-input {
  input[type="text"] {
    // text style
  }

  input[type="submit"] {
    // submit style
  }

  input[type="number"] {
   // ...
  }
}

```
這樣不僅語意清楚，也可以省下不必要的 class。

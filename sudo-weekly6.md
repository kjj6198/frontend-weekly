## 發現的新東西

### 瀏覽器的字體渲染

前陣子發現了 `font-smoothing` 的屬性，雖然 w3c 已經把他從標準刪除，但在 chrome 上面還是可以使用這個屬性。

但令人好奇的是，其他瀏覽器的字體渲染方式呢？

其實字型渲染是一個一直以來容易被忽略，卻又是極為重要的議題。因為網站的易讀性除了 layout 之外，再來就是字體了。

為了確認網站在瀏覽器的一致性，稍微研究了一下字體渲染的方式。
分享在這邊給大家做參考。

每個字，在電腦當中都會被當作一個向量圖像。而當我們把字體渲染在螢幕上的時候，就會把字型用像素渲染的方式呈現在螢幕上，所以每個字其實都是由一個個小方塊組合起來的。

但問題發生了，用小方塊的壞處就是在邊緣會出現無可避免的鋸齒狀，造成閱讀體驗不佳。

## 基本解法 grayscale

這個解法是讓邊緣的像素補上一些灰階的像素，讓字型遠遠看起來比較圓滑，用這些灰階的像素來補償字體本來應該有的面積。這個方法很聰明又優雅，讓人不禁佩服。

### 進階解法 subpixel

顏色信息，如果我們把屏幕截圖不斷放大，可以看到字體邊緣有紅藍兩色出現，這就是亞像素渲染了。

在LCD屏中，一個像素是由紅綠藍三個緊密排列的亞像素構成的，它們決定了這一像素的顏色和亮度。由於它們是如此之小，以至於肉眼不會把它們看作是一個個獨立的色點。如果我們仔細看看上圖中被白點標記的「紅色」像素，就可以發現它所採用的渲染策略：所有的亞像素都可以單獨控制開或關的；若「空白」像素最右側的亞像素是紅色的話，則此像素都將填滿紅色。

### 問題點：百家爭鳴的瀏覽器

每個瀏覽器實作字型渲染的方式都不盡相同，甚至不同的 OS 下也是。更蛋疼的是，在Windows下還可能採用兩種技術來渲染—— GDI或者DirectWrite。

Windows
在Windows系統下，字體格式對其渲染效果有很顯著的影響，比如PostScript字體和TrueType字體之間就存在著巨大的差別。但這種差別並不是由瀏覽器所引起的，只要底層的字體一樣，我們就可以看到完全相同的渲染效果。

儘管這種方法並不十分可靠，但從字體的命名中我們可以大致推斷該字體所採用的渲染技術，比如，EOT和.ttf格式一定是TrueType技術，反之.otf通常是PostScript技術。但是還有一中封裝的字體格式WOFF，它可以包含其中任意一種字體格式。因此光看文件名 ​​是不可能清楚它所採用的渲染技術的。除了EOT​​和.ttf格式文件可以斷定是TrueType渲染技術外，其他文件格式所包含的是哪種字體都無法確定。因此在你購買字體時，你最好對想要購買的字體做一番了解。（@Ryekee :我覺得這一句根本不用翻譯，中國還有人會買字體麼？）

TrueType和PostScript的區別在於描繪曲線時所採用的數學方法不同，但這一差異對柵格器並不會造成太大的影響，只有字型設計人員才需要考慮著兩者的差別。另一個重要的區別就是所採用的字體微調的方法。PostScript只包含了組成字體的各種元素的抽象位置信息，而TrueType則包含了非常詳細的底層命令，直接接管了渲染的進程。然而造成兩種渲染技術的差異並不是它們的設計理念上的差別，而是源於Micro$oft採對TrueType採用了新的渲染引擎。

操作系统OS提供了支持不同的字体渲染方法的API。在windows下是GDI(Graphics Device Interface)和DirectWrite，OS X下是Quartz。

GDI分为GDI Grayscale和GDI ClearType。前者为灰阶渲染API，后者是亚像素渲染API。由于GDI ClearType并未对字体进行垂直方向的平滑，因此当字体较大时会出现边缘不平滑的情况。为了弥补GDI ClearType的不足，MS实现了DirectWrite API，它在GDI ClearType的基础上增加了垂直方向的平滑。
但是！字体渲染的API都是由浏览器厂商自己选择的！

使用同一颜色，感官上的颜色深浅为：黑白渲染>grayscale>sub-pixel。

Chrome35/36采用的是GDI ClearType，因此在字体较大时边缘会出现毛刺，而FF30采用的DirectWrite则没有此类问题。如下图所示：

不過最近看了一下 windows 的字型渲染，不得不說微軟在這方面還真是下了不少功夫。

越來越多的字體設計師都開始注意到Web字體所帶來的技術問題，尤其是TrueType字體的微調。隨著Web字體產業的崛起，他們願意付出精力為屏幕顯示而優化字體。在不遠的將來，我們將看到大量精心設計的字體問世（或者至少是對現有字體的更新）。

隨著屏幕分辨率的增加（以及對柵格器的重大改進），我們慢慢地不再擔心字體渲染的技術細節。採用GDI渲染模式的瀏覽器必將拖後腿，正因為此，未來數年內，我們都還無法放心的使用無微調的TrueType字體。只有當這一類瀏覽器用戶比例降到足夠低的程度的時候，TrueType字體微調（耗時又需要高超的技巧）才可以被扔到一邊。儘管目前市面上幾乎所有Web字體都是TrueType格式的，我仍希望字體行業能夠大規模轉向PostScript格式，因為這種字體能為設計師減少絕大部分的工作。

不過好處是，科技日新月異。希望之後不用再擔心字型渲染的東西。

MAC 使用他們自己的引擎來渲染字體，不管 truetype 跟 posttype 渲染方式都一樣。這個渲染引擎只有一個字，屌。

truetype 跟 posttype 的差別。

### 結論

如果想直接看結果，在網頁的時候，可以使用反鋸齒的技術來增加易讀性。
但如果在手機上建議關閉，因為反鋸齒的演算需要比較多的 GPU 來做計算。通常直接採用灰階的渲染方式就夠用了！

### 事件控制

如果可以，希望在每個檔案都能夠新增一個 EventManager 的方式，統一管理事件。我很喜歡事件委託的方式。

善用 data attribute 的方式來給標籤一些好用的屬性。

## [microdata](http://lepture.com/zh/2015/fe-microdata)


## Rails view

最近希望把一些常用的 tag 跟撰寫方式拆成 helper，於是開始去研究 rails 裡面的 view helper method。發現裡面的 helper 撰寫大有學問，這邊跟大家分享：

```ruby 
def link_to(name = nil, options = nil, html_options = nil, &block)
  html_options, options, name = options, name, block if block_given?
  options ||= {}

  html_options = convert_options_to_data_attributes(options, html_options)

  url = url_for(options)
  html_options['href'] ||= url

  content_tag(:a, name || url, html_options, &block)
end
```

這邊用 `link_to` 方法舉例，可以看到這邊分成了兩種方式，如果有給定 `block` 的話，會將傳入的參數作轉換，如果沒有的話，則是將 `option` 做處理之後，傳給 	`content_tag` 這個方法。注意到這邊的 `block_given?` 方法，有這個方法我們就可以很容易的判斷是否有傳入 block。

所以在設計 helper 的時候，可以適時包裝這些方法，簡化 helper 的複雜度：

```ruby
  def link_to_with_noopener(name = nil, options = {}, html_options = nil, &block)
  	html_options.merge!({
  		:ref => "noopener"
  	})
		if block_given?
		  link_to(name, options, html_options, &block)
		else
		  link_to(name, options, html_options)
		end
  end
```
請參考：[About rel=noopener](https://mathiasbynens.github.io/rel-noopener/)

再來就是 viewhelper 裡面有很多還蠻好用的方法，我們不用再花很大的功夫重造輪子，像是：

* [link_to_if](http://apidock.com/rails/v4.2.1/ActionView/Helpers/UrlHelper/link_to_if)
* [link_to_unless](http://apidock.com/rails/v4.2.1/ActionView/Helpers/UrlHelper/link_to_unless)
* [link_to_unless_current](http://apidock.com/rails/v4.2.1/ActionView/Helpers/UrlHelper/link_to_unless_current)

不過每次只要一動到 helper 就很容易出包...，但直接用原生的 HTML 不但很醜，又不好維護，所以在撰寫 helper 的時候，為了保險還是寫一下 test 吧！

最近的有個想法是將常用的片段程式碼拆成 partial，**並且用比較統一的方式管理！**，每次接到新的頁面就要先思考哪些會是局部代碼，哪些會是共用代碼。

### partial 設計的幾個要點：

經過這次標籤消失的事件，首先先跟顆顆說聲道歉之外，更應該檢討的是以下幾點：

1. 預設值很重要！
2. if else 判斷式要思考更全面
3. 如果害怕沒有預設狀況，就乾脆讓他噴例外

第三點的實作方法如下：

```ruby
# in component_helper.rb
# ui_component 方法是對 render 的進一步包裝。
def ui_component(url, props = {})
  render "components/#{url}", locals: props
end

# in error_helper.rb
def check_required_options!(locals, *options)
    options.each do |option|
      raise MissingOptionError, %Q{option "#{option.to_s}" is required.} if locals[option].nil?
    end
  end


  private

  class MissingOptionError < StandardError
  end
```

然後在 partial 的程式碼裡面：

```html
// in _tags.html.erb
<% check_required_options locals, :propA, :propsB, :propsC %>
```

這樣如果 locals 裡面的 option 不夠完整，就會在使用的時候噴出例外，不知道這樣的設計夠不夠完整。而且 partial 設計的時候很容易因為時間久的關係，忘記了 locals 裡面的選項，在 partial 裡面養成檢查 locals 的習慣，可以幫助我們補上遺落的 option。不知道這樣的設計夠不夠恰當跟完整？還請顆顆提出意見。

### component 化

這邊的 component 化並不是指全部使用 react 改寫，畢竟我們的頁面也還沒有到這種需求。但是既然 rails 有強大的 partial 機制跟 view heleper 方法，我們可以在撰寫的時候做更進一步的拆分：

```html
<div id="page_id">
  <header>
    <%= load_component("company_header") %>
  </header>
  <main>
    <aside>
    	<%= load_component("company_sidebar__header") %>
	    <%= load_component("company_sidebar__content") %>
    </aside>
	  <%= load_component("company_summary") %>
    <%= load_component("company_introduction") %>
		<%= load_component("company_section") %>
  </main>
  <footer>
  	<%= load_component("company_footer") %>
  </footer>
</div>
```

但其實目前主站的頁面都仍時常變動，所以過早的抽象化反而會浪費更多成本吧！不過未來如果能用這樣的方式來撰寫程式碼，相信應該是非常好維護的！

## Webstrom

其實以前也是那種很瞧不起 IDE 的那一派，開起來要花我好幾秒，重點是又吃記憶體，然後又要常常使用滑鼠操作...。但使用 IDE 來幫助我們 debug、重構是一件非常省力又輕鬆的事情，只要右鍵 => refacter 即可。

所以如果是一般的功能開發，如果牽扯到比較多檔案的 CSS 或是 JS 檔案的話，我會使用 Webstorm 來開發，搭配 react 跟 jsx 的 plugin，不用每次都會忘記 action 的名字在不同檔案跳來跳去，或是突然忘記長長的 API 又跑去找文件，要整理 code 也只要按下 refactor 就好，出錯的機率自然會降低、也節省了不必要的時間。

但如果是剛入門的人還是建議從文字編輯器開始，門檻比較低。不然設定 IDE 的時間，都可以吃好幾碗肉骨茶麵了。

## CSS @supports

這個屬性
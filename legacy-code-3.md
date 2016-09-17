# 從 legacy code 中尋找出口（中）- HTML 篇

講完 CSS 的重構技巧之後，接下來會專注在 HTML 的重構技巧上。本系列的文章將不會談論 js 的重構部分，因為牽扯到較多的程式撰寫技巧，而且網路上類似的文章應該是不勝枚舉。

## 前言

其實 HTML 能夠重構的點並不多，主要就是標籤的正確使用以及顧慮 accessibility 等小細節。所以這篇文章會著重在如何使用正確的標籤以及語義化；基本的 accessiblity 認識跟 aria 標籤的使用，不僅對 screen reader 較友善，語義化的標籤使用也能夠在之後修改 HTML 時更有效率；最後是模板語言的技巧應用，這邊會用 rails 的 views 為例，不過概念的部分應該是相通的。

---

### 語義化

首先，我們先來看看 HTML5 新增的 tag 有哪些

#### section

* 用來表示網頁裡的一個**段落**，常見的誤解是用來表示一篇文章。
* 基本上跟 div 沒有太大的差別，只有語義化的表現。

#### article

* 用來表示一篇**文章**

#### datalist

#### dl dt dd

* 如果資訊是以條列式來呈現，可以使用 dt, dd 來區分。
  > Definition lists, created using the DL element, generally consist of a series of term\/definition pairs \(although definition lists may have other applications\). Thus, when advertising a product, one might use a definition list:


```html
<dl class="information">
  <dt>薪水：</dt>
  <dd>100000 ~ 300000<dd>
  <dt>工作地點：</dt>
  <dd>台北市</dd>
</dl>
```

任何以成對方式呈現的資訊都可以使用 dt, dd 包裝，像是範例中的：薪水搭配 100000 ~ 300000、工作地點搭配台北市。不過要注意的一點是 dl 預設會將內容縮排（就跟 ul, ol 一樣）。可以將 padding 設置為 0 來解決。

#### figure figcaption

* 使用在和主內容相關的圖片、代碼，或其他資訊，不一定只能放圖而已

* 搭配 figcaption 來定義標題


#### legend

* 搭配 fieldset 使用，用來表示輸入表單的內容
* 預設的邊框很醜，如果要使用這樣的結構可能要重置一下 CSS

```html
<form>
  <fieldset>
    <legend>信用卡資訊</legend>
    <label>卡號</label>
    <input type="tel" />
    <label>到期日</label>
    <input type="date" />
  </fieldset>
</form>
```

族繁不及備載，tag 的部分就在此告一段落。我個人覺得有一大部分是因為**不知道**才沒有使用這些 tag 的，詳情可以到[w3c](http://www.w3school.com.cn/tags/tag_legend.asp) 的網站看看。之後會再將其他 tag 的使用方式補上。

#### 如果都沒辦法滿足怎麼辦？

如果以上的 tag 都沒有辦法滿足你的需求，最常見的做法就是用純 div。而 HTML 為了因應這樣的需求，也提出的 ARIA 的 spec，讓開發者可以更容易製作客製化的元件。如果我們想要使用 div 來模擬按鈕的行為，除了在 class 加上 `btn` 之外，我們同時可以加上 role attribute，並且使用相對應的 aria-\* 標籤來做對應的狀態處理（是否按下、disabled、haspopup 等等），除了更加語義化之外，這些看起來有點麻煩的細節，都會大幅地改善使用 screen reader 的體驗。

但是，盡量不要取代原本語義化的標籤的含義，例如將 button 標籤加入 `role="modal"`  之後當作 modal 使用。

> Web developers _must not_ use the ARIA `role` and `aria-*` attributes in a manner that conflicts with the semantics described in the [Document conformance requirements for use of ARIA attributes in HTML](https://www.w3.org/TR/html-aria/#docconformance)table. Web developers _should not_ set the ARIA `role` and `aria-*` attributes to values that match the default implicit ARIA semantics defined in the table. - [w3c spec](https://www.w3.org/TR/html-aria/#rules-wd)

當然，雖然 spec 這麼說，但某些 tag 像是 `input + label` 的 hack 對前端開發來講真的很方便。這部分或許就看大家的取捨了。

```html
<div class="btn" role="button" aria-disabled="false">Click to Signup</div>
```

### Accessibility

accessibility 其實可以另外再寫一篇文章來討論，但因為不再本篇的討論範圍內，這邊只列幾點關於 accessibility 的注意要點。

* 文字的大小
* 顏色的對比
* 是否加入適當的 attr。（img 的 alt, link 的 title 等）
* 適當的 aria-\* 使用

如果對 accessility 有興趣，建議大家看看：

- [ARIA](https://www.w3.org/TR/html-aria/) w3c 的 aria 標準。
- [WebAIM checklist](http://webaim.org/standards/wcag/checklist) WCAG 的 checklist
- [WebAIM inviblecontent](http://webaim.org/techniques/css/invisiblecontent/) 關於 CSS invisible content 的介紹

### 模板語言（已 rails views 為例）

目前市面上有很多模板引擎，像是 `ejs` `erb` `handlebar` `jade` 等等，都有一套特定的語法、partial、客製化的函數等等幫助你簡化冗長的 HTML。

就 rails views 來說

```html
<%= render 'partial/buttons/primary', locals: {
  :name => 'foo'
  :title => 'bar'
} %>
```

將常用的元件拆成 partial，並且放在對應的資料夾（我的習慣是將所有 partial 檔統一資料夾管理，除非那個 partial 只有在某一個特定頁面使用）。這樣一來就可以很容易地引入已經撰寫好的 HTML 架構。

```html
<%= render 'partial/modal', locals: {
 :name => 'foo',
 :title => 'bar',
 :image => image_path('a.png')
} %>
```

## 結論

HTML 的重構比起 CSS 跟 JS，算是簡單許多（只要 css 撰寫的時候不用 tag 做 styling），不過從頭看下來，要顧慮的事情也不少。正確的使用 tag 以及顧慮 accessibility，能夠有效地增加 UX，同時自己在修改的時候也不會看到一大堆 div 看到心癢癢。


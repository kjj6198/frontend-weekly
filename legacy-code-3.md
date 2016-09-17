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

如果以上的 tag 都沒有辦法滿足你的需求，最常見的做法就是用純 div。而 HTML 為了因應這樣的需求，也提出的 ARIA 的 spec，讓開發者可以更容易製作客製化的元件。例如，我們想要使用 div 來模擬按鈕的行為，除了在 class 加上 `btn` 之外，我們同時可以加上：

```html
<div class="btn" role="button" aria-disabled="false">Click to Signup</div>
```



### Accessibility

### 模板語言（已 rails views 為例）


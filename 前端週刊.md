這次介紹一下我建置 style guide 的過程以及心得。

## 工作內容

1. styleguide 建置
2. 新的 `ws_job_index` search bar

## Styleguide 真的很重要

在建置的過程中，我們常常會忽略，「如果是別人，該如何快速懂得文件的內容」。
所以在上以前的 view 當中，我其實都不太清楚。「啊！原來這個已經寫成 helper 了」「啊！這個已經寫成 class」的情況屢見不鮮，所以我在上新的版面過程中，都會盡量地以「之後的自己或別人也能看得懂」為原則。同時也會順手把以前的 code 跟文件統一整理。

最陽春的 styleguide 只花了兩天就有了雛形了，這證明文件的建置並不是那麼困難。但是更重要的是 consistency，也就是持續更新這件事。

而且透過寫文件的方式，其實 code 的概念很容易就讓別人懂，雖然無可避免地要花一些時間，但我認為這是蠻值得的投資。所以讓我們一起來 document driven 吧！

接下來會介紹我在建置時發現的一些不錯的概念。

## Helper 真的很好用

使用 helper 的方式，我們不僅可以快速套用 `rails` `ruby` 內建的方法，還可以讓 view 裡面的 HTML 碼大量減少，增加易讀性跟擴展性。

```html
<%= ui_component("sudo_tags", {
	:tags => "blabla"
}) %>
```

這個 `ui_component` 的方法，其實是對 render 的進一步包裝：

```ruby
def ui_component url, options = nil
	render "components/#{url}", :locals => options
end
```

雖然是簡單的方法包裝，但其實大量提升了可讀性，我們也可以拆分元件，統一放入 `component` 裡面管理。如果元件的構成很簡單，甚至還可利用 `content_tag` 來包裝。

```ruby
def	button_generator content, options = nil
	btn_class = "btn "
	options.merge(:class => btn_class)
  content_tag :button, content, options
end
```

希望以後真的可以達到高度組件化，這樣就算是科科，也能簡單引用某個元件。

## 關於 layout 的新想法：

我實在非常不喜歡 grid，但實務上無可避免地還是會使用到。
那麼，有沒有比較折衷的辦法來維持可讀性，也就是所謂的 `layout` 與語意分離。

還真的有，而且這個方法實作起來並不難。答案是善用 css 的 attribute selector

```html
<div layout="col-3"></div>
```

```css
[layout^="col-*"] {
  // your gird style
}
```

雖然看起來只是將 layout 拆至一個 attribute 裡面，但這樣至少不會看見 `col-3` 等 grid 的 rule 出現在 class 裡面。對我來說可讀性提高很多，但最理想的方式還是盡可能寫進 class	裡面吧！


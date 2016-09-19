## MutationObserver

mutation：異變

跟 Object.observe 最大的差別在於 MutationObserver 可以幫你監聽 DOM 的變化。

而跟純 addEventListener 的不同點在於可以在 observe 方法中，透過給定選項的方式，監聽不同的 DOM 變化。

### 基本用法：

```js
var obsever = new MutationObserver(function(mutations){ 
  // do something with your mutation
});
```

這邊的 callback 接收一個 mutations 參數，此參數是一個 MutationRecord 的陣列。MutationRecord 裡頭會紀錄被觀察的 DOM 的中必要的資訊。

### MutationRecord

| 屬性 | 類別 |  |
| --- | --- | --- |
| type | string | 會紀錄被監聽的 DOM 變化屬於哪一種 type。有 attributes, characterData, childList 三種狀態 |
| target | node | 被監聽的 node 節點 |
| addedNodes | nodeList | 回傳加入的 nodeList，如果沒有，則會**回傳空陣列** |
| removedNodes | nodeList | 回傳被刪除的 nodeList，如果沒有，則會**回傳空陣列** |
| previousSibling | node |  |
| nextSibling | node |  |
| attributeName | string |  |
| attributeNamespace | string |  |
| oldValue | string |  |

### observe

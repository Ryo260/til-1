1. jQuery無しでページ読み込み時の処理を実装  
DOMContentLoadedイベントに無名関数をバインドする
```javascript
document.addEventListener('DOMContentLoaded', function() {
  alert("hoge");
});
```
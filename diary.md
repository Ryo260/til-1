# 2019/7/16
## フロント
* 参考  
https://qiita.com/yanda/items/3a99891f03e9e84ceed8
* BEM:フロントエンドにおける命名規則の一種らしい  
https://frasco.io/5-reasons-to-use-bem-f5ca38f748a1

## javascript
* 連想配列の追加  
配列のpush()みたいな関数はないため注意
```javascript
let dict = {};
dict[key] = value;
// または
dict.key = value;
```
* length()とlength
```
// 文字列の長さは関数
str.length();
// 配列の長さはプロパティ
arr.length;
```

## django
### 実装
* function based view　と　class based view
前者の方が細かい制御が可能な分実装が大変  
後者の方が楽に実装できる分ブラックボックス

* setings.pyのBASE_DIRについて
  * os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  * os.path.abspath(__file__)  
  →settings.pyに至るまでの絶対パス
  * os.path.dirname  
  → ファイルが入っているディレクトリの名前  
  つまり、BASE_DIRとは「settings.pyから２階層上のディレクトリ名」を指している(os.path.dirname*2回)  
  → 「manage.pyが入っている場所」という認識で良い
* 
### 概念
* アプリ  
機能単位で実装を切り分けるためのもの。  
基本的にurls, views, modelsをワンセットとする。
appを取りまとめるのがprojectのsettings.py
  * projectからappへ切り出していくイメージ
  * projectへリクエストを出すと、appへ処理がディスパッチされる

## LINUX
1. ファイル作成
```
$ touch <file name>  
```
## 業務
###  目標
1. 会話の目的を明確にし、意識する  
2. デザインパターン「Builder」の理解と実装

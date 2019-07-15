# 2019/7/16
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
2. 結論を相手の意見とする話運び
3. 「なぜ」の文脈と言葉選び

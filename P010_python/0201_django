* djangoプロジェクトの作成
```
$ django-admin startproject taskMan
```
* makemigration → migrateという流れでDB連携を行う
* djangoとMySQLの連携では、PyMySQLというモジュールを使う。  
→django2.2.xにはまだ対応していないので注意。  
→接続先情報はsetting.pyに記載する

### 詰まったこと
1. venv環境をVSCodeで開くとエラー表示  
→VSCodeのターミナルで動作させるPythonモジュールの場所へvenvを適用するには、  
venvを起動してからVSCodeを開く必要がある。
2. migrateでエラー  
→django2.2.0にはまだPyMySQLが対応していない。  
→仮想環境でバージョンを揃える重要さが分かった
3. makemigrationでエラー  
→接続先情報が間違っているor権限が与えられていない  
→DB接続のセットアップに必要なことが分かった。

### 所感
* 仮想環境とVSCodeの連携
* 仮想環境でバージョンを揃えること
* sqlite以外のDB連携のイメージが分かった
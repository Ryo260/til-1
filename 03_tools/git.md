# gitのTips

## コマンド

* git init
    * ローカルリポジトリの作成  
``` 
git init hogehoge
```

* git status
    * ローカルリポジトリと作業コピーの状態を比較する。

* git add
    * 変更内容をコミットのステージへ上げる。  
```
git add "hogehoge.java"
```

* git commit
    * ステージへあがっているリソースをリポジトリへコミットする。  
```
git commit -m "update"
```

* git clone
    * リモートリポジトリをローカルへコピーする
```
TODO 例
```

* git push
    * ローカルリポジトリのコミット済みリソースをリモートリポジトリへコミットする。
```
git push git@github.com:<user-name>/<repo-name>
```

* git rm
    * あまり使ってないので調べ中

## ファイル
* .gitignore
    * コメント：#
    * .ignoreファイルを配置したディレクトリより深い階層にのみ適用される
```ignore
\# tempフォルダを無視する
temp/
```
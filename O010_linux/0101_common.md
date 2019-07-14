* ファイル・フォルダ削除
```
$ # ディレクトリ内にファイルがあると削除不可のフォルダ削除
$ rmdir <name>
$ rmdir <name> --ignore-fail-on-non-empty
$ # ファイル・フォルダ削除
$ rm -r <name>
```
* 大体のことはhelpオプションでなんとかなる
```
$ <command> --help
```
* homeからの相対パス指定(例:ssh keyをcatで参照する)  
```
$ cat ~/.ssh/id_rsa.pub
```
* ファイルの移動
```
$ mv <before> <after>
```
* VSCodeインストール
```
$ # curlでdebパッケージをダウンロード
$ curl -L https://go.microsoft.com/fwlink/?LinkID=760868 -o vscode.dev
$ # debパッケージからインストール実行
$ sudo apt install ./vscode.deb
$ # VSCode呼び出し
$ code
```

* pipのインストール
```
sudo apt install python-pip
```
   * 注意点：  
   aptでインストールしたpipをpip自身でupgradeするとモジュールが読み込めなくなる  
   [参考]https://laboradian.com/cannot-import-name-main-when-upgrading-pip/#i-2  
   定期的にapt updateしてapt install python-pipすることで更新を行うこと  
   また、pipでモジュールをインストールする場合は以下のようにする  
   1. ユーザー用  
   ```
   pip install --user <module name>
   ```
   2. システム用(rootのみアクセス可能)
   ```
   sudo pip install <module name>
   ```
* pipでインストールした機能はパスを通す必要がありそう  
※virtualenvが使えない → スーパーユーザ以外の権限でpipインストールをすると環境変数が自動設定されない模様  
→ ちなみにwhich 変数名 で、環境変数の設定内容を調べられる

パスの設定は以下を参考  
https://bi.biopapyrus.jp/os/linux/path.html  
* virtualenv環境でpipインストールしたdjangoが見つからない  
→ pipのバージョンは3.6だが、pythonは3.7  
python3 -m django --versionはpython3.7にインストールされたモジュールを参照する  
python -m django --versionはvirtualenv環境のpythonバージョンにインストールされたモジュールを参照する  
※virtualenv -p python3.6 <環境名>  
pipでインストールしているのは後者のパスであるため、python3コマンドでは参照できなかった

### Ubuntu18.04とwindows10のデュアルブート
* 参考  
    * インストール  
    https://veloart-intelligence.com/blog/dualboot-ubuntu18-04-with-windows10/#i-8
        * ブートローダのインストール先に注意。今回はSSDを指定した
    * python3.7  
    https://www.sejuku.net/blog/83204
    * Let's Noteで輝度調整ができなくなる問題への対応  
    https://www.kinacon-blog.work/entry/2018/07/29/081812
      * [acpi_backlight=vendor acpi_osi=] をGRUB_CMDLINE_LINUX_DEFAULTに追記する

* ターミナル起動：Ctrl+Alt+T
* ls カレントディレクトリの中身を一覧表示する(dir)
* mkdir フォルダ作成
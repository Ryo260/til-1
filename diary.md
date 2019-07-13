# 2019/7/14

# 2019/7/13

## python
* pip
 * 2.xと3.xが共存している環境ではpip3を使う  
 ※ そもそも、インストール先となるディレクトリが違う模様
 ```
 $ # インストール済のパッケージ一覧
 $ pip3 list # 非推奨？
 $ pip3 freeze # listと何が違う？
 ```
* なお、sudo pip3したパッケージは消す時もsudoしなければダメ
```
$ # 例
$ sudo pip3 uninstall django
```

```python
# coding: utf-8

def debug(prop, x):
    print('[debug] ' + prop + ' = ' + str(x))
    # print(str(x))    

def conv_to_array(arg):
    return [int(value) for value in arg]

def get_even(arr):
    debug('get_even_arr', [num*2 if (num*2 < 10) else (1 + (num*2)%10) for num in arr])
    return [num*2 if (num*2 < 10) else (1 + (num*2)%10) for num in arr]
        
def get_first_digit(numbers):
    # debug('numbers', conv_to_array(numbers[0:15:]))

    even = sum(get_even(conv_to_array(numbers[0:15:2])))
    # debug('for_even', conv_to_array(numbers[0:15:2]))
    debug('even', even)
    odd = sum(conv_to_array(numbers[1:15:2]))
    # debug('for_odd', conv_to_array(numbers[1:15:2]))
    # debug('odd', odd)
    hoge = even + odd
    # debug('hoge',hoge)
    
    X = 10 - (even + odd)%10
    
    return X

N = int(input())
"""
for i in range(0, N):
    print(get_first_digit(input())
"""
# debug('N', N)
for i in range(0, N):
    debug('i',i)
    debug('X',get_first_digit(input()))
```

## Linux
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
## Docker
* Linuxカーネル上で動くので、Windowsより導入が楽そう  
※そもそもWindows10はProじゃないと結局VMクライアント使うことになって重い



# 2019/7/12
## チーム開発
* リリース資材の確認  
→ 「自分の修正に係るソース」が全量揃っているかを確認する。  
→自分のソースに「他人が直したソースに依存する箇所」がある場合、  
その修正もリリース対象になっているかを確認する。  

## エディタ
### vim
* :Tutorial
* オペレータ／カウント／モーション
  * オペレータ  
  指定なし → 移動
  [d] → 削除
  
  * モーション  
  [w] → 次の単語の先頭まで
  [e] → 次の単語の末尾まで
  [$] → カーソルがある行の末尾まで
  [0] → カーソルがある行の先頭まで
  
  * カウント  
  カウント + モーションで、そのモーションの動作を複数回繰り返す指定となる。  
  2w → 次の次の単語へ移動  
  d2w → 次の次の単語まで削除
  
## 統計
* データを情報とする方法の一つ = データ全体の様子(特徴)を捉えること  
 * データがどんな値をとっているか  
 * データがどのようにばらついているか  
 * データがどの程度ばらついているか  
* データを分析するための観点「分布」
 * １つの値or値の範囲に対する[件数]
* データの種類によって、適切な件数の数え方を選択する
 * 性別や血液型のような「分類」⇒分類ごとのデータ件数を数える
 * 身長や体重のような「数値」のデータ→値あるいは値の範囲を指定してデータ件数を数える  
 →分析の方法も、データの種類によって異なる  
 →まずは「データの種類」を識別することが重要  
 
 

# 2019/7/11
## Linux
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
   
## 学習について
* 覚えようとするのではなく、実際にやってみる
* 疑いをもって深堀する
* 失敗の原因を外部に求めない

# 2019/7/10-python週次
# 命名規則について
* モジュール名 = ファイル名

## 命名規則の目的
* 仕事を早く終わらせるため(非効率)
    * パターン認識

## 直接的、間接的なメリット
* 可読性、視認性
* 効率性、保守性

## 可読性とは
* 読みやすいこと⇒読みやすいとは？⇒英語のように読めることが重要
* 「意味を持たせること」

## 視認性とは
* 見てすぐわかるとは？
* 逆に見てすぐ見てわからないものとは？

## 効率性と保守性
* 読みやすいコードには「一貫性」がある

## PEP8⇒パイソンのコーディングについてのノウハウ
* スペースの使い方なども注目

## 要するに、わかりやすければよい。
## そもそも「わかりやすい」とは？？
* 個人差があるため、少なくともプロジェクトで統一する
* しかし、言語に精通していなければ難しいことでもある
* すでにあるものに忠実なのが無難

## pythonの命名規則(PEP)
* あるいみデファクトスタンダード
* 破るなら破る理由（昔からの一貫性とか）

* 参考：realpython.com
```python
import requests
# リクエスト　成功例
requests.get('https://google.com')

response = requests.get('https://google.com')

response.status_code

# 実装例
import requests
from requests.exceptions import HTTPError

for url in ['https://api.github.com', 'https://api.github.com/invalid']:
    try:
        response = requests.get(url)

        # If the response was successful, no Exception will be raised
        response.raise_for_status()
    except HTTPError as http_err:
        print(f'HTTTP Error occurred: {http_err}') # Python 3.6
    except Exception as err:
        print(f'Other error occurred: {err}') # Python 3.6
    else:
        print('Success!')

response = requests.get('https://api.github.com')

if response.status_code == 200:
    print('Success-1')
elif response.status_code == '404':
    print('Error-1')

# 内部
if response:
    print('Success-2')
else:
    print('Error-2')

# バイト型JSONオブジェクト(b'')
response.content
# ストリング型JSONオブジェクト
response.text

>>> type(response.text)
<class 'str'>
>>> type(response.content)
<class 'bytes'>

# 辞書型JSONオブジェクト
response.json()
>>> type(response.json())
<class 'dict'>

response.json().get('current_user_url')

# レスポンスの文字コードを指定
response.encoding = 'utf-8'
response.text # utf-8でJSON文字列が返却

"""
ストリングフォーマット文
f'hogehoeg{hensuu}
⇒.format()使わなくてもより短くかける
"""
ドックストリング
モジュールの上のほうに
"""
hogehoge
~~~~~~~~~~~~~~~~~~~

"""
としておくと、__doc__で決められた形式で取得できる

文頭のutf-8 は書いておくと丁寧

chardet.detect(バイト文字列)で{'encoding', 'confidence', language}の辞書をとってくるので
[encoding]で文字コードを取得できる
```

### WEBスクレイビング HTTP取得⇒構文解析
### セールスフォース

# 2019/7/10
## 環境構築
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
## Linux
* ターミナル起動：Ctrl+Alt+T
* ls カレントディレクトリの中身を一覧表示する(dir)
* mkdir フォルダ作成
## javascript
1. jQuery無しでページ読み込み時の処理を実装  
DOMContentLoadedイベントに無名関数をバインドする
```javascript
document.addEventListener('DOMContentLoaded', function() {
  alert("hoge");
});
```
## eclipse
### ブレークポイントに条件を付けることができる。  
* 参考  
https://www.hitachi.co.jp/Prod/comp/soft1/cosminexus/useful/tips/090107_eclipse-breakpoint.html
## エディタ
### Vim
* kaoriyaのWindows用Vim
   * hjkl ←↓→↑
   * w 次の単語の先頭へ b 前の単語の先頭へ e 今の単語、次の単語の末尾へ
### EMacs
* GNU EMacsのサイズが大きいのでダウンロードできなかった

# 2019/7/9
## 統計
### 統計のフロー
 1. 解決したい問題を整理する
 2. 調査方法、実験方法を設計する
 3. データを収集し、整理する
 4. データを分析する
 5. 分析結果から結論を出す
 ### 目標
 1. 問題解決のプロセスを理解する
 2. データの集め方が適切であるかどうかに言及できる
 3. データ分析の手法を説明できる
 4. 分析結果の解釈について説明し、自分の意見をまとめることができる
### 統計の種類
1. 記述統計  
手元にあるデータを全体ととらえ、結論を出す。
2. 推測統計  
手元にあるデータを全体の一部分ととらえ、  
分析結果から全体の状態を推測し結論付ける。
## その他  
技術はお金と同じく、持っている人の所に集まる。  
技術はお金と異なり、総量がない。  

# 2019/7/8
## バグ調査
* 以下の点を明確にする
  * 内容
  * 検出の経緯
  * 影響
  * 原因
  * あるべき姿
  * 対応に必要な作業
  * 対応に必要な工数
## 進め方
* 常に「他人への引き継ぎ」を意識した資料を用意しておく
## テスト実施
* 再テストの考え方：  
 * 全ステップ再実施
 * 前段のステップから再実施  
 ⇒根拠が必要
## 情報の連携
* なるべく「データ」⇒「情報」へ精度を上げてから伝えるよう心掛ける

## python
1. 約数求めたり
```python
# coding: utf-8

N = int(input())
numbers = []
for i in range(0,N):
    numbers.append(int(input()))

def get_divisor_sum(num):
    temp = []
    for i in range(1, num+1):
        if num % i == 0:
            temp.append(i)
    result = 0
    for j in temp:
        result += j
    return result

def get_judge_result(num):
    divisor_sum = get_divisor_sum(num)
    S = divisor_sum - num
    if S == num:
        return 'perfect'
    elif abs(num - S) == 1:
        return 'nearly'
    else:
        return 'neither'

for num in numbers:
    print(get_judge_result(num))
```
2. 内包表記の利用(リストの宣言と型変換を同時に行う)
```python
# coding: utf-8

set_1_1 = [int(num)-1 for num in input().split()]
set_1_2 = [int(num)-1 for num in input().split()]

# 1回戦
time_1 = [int(num) for num in input().split()]
winner_1_1 = set_1_1[0] if time_1[set_1_1[0]] < time_1[set_1_1[1]] else set_1_1[1]
winner_1_2 = set_1_2[0] if time_1[set_1_2[0]] < time_1[set_1_2[1]] else set_1_2[1]

# 2回戦
time_2 = [int(num) for num in input().split()]
set_2 = sorted(num + 1 for num in [winner_1_1, winner_1_2])

# 結果
if (time_2[0] < time_2[1]):
    print(set_2[0])
    print(set_2[1])
else:
    print(set_2[1])
    print(set_2[0])
```
# 2019/7/7
## python
1. 改行せずにprint()関数を使用する
```python
N = int(input())

for i in range(0, N):
  # 第二引数でオプションを渡せる。endは出力文字列の末尾を指定できる
  # デフォルトが'\n'らしい
  print('*', end='')
print('\n')
```
2. リスト内包表記と辞書からの抽出オプション
```python
# coding: utf-8

word = input()

leet_map = {
    'A':'4',
    'E':'3',
    'G':'6',
    'I':'1',
    'O':'0',
    'S':'5',
    'Z':'2',
}

def convert2Leet(arg, leet_map):
    result = ''
    # [返却値 for リストの一要素 in リスト]
    # get(キー, <キーがない場合の値>)

    for s in [leet_map.get(s,s) for s in arg]:
        result += s
    return result;

print(convert2Leet(word, leet_map))
```
3. 複数行の標準入力取得と３項演算子
```python
# coding: utf-8

N = int(input())
pitching = []
for i in range(0, N):
    pitching.append(input())

count_strike = 0
count_ball = 0

for s in pitching:
    call = ''
    if s == 'strike':
        count_strike += 1
        call = 'out!' if count_strike == 3 else s+'!' 
    if s == 'ball':
        count_ball += 1
        call = 'fourball!' if count_ball == 4 else s+'!'
    print(call)
```
# 2019/7/6
## python備忘
1. オブジェクト参照の挙動
```python
a = 9
b = a
a = 3
print(b)
```
2. 文字と演算
```python
start = 'Hoge\t' * 4 + '\n'
end = 'Foo\t' * 3 + '\n'
print(start + end)
```

3. スライシング
```python
N = '0123456789abcdef'

print(N[::2])
print(N[-1::-1])
print(N[:12:])
```
4. 文字列操作
```python
todos = 'get gloves,get mask,give cat vitamins,call ambulance'
todos_splited = todos.split(',')
todos_joined = ', '.join(todos_splited)
```
5. その他の文字列操作
```python
X = 'abcdedcbab'

X[:3]
len(X)
X.startswith('a')
X.endswith('b')
X.find('ab')
X.rfind('ab') # 最後に登場
X.count('ab')
X.isalnum() # すべて数値か
```
# 2019/7/5
## java
* シャローコピーとディープコピー
* HashMapをVに持つHashMap
```java
// 生成
HashMap<String, HashMap<String, String>> map = new HashMap<>();
// 値の取得
map.get("parentKey").get("childKey");
// すべての値を取得
for (String parentKey:map.keySet()){
 map.get("parentKey").get("childKey");
}
```

## javascript
* パフォーマンス調査：  
「パフォーマンス」タブで記録を開始、操作を行い記録停止後にレポート出力で、  
どのイベントにどのくらいの時間が掛かっているのかを分析できる  

## その他
* シングルページアプリケーション  
クライアントの処理性能に依存。その分通信量やサーバー性能は重視しなくても良くなる。
* 自宅環境へのリモート接続におけるセキュリティ  
⇒sshを利用した公開鍵認証でよいのでは
* 関数の切り分けTips
 * 関数単位で「責任」を考える。  
 責任 = 関数通過前後の遂行されるべき動作  
 ⇒ 複数の「責任」が与えられている場合は、切り分ける。  
 ※ 責任はその関数の最上位レイヤー部分だけを見る。  
 複数のロジックを実行する関数(Service)の場合、  
 「関数を実行する」という責任は持つが、「正しい関数の出力結果を出す」責任は持たない。  
 

# 2019/7/4
## ストアドファンクション(SSMS)
```sql
CREATE FUNTION fc_hoge
(@iArg1 nvarchar(100), @iArg2 nvarchar(100))
RETURNS nvarchar(10)
AS
  BEGIN
    DECLEAR @val nvarchar(10)
  SET
    @val = (
      SELECT
        hoge
      FROM
        table
      WHERE
        column1 = @iArg1
        AND
        column2 = @iArg2
    )
```
# 2019/7/3
## python 社内勉強会
* TODO: 記載

# 2019/07/01
## 用語
* クリーンアーキテクチャ
  * スマホ開発で主流。Web開発はまだMVC
* DDD(ドメイン駆動開発)
  * 業務フローをベースとした開発？
* ファーストクラスコレクション
  * 配列もラップ
* 

## プログラム
### python
* class
```python
class Test(Object):
      def __init__(self, obj):
        self.obj = obj

    def __getstate__(self):
        return self.obj.items()

    def __setstate__(self, items):
        if not hasattr(self, 'obj'):
            self.obj = {}
        for key, val in items:
            self.obj[key] = val
```
---
* enum
```python
import enum

class Color(enum.Enum):
    Red = 10
    Blue = 20
    Yellow = 30

    def __str__(self):
        if self.value == Color.Red.value:
            return "赤"
        elif self.value == Color.Blue.value:
            return "青"
        elif self.value == Color.Yellow.value:
            return "黄"
```
---
* matplotlib
```python
# done pip insatall matplotlib

from pylab import plot, show

# 座標の指定
x_numbers = [1, 2, 3]
y_numbers = [2, 4, 6]

# プロット
plot(x_numbers, y_numbers]

# 表のポップアップ
# (1, 2), (2, 4), (4, 6)を通る線形グラフ
show()

```
---
* リストとタプル
  * リストは後から要素の追加が可能。タプルは不可
  * 宣言：前者[] 後者()
  
##  その他
* オブジェクト指向エクササイズ
* 

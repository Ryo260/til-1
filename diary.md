# 2019/7/10
## 環境構築
### Ubuntu18.04とwindows10のデュアルブート
* 参考  
    * インストール  
    https://veloart-intelligence.com/blog/dualboot-ubuntu18-04-with-windows10/#i-8
        * ブートローダのインストール先に注意。今回はSSDを指定した
    * python3.7  
    https://www.sejuku.net/blog/83204
## Linux
* ターミナル起動：Ctrl+Alt+T
* ls カレントディレクトリ一覧
* mkdir フォルダ作成

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

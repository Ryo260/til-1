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
# 2019/7/3
## python 社内勉強会
* TODO: 記載

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
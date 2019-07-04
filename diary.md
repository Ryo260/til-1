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

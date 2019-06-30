# 見出し1 [\#]
## 見出し2
### 見出し3

# 中点リスト [\*] or [\+]
※"+"と"*"は、構造上「別リスト」として扱われる。
そのため、上下に隣り合わせ場合は行間が発生する。
+ リストA
* リストA
    * リストA

# 数字付きリスト [x.]
1. リスト
2. リスト
    1. リスト

# 引用 \>
> 引用
>> 引用
>>> 引用

# ソース [\```] ~ [```]

```html
<div class="hoge"></div>
```

***

```java
public static void main(String[] args){
    System.out.println("hoge");
}
```

***
```python
import Fraction from fractions

#comment
def hoge_func():
    print('hoge')

if __name__ == '__main__':
    hoge_func()
```

# テーブル [|]

| Left align | Right align | Center align |
|:-----------|------------:|:------------:|
| This       |        This |     This     |
| column     |      column |    column    |
| will       |        will |     will     |
| be         |          be |      be      |
| left       |       right |    center    |
| aligned    |     aligned |   aligned    |


***

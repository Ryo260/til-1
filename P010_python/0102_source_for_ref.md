#201907

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
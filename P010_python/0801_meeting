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
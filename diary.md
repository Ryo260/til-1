# 2019/7/20

## django
* CSRF(クロスサイトリクエストフォージェリ)対策をしていなければエラー
```html
<!--慣例としてform要素の右に記述 -->
<form action="" method="POST"> {% csrf_token %}
</form>
```
* submit後のリダイレクトを設定していなければえらー
```python
# viewsで指定

class HogeView(CreateView):
    # ...
    success_url = reverse_lazy('')

```
* reverseについて
URLを「逆回り」する。  
通常はurls→viewsと関連付けて通信を行うが、  
逆回りではviewsからurlに紐づけて通信を行う。(厳密にいえばviews→urls→views)  

```python
# // views.py
from django.urls import reverse, reverse_lazy()

class HogeView(CreateView):
    success = reverse_lazy('top')

# class based viewでは reverse()を使用する
# function_based_viewでは reverse_lazy()を使用する

```
```python
# // urls.py
urlpatterns = [
    # path()の引数で指定したnameへreverseされる
    path('top/', TopView.as_view(), name='top')
]
```
## bootstrap4
* 

## エディタ
### VSCode
* コード整形(Linux)
```
Ctrl + Shift + I
※Windowsは I ではなく F
```

# 2019/7/19
## 業務
###  目標
1. 大分類→小分類 という順で話をする。  
(最初から細かい話を振らない)
2. デザインパターン 「**Adapter**」の理解と実装  

* DP学習進捗
1. Builder_20190716
2. Abstract Factory_20190717

## Java
### Adapterパターンについて  
[参考]https://qiita.com/shoheiyokoyama/items/bd1c692db480b640c976
* 別名「Wrapper」パターン
* interface間の違いを吸収する。
* 継承を利用するパターン／委譲を利用するパターンで２つの実現方法がある。

### 概要  
* 継承を利用するパターンにて説明(委譲も大した変わらないので最後に記載)
1. MyAppからクラスの機能を使いたい
```java
class MyApp {
    public void execute(){
        Hote hoge = new Hoge();
        // Hogeクラスの機能
        hoge.doHoge();
    }
}
```
```java
class Hoge {
    public void doHoge(){};
}
```
2. ここで、HogeクラスからdoFuga()というメソッドを使いたくなった。
```java
class MyApp {
    public void execute(){
        Hote hoge = new Hoge();
        // Hogeクラスの機能
        // hoge.doHoge();
        hoge.doFuga(); 
    }
}
```
3. しかし、HogeクラスのinterfaceにdoFuga()は存在しない。  
このとき、最も単純に解決するならば以下の様になる。
```java
class Hoge {
    public void doHoge(){};
    public void doFuga(){}; // doFugaを新しく追加する
}
```
4. 上記の例には問題がある。  
doFuga()を追加したことにより、Hogeクラスそのものを触ってしまったため  
__「Hogeクラス自体の全機能について再テストが必要」__ となってしまった。  

5. では、Hogeクラスを拡張して使いたいがHogeクラスを直接修正したくない場合はどうすれば良いかを考える。  
```java
class HogeAdapter extends Hoge implements IFuga{
    // Hogeの拡張クラスへdoFugaするinterfaceをimplementsする
    @Override
    public void doFuga(){};
}
```
```java
interface IFuga {
    // doFugaする機能を持ったインターフェース
    void doFuga();
}
```
6. こうすることで、既存機能へ影響を与えることなく、  
MyAppにて新たに必要となったHogeクラス機能としてのFugaを追加するという拡張が可能となる。  
  
◆利点:  
* Hogeの再テストが不要
* Fugaを実装したことでバグが発生しても、Hogeクラスではバグが発生していないことが保証される

なお、IFugaをimplementsしたHogeAdapterを使うのが「継承を使用するパターン」で、  
doFuga()するFugaクラスを作成してHogeAdapterから呼び出すのが「委譲を使用するパターン」
```java
class HogeAdapter extends Hoge {

    // Fugaクラスの処理をHogeAdapterへ委譲
    Fuga fuga = new Fuga();
    fuga.doFuga();
}
```
```java
class Fuga {
    public doFuga(){};
}
```



# 2019/7/18
## 休み
## djangoについて後日記載

# 2019/7/17
## 業務
###  目標
1. 目的を伝える→本題に入るの流れを徹底する  
2. デザインパターン 「**Abstract Factory**」の理解と実装  

* DP学習進捗
1. Builder_20190716

## Java
### Abstract Factoryパターンについて
[参考]https://qiita.com/morimotof/items/67a9e2a8d7e15ea321d2
#### 概要

1. 通常のJavaアプリケーション  
利用したいコンポーネントのインスタンスを得るためのコードが、  
アプリケーション本体(MyApp)に記載されている。
```java
// "A"を"B"にしたい場合、MyAppを編集しなければならない
// アプリケーションの実行系を直接触るのはいろいろやだ
class MyApp {
    /**
     * 本処理
     */
    public void execute(){
        Hoge hoge = new Hoge("A");
        Foo foo = new Foo("A");
    }
}

class Hoge {
    public Hoge(String arg) {
        this.arg = arg;
    }
}

class Foo {
    public Hoge(String arg) {
        this.arg = arg;
    }
}

```

2. Factoryパターンを使用した場合  
Factoryクラスのメソッド経由でインスタンスを得る。  
これにより、インスタンスの作成がアプリ本体から分離されるため、  
例えばインスタンスの生成方法が変わってもFactoryクラスを改修するだけで良くなる。
```java
class MyApp {
    
    /**
     * 本処理
     */
     public void execute() {
        Factory f = new Factory();
        Hoge hoge = f.creageHoge();
        Foo fo = f.createFoo();
     }
}

// "A"を"B"にしたいときでも、MyAppは触らなくてよい
class Factory {
    public Hoge createHoge() {
        return new Hoge("A");
    }
    
    public Foo createFoo() {
        return new Foo("A");
    }
}
```
3. Abstract Factoryパターンを利用した場合  
Factoryを抽象クラスにする。
これにより、いろんな種類のHogeやFoo作成用Factoryを定義することができる。
(Factoryの具象クラスで各々の振る舞いを実装できるため、  
コンポーネントの動的な切替や差し替えが可能になる)
```java
class MyApp {
    
    /**
     * 本処理
     */
     public void execute(String flag) {
        if (flag == "X") {
            Factory f = new FactoryX();
        } else if (flag == "Y") {
            Factory f = new FactoryY();
        }
        this.execute(f);
     }
     
     // 動的にコンポーネントが定義されたFactoryで処理を実行
     private void execute(Factory f) {
        Hoge hoge = f.createHoge();
        Foo foo = f.createFoo();
     }
}

// Factory抽象クラス
abstract class Factory {
    abstract public Hoge createHoge();
    abstract public Foo createFoo();
}

// フラグが"X"の場合用コンポーネント生成Factory具象クラス
class FactoryX extends Factory {
    public Hoge createHoge() {
        return new Hoge("X");
    }
    public Foo createFoo() {
        return new Foo("X");
    }
} 

// フラグが"Y"の場合用コンポーネント生成Factory具象クラス
class FactoryY extends Factory {
    public Hoge createHoge() {
        return new Hoge("Y");
    }
    public Foo createFoo() {
        return new Foo("Y");
    }
} 

```

#### 利点
* 主処理におけるコンポーネント差し替えが容易であるため、  
例えばデバッグ用のコンポーネントを手元で作ってFacotry経由で呼び出すといったことが可能になる。

* Factory毎にコンポーネントの組み合わせ及び振る舞いを定義しているため、  
たとえば本来flag=="X"用のインスタンスを使うべきところでFoo("Y")してしまう、  
といった間違いが起こらなくなる。  
(常にflag=="X"用、"Y"用のコンポーネントを利用できていることが分かりやすくなる。

## django
* virtualenvおさらい
```shell
$ virtualenv <env name>
```
* django startprojectおさらい
```
$ # 普通にprojectを作成(カレントディレクトリにフォルダを作り、その中にprojectとmanage.pyができる)
$ django-admin startproject

$ # オプションで"."を指定→カレントディレクトリにprojectとmanage.pyをつくる
$ django-admin startproject .
```
* アプリ作成の流れ
1. starapp app
2. project/settings.pyで以下を追記  
  ・TEMPLATES  
  ・INSTALLED_APPS
3. app配下にurls.pyを作成
  ※ちなみにデフォでファイルが無いのは、  
  djangoが当初すべてのリクエストをprojectのurls.pyで受け取る思想だったから。
4. projct/urls.pyで以下を追記
  ・include import
  ・urlpatternsでinclude('app.urls')
      
# 2019/7/16
## 業務
###  目標
1. 会話の目的を明確にし、意識する  
2. デザインパターン「Builder」の理解と実装

## Java
### Builderパターンについて
[参考]https://qiita.com/takutotacos/items/33cfda205ab30a43b0b1

#### 概要  
以下のような、複数のプロパティを持ちコンストラクタ引数で初期化するクラスがあるとする。
```java
class Hoge(){
    public Hoge(int arg1, int arg2, int arg3, int arg4, int arg5, int arg6){
        this.arg1 = arg1;
        this.arg2 = arg2;
        // ...
    }
}
```
* このようなクラスは、以下の問題を抱えている。  
1. 引数の数が多く、順番を間違えやすい。
2. 必須ではない引数(特定のインスタンスでのみ使用するプロパティ)に対して、  
何かしらの値を渡さなければならない。  
```java
// arg3, arg4を使わない場合でもnullか何かは設定する必要がある
public Hoge hoge = new Hoge(1, 2, null, null, 5, 6);
```

そこで、
* コンストラクタの引数は必須項目のみ
* 任意項目は「値をセットした自分自身のインスタンスを返却する」メソッドを用意する  
というクラスを作る。以下例。
```java:Hoge.java
// arg1, arg2が必須でそれ以外は任意
class Hoge {
    // 内部クラスでBuilderを定義
    class Builder{
        public Builder(arg1, arg2){
            // 必須項目にnullが設定されている場合は例外をスロー
            if (arg1 == null || arg2 == null) {
                throw new NullPointerException();
            }
            this.arg1 = arg1;
            this.arg2 = arg2;
        }
        
        /**
         * 任意項目の値を設定
         */
         public Hoge arg3(arg3){
            this arg3 = arg3;
            return this;
         }
         
         public Hoge arg4(arg4){
            ...
         }
         // ...以下同じ
         
         public Hoge arg6(arg6){
            //...
            return this;
         }
         
        /*
         * 値が設定されたBuilderからHogeインスタンスを生成
         */
        Hoge build(){
            if (arg1 == null || arg2 == null) {
                throw new NullPointerException();
            }
            // Builderを受け取って値をセットするコンストラクタ
            return new Hoge(this);
        }
    }
}
```
* 呼び出しは以下
```java:Main.java
// Builder経由でインスタンスを生成する
Hoge hoge = new Hoge.Builder(1, 2).arg6(6).build();

// これにより、arg1, arg2, arg6がセットされたHogeインスタンスが生成される
```

* Builderパターンが適している例
1. 必須項目が少ない
2. 任意項目を含めると引数が多い
3. 同じクラスの複数インスタンスを使いたい

* Builderパターンが適さない例
1. 全て必須項目(コンストラクタから分離する意味なし)
2. 項目が大量(大量のBuilder用メソッドをチェーンするため意味なし)

#### 利点
* 引数が多いインスタンス生成の見通しが良くなる
* どの引数をインスタンス生成時に渡しているのかが分かりやすくなる

## フロント
* 参考  
https://qiita.com/yanda/items/3a99891f03e9e84ceed8
* BEM:フロントエンドにおける命名規則の一種らしい  
https://frasco.io/5-reasons-to-use-bem-f5ca38f748a1

## javascript
* 連想配列の追加  
配列のpush()みたいな関数はないため注意
```javascript
let dict = {};
dict[key] = value;
// または
dict.key = value;
```
* length()とlength
```
// 文字列の長さは関数
str.length();
// 配列の長さはプロパティ
arr.length;
```

## django
### 実装
* function based view　と　class based view  
前者の方が細かい制御が可能な分実装が大変  
後者の方が楽に実装できる分ブラックボックス  

* setings.pyのBASE_DIRについて
  * os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  * os.path.abspath(__file__)  
  →settings.pyに至るまでの絶対パス
  * os.path.dirname  
  → ファイルが入っているディレクトリの名前  
  つまり、BASE_DIRとは「settings.pyから２階層上のディレクトリ名」を指している(os.path.dirname*2回)  
  → 「manage.pyが入っている場所」という認識で良い
* 
### 概念
* アプリ  
機能単位で実装を切り分けるためのもの。  
基本的にurls, views, modelsをワンセットとする。
appを取りまとめるのがprojectのsettings.py
  * projectからappへ切り出していくイメージ
  * projectへリクエストを出すと、appへ処理がディスパッチされる

## LINUX
1. ファイル作成
```
$ touch <file name>  
```

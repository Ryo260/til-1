# 2019/7/23
## -7/20分の棚卸を行う事

## 業務
###  目標
1. 相手が何を求めているのか把握してからレスポンスを返す
2. デザインパターン 「**Singleton**」の理解と実装  

* DP学習進捗
1. Builder_20190716
2. Abstract Factory_20190717
3. Adapter_20190719
4. Chain of Responsibility_20190722

## django
* function based view
```python
from django.shortcuts import render

def hoge(request):
    template_name = 'hoge.html'
    model = HogeModel
    context = {'hoge':100}
    return render(request, template_name, context)
```


# 2019/7/22
## 業務
###  目標
1. 作業着手前に、必要な要素と不確定な要素を明確にしておく。  
2. デザインパターン 「**Chain of Responsibility**」の理解と実装  

* DP学習進捗
1. Builder_20190716
2. Abstract Factory_20190717
3. Adapter_20190719

## Java
### Chain of Responsibilityパターンについて  
[参考]https://qiita.com/mk777/items/7a8f23d3e58d77486fe4
* 処理の要求元と実行クラスとをうまい感じにマッピングするデザインパターン。
* 要求された処理をどのクラスが担当するのかという判定方法を、処理実行クラスの基底クラスで定義する。  
具体的には、  
1. 「自分が処理の担当である」
2. 「別のクラスが処理の担当である」
3. 「処理の担当が実装されていない」  
を判断する。

### 概要  
1. MyAppは他のクラスに対して処理を振り分け、要求する
```java
class MyApp {

    public MyApp(){
        Foo foo = new Foo();
        Baa baa = new Baa();
        Qux qux = new Qux();
    };

    // Hogeをするメソッド
    public doHoge(int arg){
        // 引数の値の大きさによって、実行するクラスを分ける
        if (0 <= arg < 5) {
            foo.do();
        } else if(5 <= arg < 10) {
            baa.do();
        } else {
            qux.do();
        }
    };
}
```
2. 上記のMyAppクラスには問題がある。  
処理の振り分け条件を変える場合、MyAppクラスを編集しなければならない  

3. そこで、doHoge内で定義していた処理の振り分けを以下の様に切り分ける。
```java
public abstract class HogeHandler(){
    
    // 次に処理担当の判定を行うクラスを保持
    private HogeHandler next;
    
    public HogeHandler setNext(HogeHander next) {
        // 次に処理担当の判定を行うクラスを設定
        this.next = next;
    }
    
    public void support(inr arg){
        if (this.isMyResponse(arg)) {
            this.do();
        } else if (this.next != null) {
            // 責任は自分以外で、次の判定クラスがある
            this.next.judge();
        } else {
            // 責任は自分以外で、次の判定クラスはない
            this.fail;
        }
    }
    
    // 正常系処理実装用の抽象メソッド
    protected void do();
    
    // 異常系処理実装用の抽象メソッド
    protected void fail();
    
    // 処理担当の判定用の抽象メソッド
    public abstract void isMyResponse(int arg);
}
```
4. 上記抽象クラスにて条件の受け皿と判定用メソッドを準備したため、  
これを以下の様に継承して使用する
```java
public class Foo extends HogeHandler(){
    private int limitLevel = 5;
    
    public Foo (){};
    
    @Override
    protected boolean isMyResponce(int arg) {
        if (arg < this.limitLevel) {
            return true;
        } else {
            return false;
        }
    }
    
    @Override
    protected void do(){
        System.out.println("Foo do.");
    }
    
    @Override
    protected void fail(){
        System.out.println("Foo fail.")
    }
}
```
```java
public class Baa extends HogeHandler(){
    private int limitLevel = 10;
    
    public Baa (){};
    
    @Override
    protected boolean isMyResponce(int arg) {
        if (arg < this.limitLevel) {
            return true;
        } else {
            return false;
        }
    }
    
    @Override
    protected void do(){
        System.out.println("Baa do.");
    }
    
    @Override
    protected void fail(){
        System.out.println("Baa fail.")
    }
}
```
```java
public class Qux extends HogeHandler(){
    private int limitLevel = 10;
    
    public Qux (){};
    
    @Override
    protected boolean isMyResponce(int arg) {
        if (10 <= arg) {
            return true;
        } else {
            return false;
        }
    }
    
    @Override
    protected void do(){
        System.out.println("Qux do.");
    }
    
    @Override
    protected void fail(){
        System.out.println("Qux fail.")
    }
}
```
```java
class MyApp {

    public MyApp(){
        Foo foo = new Foo();
        Baa baa = new Baa();
        Qux qux = new Qux();
        
        foo.setNext(baa).setNext(qux);
    };

    // Hogeをするメソッド
    public doHoge(int arg){
        foo.support(1);     // Foo do
        foo.support(6);     // Baa do
        foo.support(12);    // Qux do
        foo.support(-23);   // Qux fail
    };
}
```

### 利点
* MyAppは「処理を要求するだけ」になり、  
振り分け方を変える場合は各HogeHandler実装クラスをいじるだけで良くなった。
* メソッドチェーンがかっこいい

### デメリット
* 処理はFooから順に線形で回されるため、  
処理実行クラスが増えるといちいち判定するので重くなる。
## TODO
* ノウハウ管理をWebアプリで実施する  
→djangoで作成する。


---
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

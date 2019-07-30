##### -7/20分の棚卸を行う事
##### [デザインパターン一覧]https://www.techscore.com/tech/DesignPattern/index.html/
##### [単体テストの参考]https://qiita.com/disc99/items/177bdf6352de463fdc87
---

# 2019/7/30
## 業務
### 目標
1. テストケースを考えてから実装する(なんちゃってTDD)
2. デザインパターン 「**Composite**」の理解と実装  

デザインパターンについて考えるミソ：「何が嬉しいのか」  

### あぶない
* ブランチを切らないソース管理の場合、  
**修正中のソースに他人の修正中ソースが混在しているかどうか**気を付ける。  
修正ファイルが競合している場合、リリースの足並みを揃えなければコンパイルエラーが発生する。  
※しかもリリース先だけ。

## java
### DIについて
* 実行クラスにインスタンス生成処理をハードコードするのではなく、  
抽象的なオブジェクト生成処理を用意しておく。  
これによって、実行クラスで使用するインスタンス
(=依存オブジェクト)を外部から注入する仕組みを作る。  
★勘違いしていたのが、「DIコンテナ」はDIの実装をスマートにするための仕組みであり、  
DIと同一の概念ではないということ。  
https://qiita.com/ritukiii/items/de30b2d944109521298f  

### 作って覚えるDIコンテナ  
http://nowokay.hatenablog.com/entry/20160406/1459918560

### Compositeパターンについて  
[参考]https://qiita.com/takehiro224/items/e8ebc98d4eff29d3805c
* 階層的な構造をもつオブジェクトを抽象化し、親子を同一視する。

### 概要  
1. 帰ったら書く

## 統計
* 独立　⇔　従属

## AWS
### cloud9
* pythonのデフォルトが2.7らしいので3にアップデートすること。  
[参考]https://qiita.com/acecrc/items/fb34a12b265122816d4b  
1. ~/.bashrcのpythonを更新
2. ~/.bashrcをsourceから実行
3. upgradeする  
??よくわかっていないので、意味を調べること。

* cloud9-shiiba作成  
* Cloud9とEC2の権限だけを持つIAMユーザー cloud9-shiibaを作成  
* 東京リージョンでCloud9を作成


### VPCの作成
1. VPCを作成する  
AWSではアカウント作成時にデフォルトのVPCが作成されているが、  
これは使用せず新規に作成する。  
サービス→VPC→VPCの作成  
・名前タグ：VPCを識別する名前。  
・IPv4 CIRDブロック：VPCネットワークをCIDR表記で指定  
10.0.0.0/16  
IPv6 CIDRブロック：ブロック無し  
テナンシー：deforuto



2. サブネットを作成する
  * パブリックサブネットを作成する
  * プライベートサブネットを作成する
3. ルーティングを設定する
  * インターネットゲートウェイを作成し、VPCにアタッチする
  * ルートテーブルを作成し、パブリックサブネットに紐づける

# 2019/7/29
## 業務
### 目標
1. テストケースを考えてから実装する(なんちゃってTDD)
2. デザインパターン 「**State**」の理解と実装

## java
### Stateパターンについて  
[参考]https://qiita.com/hikao/items/e29c08d3a82bc0827a62
* 「状態」とそれに依存する処理を取りまとめたクラスを作り、ロジックから分離させる

### 概要  
1. 指定された図書について、書籍の貸し出し状況を表示するMyApp
```java
class MyApp(){
 
  public void displayBookStatus(Book book){
    if (book.state == "exist") {
      // 在庫あり
      system.out.println("exist");
    }
    if (book.state == "lent") {
      // 貸し出し中
      system.out.println("lent")
    }
  }
}
```
2. 書籍の貸し出し状態として「所在不明」を追加したいとき、  
MyAppクラスを編集しなければならない。  
※Bookにもstateを追加する必要がある。
```java
class MyApp(){
 
  public void displayBookStatus(Book book){
    if (book.state == "exist") {
      // 在庫あり
      System.out.println("exist");
    }
    if (book.state == "lent") {
      // 貸し出し中
      System.out.println("lent")
    }
    if (book.state == "missing") {
      // 所在不明
      System.out.println("missing");
    }
  }
}
```
3. MyAppクラスを編集すると、既存の処理に対する影響を考えて再テストが必要になる。  
そこで、以下のように書籍の「状態」を分離する。
```java
interface State {
  void displayState();
}
```
```java
class Exist implements State {
  @Override displayState(){
    System.out.println("exist");
  }
}
```
```java
class Lent implements State {
  @Override displayState(){
    System.out.println("lent");
  }
} 
```
```java
class Missing implements State {
  @Override displayState(){
    System.out.println("missing");
  }
}
```
* 上記にてクラス単位に分離した「状態」を取りまとめ、保持するクラスを用意する
```java
class Context {
  
  // 状態を保持するクラス変数
  private State state;

  public Context(){
    Exist exist = new Exist();
    Lent lent = new Lent();
    Mising missing = new Missing();
    
    // 初期設定
    this.state = exist;
  }
  
  /**
   * 状態の変更
   */
  public void changeState(String state) {
    switch (state) {
      case "exist":
        this.state = exist;
        break;
      case "lent":
        this.state = lent;
        break;
      case "missing":
        this.state = missing;
        break;
      case default:
        break;
    } 
  }
  
  /**
   * 処理のディスパッチ
   */
  public void displayState() {
    // このメソッドが呼ばれた時点で保持している「状態」のdisplayState()を実行する
    this.state.displayState();
  }
}
```
以上の機構を利用したMyApp
```java
class MyApp {
  Context context = new Context();
  context.displayState();
  // existが出力
  
  context.changeState("lent");
  context.displayState();
  // lentが出力
  
  context.changeState("missing");
  context.displayState();
  // missingが出力
}
```
状態が追加されても既存のロジックに影響を与えないため、  
テスト範囲を限定することができる。


## 統計
### 用語
* "|"：バーティカルバー  
P(A|B)の意味：「Aという条件の下で事象Bが発生する確率P」  

## AWS
### メモ
* CloudTrailのS3に対するログ配信は一旦オフにした（証跡を削除）  
※3日で無料使用枠切れそうなので。  

---
# 2019/7/28
## django
```python

```
* 例外処理をロジックとして使わないで欲しい。

# 2019/7/26
* 朝起きたらノートパソコンが机の上で水没して死んでた...
## 業務
### 目標
 1. ~~パソコン買う~~
 2. ~~化石PCにLinux mintを入れる~~  
 https://www.toma-g.net/entry/linux_mint_18_x205ta
3. X205TAはLinux mintと相性が悪いので、  
Ubuntu16にした。  
http://jyuvaworks.blue/2019/01/05/post-360/
```
$ LANG=C xdg-user-dirs-gtk-update
```
## AWS
### (1)初期設定
#### (1.1)AWSのアカウントを作成する
#### (1.2)料金アラートを設定する(CloudWatch)  
##### 概要
〇円以上の従量課金が発生したときに通知する。  
1. 請求アラートを有効にする  
 1. マイ請求ダッシュボードを開く
 2. Billingの設定
 3. 「請求アラートを管理する」

2. CloudWatchで通知設定をする
 1. 「請求」
 2. 「アラームの作成」

#### (1.3)作業ユーザーを作成する
* IAMユーザー
 * 初期作成されるルートユーザーは作業で使用しないのが  
 ベストプラクティスであるため。
 * IAMユーザーはAWS内で作成する。  
 ユーザーごとに権限を割り振ることができる。  
 ログも個別に追跡できる。  
1. 「マイアカウント」の「IAMユーザー／ロールによる請求情報へのアクセス」  
デフォルトは無効になっているので、「編集」から有効にチェックする。  
2. 上部「サービス」から「IAM」で検索を行う
3. 左部「ユーザー」
 * AWSアクセスの種類：マネジメントコンソールへのアクセスを有効
 * （T**0)
 * パスワードのリセットは、自分用なので不要
 * ↑ほかの人用を作る場合はチェックしておく
4. アクセス許可の設定  
 * 既存のポリシーを直接アタッチ
5. タグの追加は不要
6. 作成完了時、ログイン用のURLが発行される。
#### (1.4)操作ログを記録する
* CloudTrail  
 * いつ、だれが、何をしたか追跡する。  
 複数人での開発で重要。  
 S3へログを保存することで、データを永続化できる。  
 ※CloudTrailの追跡は90日のみであるため。
* CloudTrailは無料で、S3は有料
1. 上部「サービス」から「CloudTrail」で検索を行う
2. 証跡の作成
3. 全てのリージョンに適用→「はい」
4. 管理イベント：記録のトリガー→「すべて」
5. データイベント
6. S3バケットの作成：フォルダのようなもの  
バケット名はURLになるため、全S3内でユニークである必要がある。  
https://dev.classmethod.jp/cloud/amazon-s3-bucket-name-rule/


#### (1.5)用語  
* マネジメントコンソール：ログイン後に表示されるAWSの管理画面
* IAM(アイアム)ユーザー：ルート権限を持たず、個別に権限設定した作業用ユーザー
* S3(エススリー)：データストレージサービス(有料)
* S3バケット：ストレージの名前（フォルダ名みたいなもの）

### (2)VPCネットワークの構築
#### (2.1)AWS VPCとは
* AWSのサービスの一つ。  
Virtual Private Cloud で「VPC」  
クラウド環境で仮想ネットワークを構築する。  
AWSで使用するリソースは。このVPCへ配置していくことになる。  
[参考]https://qiita.com/uenohara/items/f7c11995568030d79533
* 必ず１つのリージョン内で構築される。  
（VPC作成時に指定する)
つまり、複数リージョンをまたぐようなネットワークは不可である。  
* アベイラビリティゾーンをまたぐことで冗長化を行う。 

#### (2.2)AWSのネットワークについて
* VPCの中に「パブリックサブネット」「プライベートサブネット」を構築する  
前者は「インターネットから直接アクセスできる」場所(公開)  
Webサーバーを配置する。  
後者は「特定のルートでしかアクセスできない」場所(非公開)  
DBサーバーを配置する。  

##### リージョン
AWSの各サービスが提供されている地域のこと。  
AWSでサービスを利用する際は、まずこの「リージョン」を選択する。
つまり、サービスの利用単位といえる。  
使えるサービスがリージョンによって異なるため、  
自分の使いたいサービスがあるかどうかを確認する必要がある。  
基本的に、アメリカのリージョンで先行して機能が利用可能になり、  
他リージョンでも随時使えるようになっていく形態とのこと。  
 * 具体例  
  1. (1)初期設定のログ監視でCloudTrailとS3の利用を開始した。  
  2. CloudTrailは「北アメリカ」のリージョンでしか提供していない
  3. S3は東京リージョンでも提供している
  4. なので、(1)では監視するサービスを北アメリカリージョンで、  
  そのログファイル出力先となるデータストレージサービスを日本リージョンで利用していることになる。  
  * なお、地域が離れるほど応答時間が掛かる。
##### アベイラビリティゾーン
* クラウド環境のリソースを提供するデータセンターのグループを「アベイラビリティゾーン」と呼ぶ。
* リージョンは、この「アベイラビリティゾーン」を組み合わせることで構成されている。
* どのリージョンにもアベイラビリティゾーンは2つ以上存在しているため、  
１つのアベイラビリティゾーンが利用不可になっても、システムの稼働が継続できる。  

#### サブネット
* ネットワークを分割する概念。  
一つのネットワーク(ここではVPC)について、  
例えば「この範囲は公開、この範囲は非公開」としたい場合に、  
複数のネットワークではなく、一つのネットワークの中で切り分けたい場合に  
サブネットを使用する。  
AWSでサーバーを配置する場合、サブネット内に配置していくことになる。  

#### (2.3)ネットワークのIPアドレス
* 用意するネットワーク３つ
 1. VPC(リージョン)
 2. パブリックサブネット
 3. プライベートサブネット  
まずは、この３つのIPアドレスを決める必要がある。


***
# 2019/7/25
## 業務
### 目標
1. テストコードを書いてローカルで動かす。  
 → テスト自動化のイメージを作る。  
2. デザインパターン 「**Itarator**」の理解と実装

* DP学習進捗
1. Builder_20190716
2. Abstract Factory_20190717
3. Adapter_20190719
4. Chain of Responsibility_20190722
5. Facade_20190723
6. Prototype_20190724

## java
### メモ
* switch文の条件式と3項演算子  
**switch(null)はぬるぽになる**  
※switch(<String>)は、内部でString#equals()しているらしい。  
```java
// NullPointerException
final String condition = null;
switch(condition){
 case foo:
 case baa:
   // すごい処理
   break;
 default:
   // 処理
}
```
**コーディング規約で3項演算子が禁止されていないなら**、下記が良い感じでは。
```java
// 7/23の応用
final String condition = null;

// 条件式にnullが入った場合は空文字へ変換
switch(condition == null ? "" : condition){
 case foo:
 case baa:
   // すごい処理
   break;
 default:
   // 処理
}
```

### Iteratorパターンについて  
[参考]https://53ningen.com/iterator-pattern/
* 「コレクション」に分類されるデータ構造に対して、抽象的な走査手段を提供する
* プログラムを仕様変更に強くする典型（と捉えた）

### 概要  
1. 現行の図書検索システムMyAppは「配列」で本を管理していた
```java
class MyApp {
    
    private Book[] books;
    
    // シーケンスで本を走査する
    public Book checkBooks(int seqence){
        for (int i = 0; i < books.length(); i++ ) {
            if (i == sequence) {
                return this.books[i];
            }
        }
    }
}
```
2. 新しい図書検索システムとして、「辞書」で本を管理したいらしい。  
しかし、辞書型はインデックス番号を持たないのでforループが使えない。  
→ MyAppクラスを改修しなければならない...。  

```java
    private HashMap<String, Book> books;
    
    // 今までの方法じゃ走査できない...
    /** 
    public Book checkBooks(int seqence){
        for (int i = 0; i < books.length(); i++ ) {
            if (i == sequence) {
                return this.books[i];
            }
        }
    }
    */
```
3. データ構造と走査方法が密に結合していると、上記のような問題が起こる。  
そこで、これらを分離する方法を考える(いわゆる「疎結合」)  
用意するべき要素は以下の通り。  
    * 本をまとめ、順番や数を管理する自前の拡張コレクション(Aggregate)
    * Aggregateを走査する機能を提供するinterface(Iterator)

汎用的な概念のため、抽象クラスを用意
```java
interface Aggregate {
    public abstract Iterator iterator();
}
```
```java
interface Iterator {
    public abstract boolean hasNext();
    public abstract Object next();
}
```
「本の管理に特化したクラス」を、上記抽象クラスの具象クラスとして実装
```java
class BookShelf implements Aggregate {

    // 配列を拡張するイメージ
    private Book[] books;
    private int last = 0;
    
    public BookShelf(int maxsize) {
        this.books = new Book[maxsize];
    }
    
    public Book getBookAt(int index) {
        return books[index];
    }
    
    public void appendBook(Book book) {
        this.books[last] = book;
        last++;
    }
    
    public int getLength() {
        return last;
    }
    
    @Override
    public Iterator iterator() {
        return new BookShelfIterator(this);
    }
}
```
「本の管理に特化したクラス」を使用するためのIterator具象クラス
```java
class BookShelfIterator implements Iterator {
    private BookShelf bookShelf;
    private int index;
    
    public BookShelfIterator(BookShelf bookShelf) {
        this.bookShelf = bookShelf;
        this.index = 0;
    }
    
    @Override
    public boolean hasNext() {
        if (index < bookShelf.getLength()) {
            return true;
        } else {
            return false;
        }
    }
    
    @Override
    public Object next() {
        Book book = bookShelf.getBookAt(index);
        index++;
        return book;
    }
}
```
以上の機構を利用するMyApp。  
コレクションの構造はBookShelfに切り分けされているので、  
もしその使い方が変わってもMyAppではwhile(bookShelf.hasNext())するだけでOK  
```java
class MyApp {
    
    BookShelf bookShelf = new BookShelf(5);
    
    bookShelf.appendBook(new Book());
    bookShelf.appendBook(new Book());
    bookShelf.appendBook(new Book());
    bookShelf.appendBook(new Book());
    bookShelf.appendBook(new Book());
    
    Iterator iterator = bookShelf.iterator();
    
    while (iterator.hasNext()) {
        Book book = (Book)iterator.next();
        // 本に対する処理
    }
}
```
### 利点など
* 正直なところ、あまり理解できていない。  
ひとまず、ループ処理を「ループ実行側」ではなく「被ループ側」に  
依存させることで、データ構造の変化を吸収しやすくする、と考えておく。
* つまり、「ループの実行方法」は変えずに「ループの挙動」を変更させることができる。  
※コレクションのダックタイピング

## AWS  
Amazon Web Services  
クラウド上でサーバーやネットワークを構築できる。

* インフラができるようになるメリット  
    1. 自分でサービスがリリースできる
    2. テスト環境の構築ができる
    3. 障害が起きたときに問題の切り分けができる
    4. 課題に対して、システム全体で対応策が考えられる

* インフラ構築で考えること
    1. どのようなサーバーが必要か
    2. どのようなネットワークが必要か

* AWSの特徴
    1. サービスが豊富
    2. リソースが柔軟
    3. 従量課金

* インフラ関連の用語について
    1. インフラとは  
    サーバーやネットワークのこと。  
    Infrastructure:基盤  
    システムやサービスの基盤となる設備を指す。
    
    2. サーバーとは  
    クライアントへサービスを提供するコンピューターのこと。  
    クライアントとは、サーバーの対してリクエストを送る主体のこと。  

    3. ネットワークとは  
    複数のコンピューターを繋いで、データを送受信できるようにするもの。 
    
    4. クラウドとは  
    ネットワークを利用してコンピューターリソースを利用する形態のこと。  
    利点は、初期コストが少なく、すぐに利用開始が可能で、  
    サーバーの増減が自由にできること。  
    欠点は、費用の予測が付きにくいことと、  
    クラウド全体で障害が起こると対応しようがなくなること。  
    対義語は「オンプレミス」
    
    5. オンプレミスとは  
    インフラを自前で用意して、自社で所有・管理すること。  
    利点は、設定などの自由度が高いこと。  
    欠点は、初期コストがかかり、調達期間が必要で、  
    サーバーの増減がしにくいこと。  




# 2019/7/24
## 業務
### 目標
1. 建設的な取り組みが出来ているかを自省する。  
    * 結論・目的ベース
    * その行動によって誰が得するのか
    * その行動によって誰に負担が行くのか。それに見合うメリットはあるのか
2. デザインパターン 「**Prototype**」の理解と実装

* DP学習進捗
1. Builder_20190716
2. Abstract Factory_20190717
3. Adapter_20190719
4. Chain of Responsibility_20190722
5. Facade_20190723

### Prototypeパターンについて  
[参考]https://www.techscore.com/tech/DesignPattern/Prototype.html/
* 一度生成したインスタンスをコピーして使いまわす
* ざっくり言うと、new()を使わずにインスタンスを生成する

### 概要  
1. RoboControllerは10体のロボットを使役する
```java
class RoboController {
    public void execute(){
        Robot robo01 = new Robot();
        Robot robo02 = new Robot();
        Robot robo03 = new Robot();
        Robot robo04 = new Robot();
        Robot robo05 = new Robot();
        Robot robo06 = new Robot();
        Robot robo07 = new Robot();
        Robot robo08 = new Robot();
        Robot robo09 = new Robot();
        Robot robo10 = new Robot();
    }
}
```
2. RoboControllerクラスには１つ問題があった。  
Robotを組み立てる工程が複雑であり、10体揃えるために時間が掛かってしまう。  
```java
class Robot {
    public Robot(){
    
        // action001-689が組み立てるためのメソッド
        // 1メソッドあたり4秒程度かかる、コストの重い処理
        action001(){}
        action002(){}
        //
        // ...
        //
        action689(){}
    }
}
```
3. 対策として、Robotの完成品をコピーすることで10体を揃えるようにする。  
こうすることにより、9体分の組み立てコスト削減が見込める。  
オブジェクトのディープコピーは、以下のような実装となる。  
```java
// ディープコピーをサポートしているjava標準のinterface
import java.lang.Cloneable;

interface RobotPrototype extends Cloneable{
    
    // コピー用の抽象メソッド
    public abstract RobotPrototype createClone();
    
}
```
上記Cloneableを継承したinterfaceの実装クラスRobot    
Cloneableを継承していなければ例外が発生するため、キャッチする必要がある。
```java
class Robot implements RobotPrototype {
    
    public Robot(){
    
        action001(){}
        action002(){}
        //
        // ...
        //
        action689(){}
    }
    
    @Override
    public RobotPrototype createClone() {
        RobotPrototype proto = null;
        try {
            proto = (RobotPrototype).clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return proto;
    }
}
```
一度生成したインスタンスを保持、コピーするClientクラスを用意する
```java
class Client {

    // インスタンス名とインスタンスを辞書として保持する
    private hashMap<String, RobotPrototype> registry

    public Client(){
        this.registry = new HashMap<>();
    }

    // 辞書へインスタンスを保存するメソッド
    public void register(String name, RobotPrototype proto) {
        this.registry.put(name, proto);
    }
    
    // インスタンスをコピーするメソッド
    public RobotPrototype create(String name) {
        RobotPrototype proto = (RobotPrototype) registry.get(name);
        return proto.createClone();
    }
}
```
以上で実装した機構を利用する実行クラスRoboController
```java
class RoboController {
    public void execute(){
        
        // Clientの生成
        Client client = new Client();
        
        // 1体目のRobot生成
        Robot robo01 = new Robot();
        
        // 1体目のRobotをClientに登録
        client.register("proto", robo01);
        
        // あとはコピーを生成
        RobotPrototype robo02 = client.create("proto");
        RobotPrototype robo03 = client.create("proto");
        RobotPrototype robo04 = client.create("proto");
        RobotPrototype robo05 = client.create("proto");
        RobotPrototype robo06 = client.create("proto");
        RobotPrototype robo07 = client.create("proto");
        RobotPrototype robo08 = client.create("proto");
        RobotPrototype robo09 = client.create("proto");
        RobotPrototype robo10 = client.create("proto");
    }
}
```
### 利点など
* 利点は上述の通り。めちゃくちゃ便利そう。
* Abstract Factoryパターンは「複数のFactory」を作るデザインなので、  
同じようなFactoryを量産する場合にPrototypeを組み合わせられそう。
* Clientで「辞書として保持」しているのが好み

## thymeleafについて
* 利点：サーバーが起動していなくても静的なhtmlとして読み込むことができる

## エディタ
### VSCode
* Ctrl + B で左サイドバーの開閉
* Ctrl + Tab でエディタのタブ切り替え

## python週次

```python
# フォーマット文字列リテラル
testDict = {'foo':1, 'baa':2, 'qux':3}
for x , y in testDict.items():
    print(f'Key:{x}, Value:{y}')
```
```python
# generator式
generator = ((x, y) for x, y in zip(range(1, 1000), range(200, 300)))
for x, y in generator:
    print(x,y)

# generator関数
def generate_ints(number):
    for index in range(number):
        yield index

genfunc = generate_ints(3)
genfunc.__next__()
```
yeildはreturnと同じように値を返すが、**オブジェクトの状態を保持する**  
→続きから値を取得することができる  
```python
def generate_ints(number):
    multiple = 1
    for index in range(number):
        value = (yield index * multiple)

        if value is None:
            multiple = 1
        else:
            multiple = value

if __name__ == '__main__':
    funcgen = generate_ints(3)
    print(next(funcgen))
    print(funcgen.send(10))
    print(funcgen.send(-10))
```


# 2019/7/23
## 業務
### 目標
1. 相手が何を求めているのか把握してからレスポンスを返す
2. ~~デザインパターン 「**Singleton**」の理解と実装~~  
→非推奨らしいのでやめた。  
**なぜシングルトンがだめなのかが、  
単体テストの考え方も踏まえて非常にわかりやすい記事。↓  
http://blog.yuyat.jp/archives/1500**  
・シングルトンは実質的に「オブジェクトをグローバルで共有する」こと  
・シングルトンなオブジェクトを使用しているモジュールは、オブジェクトの状態に影響を受ける。  
・まっさらな状態のシングルトンオブジェクトを用いたテストが不可能になる。  
→そもそもシングルトンオブジェクトに状態を持たせることが間違いという意見も。。  
・まとめると、「シングルトンオブジェクトを使えば使うほど、テストの見通しが悪くなる」  
・あえてシングルトンにしなければならないパターンはそこまで多くない  
2. デザインパターン 「**Facade**」の理解と実装

* DP学習進捗
1. Builder_20190716
2. Abstract Factory_20190717
3. Adapter_20190719
4. Chain of Responsibility_20190722

## java
### メモ
* 標準メソッドの適切なラップ
```java
// String.equals()のぬるぽ回避
exEquals(String str1, String str2){
    return str1 != null ? str1.equals(str2) : str2 == null;
}
// これは他のString.〇〇にも使える
```
### Facadeパターンについて  
[参考]https://qiita.com/mk777/items/071ec30a1f7231029aae
* 個々の処理をひとまとめにする窓口クラスを作る
* Abstract FactoryパターンはFacadeの具体例の一つといえる

### 概要  
1. ヘッダー→本文→フッターという順番でメールを組み立てる機能があるとする
```java
class MyApp {
    
    public String createMail(){
        MailMaker maker = new MailMaker();
        maker.addHeader();
        maker.addBody();
        maker.addFooter();
    }
}
```
```java
class MailMaker {
    // ヘッダーを作る
    public String addHeader(){};
    // 本文を作る
    public String addBody(){};
    // フッターを作る
    public String addFooter(){};
}
```
2. 上記のMyAppクラスには問題がある。  
* プログラム実装者が「ヘッダー」⇒「本文」⇒「フッター」という順番で  
メソッドを呼び出すことを知らないかもしれない。
* 呼び出す順番を変えたり、間に別のメソッドを挟み込みたくなった時に、  
MyAppクラスを編集しなければならない。

3. そこで、処理の呼び出し口としてのFacadeクラスを作成する
```java
class MyApp {

    public void execute(){
        MailMakeFacade facade = new MailMakeFacade();
        facade.createMail();
    }
}
```

```java
class MailMakeFacade(){
    public String createMail(){
        MailMaker maker = new MailMaker();
        maker.addHeader();
        maker.addBody();
        maker.addFooter();
    }
}
```

```java
class MailMaker {
    // ヘッダーを作る
    public String addHeader(){};
    // 本文を作る
    public String addBody(){};
    // フッターを作る
    public String addFooter(){};
}
```

### 利点など
* 上記の例はシンプルなのであまり実感できないが、  
実際に複数のサブシステムを通過してデータを作成していく大規模なシステムの場合、  
処理を呼び出す順番を実装者にいちいち考えさせるのは非効率であり、またバグの温床となる。  
(呼び出す順番の実装ミスなど)  
* 窓口を設けることによって処理内容とクライアントを疎結合にできるため、  
プログラムの柔軟性からみてもプラスとなる。
* 「Abstract FactoryはFacadeの具体例」というのは、  
Factoryクラスが「インスタンス生成処理をひとまとめで請け負う窓口」という意味で  
ユーザーが「とりあえずFactoryを使っておけばインスタンス生成ができる」と考えることができるから。

## javascript
* 最新の構文に関する良さげな資料があった  
https://jsprimer.net/

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

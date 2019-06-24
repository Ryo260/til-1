###演算子
  床除算演算子：// 
    除算演算子の結果を超えない最小の整数
    ex) 3//2  >>> 1
        -3//2 >>> -2
  累乗：** 
    ex) 3**3 >>> 27
        8 ** (1/2) or 0.5 >>> 2
        
  複素数の表現：
    ex)   z = 2 + 3j
    note) complex型
          実部の取得はz.real
          虚部の取得はz.imag
          変数の初期化はz = complex(2,3)でも可
          共役複素数(虚部の符号を反転した複素数)はz.conjugate()で取得
          複素数の大きさ(√(実部^2+虚部^2))はabs()でも取得可能

###構文
  インポート宣言：
    ex) From HOGE import FOO：
        HOGEモジュールからFOOクラスをインポートする
  例外ハンドリング
    ex)
    try:
      #すごい処理
    except HogeError:
      #HogeErrorの場合の例外処理

###モジュール
  fractions[std]：
    分数を扱う。
    ex)   Fraction(3,4)...3/4を表す
    note) doubleと演算するとdoubleへ型変換
          intと演算するとfractions.Fractionのまま

###関数
  type(arg)：
    argの型を返却する
  
  complex(arg1, arg2)：
    arg1を実部、arg2を虚部とした複素数を返却
  
  複素数.conjugate()：
    共役複素数を取得
  
  input()：
    ユーザ入力を受け取る
  
  fractions.Fraction(arg1, arg2)：
    arg1/arg2という分数を表現する
    
    文字列フォーマット
      '{1}は{2}で{3}'.format('hoge', 'foo', 'bar')
      >>> hogeはfooでbar
      note)引数が数値でも置換してくれる

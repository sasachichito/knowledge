# PHPのインストール
パッケージでインストールするか、ソースからビルドする。  
ソースからビルドする場合、オプションを指定することでカスタマイズできる。  

https://qiita.com/dksatou/items/23c2a2567540eebc0ed9

# ライブラリ
|ライブラリの種類|配布サイト|インストーラ|備考|
|:--|:--|:--|:--|
|PHP言語で書かれたライブラリ|PHER (http://pear.php.net/) |pear|Composerに移行され、現在は未使用|
|PHP言語で書かれたライブラリ|Packagist (https://packagist.org/)	|Composer||
|C言語で書かれたライブラリ|PECL (https://pecl.php.net/) |pecl||

後者の「C言語で書かれたライブラリ」のことを「エクステンション」という。  


### PEAR (PHP Extension and Application Repository) はPHP言語で書かれたライブラリを提供するリポジトリの名前であり、そのパッケージ管理ツールのpearのことでもある。ペアと発音する。  

pearによるインストールはオペレーティング・システム上でグローバルに保管される欠点がある。  
つまり稼働している既存アプリケーションを新サーバーにをデプロイするとき、新サーバーで新たに依存ライブラリを用意しなければならない。  

その解決策としてComposerが開発された。

### PECL(PHP Extension Community Library)はPHPエクステンションを配布するWebサイトの名前であり、パッケージング管理ツールのpeclのことでもある。ピクルと発音する。peclコマンドは内部でpearを利用している。  
※PHPエクステンション(拡張モジュール)... PHPの機能を拡張するためのモジュール。 

PHPエクステンションを読み込む方法は2通りあり、  
PHPのコンパイル時に静的に組み込む(http://php.net/manual/ja/install.pecl.static.php)  
か、so(dll)ファイルとして読む込むかである。  

so(dll)ファイルとして読み込む場合、php.iniで指定するか、スクリプトの中でdll()関数を用いる。  
現在ロードされているモジュールは、get_loaded_extensions()関数を使うことによって取得できる。  

##### pecl install (so(dll)として読む込む)
`pecl install`は「*.soファイルを生成して所定の場所(extension_dir)に置く」という動作をしている。  
(extension_dirがどこかわからない場合はphp -i | grep -i extension_dirで調べられる。)  

`pecl install hoge`した時を考えると、以下の作業が行われる。  
```
パッケージのソースコード一式(tar.gz)をダウンロード
↓
tar.gzを解凍
↓
ビルド
↓
生成されたhoge.soファイルをextension_dirにコピー
```
上記を手動で実施することもできる。  

・手動ビルドの手順(memcachedエクステンションの例)  
1. 依存ライブラリの解決
```
# エクステンションが依存するライブラリを入れておく
$ sudo yum install libmemcached-devel
```
2. エクステンションのソースコードを取得  
ソースの場所はPECLサイトのmemcachedのページ (https://pecl.php.net/package/memcached) にて、”Browse Source” というリンクを辿る。  
```
# エクステンションのソースコードを取得
$ git clone -b php7 --depth 1 https://github.com/php-memcached-dev/php-memcached
```
3. configureスクリプトの作成【参考】http://engineering.otobank.co.jp/entry/2015/02/19/124500  
phpize... PHPに静的に組み込みなおす必要なく、エクステンションだけをコンパイルできるconfigureスクリプトを作成できるコマンド【参考】http://hlis-toproad.com/blog/2015/02/24/338/
```
$ cd php-memcached
$ phpize
```
4. configureスクリプト実行しMakeファイルを作成
```
$ ./configure
```
5. Makeファイルを使ってコンパイル  
```
$ make
```
6. 生成された./modules/memcached.so をextension_dirにコピー  
```
$ sudo make install
```

【参考】http://dqn.sakusakutto.jp/2015/07/php_extension_pecl_phpize.html

似た名前のPharとは？
phar拡張モジュールは、pharストリームラッパーおよびPharクラスを提供し、PHPアーカイブ(phar)ファイルを操作することができる。  
Pharクラスを使用すると、pharファイルの作成や展開、そしてその中身を順に処理することなどができる。   
【参考】https://www.weblio.jp/content/Phar  

# phpの設定確認
```
# php -r 'phpinfo();' | grep conf.d
```

# namespace   
名前空間という。   
phpプロセスが扱う論理的な階層構造。   
phpがスクリプトを処理するとき、まずはそのスクリプトのnamespace宣言からカレント名前空間を設定する。(namespaceが省略されていたらカレントはルート名前空間（グローバル名前空間）に設定する)   
スクリプトにクラス・インターフェース・関数・定数の定義があれば、カレント名前空間に配置する。   
クラス・インターフェース・関数・定数の呼び出しがあれば、どの名前空間のものかを示す絶対パスか相対パスがそれぞれに記載されているので、   
絶対パスであればグローバル名前空間を、相対パスであればカレント名前空間を起点に読み込む。(名前空間が省略されていたらカレント名前空間から読み込む）   
   
# use   
呼び出しで名前空間を記載するのを簡単にするために、   
名前空間・クラス・インターフェース・関数・定数のエイリアスを作成する。(asキーワードを省略した場合は、パスの最後の名前でエイリアスを作成したことになる)   
これは呼び出し宣言で利用される。   
   
# require   
PHPにおけるインクルード。   
phpがスクリプト処理前にrequire宣言を見つけ、そこに物理的にファイルを結合する。  

# リテラル
リテラルとは... 変数への代入値や、関数・メソッドの引数に指定するデータのソースコード表現。  
※ソースコード表現とは、コードの中で、ある値を指定するために用いる表現のこと。  
例)  
`a = '10';`  
等式が正しければaには「'10'」という4文字が代入されるが、実際は文字列の「10」が入っている。  
また、phpではif()の中にture,false以外も受け付けるが、0を入れると、0はfalseのソースコード表現のため分岐に入らない。  

# オートローダー
オートローダーとは... spl_autoload_registerという名前の関数のこと。引数にコールバック関数を指定する。  

どのような関数か？  
オートローダーを定義してるphpファイルで、未定義のクラスをインスタンス化すると呼び出される。  
必要なphpファイルをインクルードしてくれる。  
※オートローダーに渡すコールバック関数に引数としてインスタンス化したいクラスの完全修飾名が渡されるので、ここでrequire_onceするという仕組み。  

ポイントは、オートローダー関数を全てのphpファイルでrequire_onceしなければならない点で、非常に面倒である。  

そこでphp.iniで`auto_prepend_file = 「オートローダー関数を定義したphpファイル」`と設定しておくとそのサーバーの全てのphpファイルに自動でインクルードしてくれる。  

しかし、サーバー全体では影響が大きすぎるので、Apache標準の設定ファイル`.htaccess`で指定のディレクトリ配下のphpファイルにのみ適用することもできる。  

引数のコールバック関数でクラスのインクルード方法をカスタマイズすることで独自のオートローダーを作成できるが、ファイル名や名前空間のルール(ロード方式)を決めておかないと混乱の元となる。  

Composerを使って、オートローダー関数を自動で作成できる。手順は以下  
【参考】  
https://qiita.com/atwata/items/5ba72d3d881a81227c2a
http://blog.comnect.jp.net/blog/122  
```
1. composer.jsonにロード方式とロード対象を記載する
 ロード方式は、PSR-4, PSR-0, classmap, files がある。
 ・PSR-4... 名前空間に対応するベースディレクトリを指定
 ・PSR-0... 指定した名前空間ディレクトリがベースディレクトリ内に存在する
 ・classmap... 読み込みたいディレクトリやファイルパスを指定
 ・files... ?

2. composer update(あるいはcomposer dump-autoload)を実行する
 ロード方式ごとに、ロード対象定義ファイルが作成される。
 ・PSR-4... autoload_psr4.php
 ・PSR-0... autoload_namespaces.php
 ・classmap... autoload_classmap.php
 ・files... autoload_file.php(?)

 このタイミングでautoload.phpという、オートローダー関数をrequireするファイルが作成される。

3. オートローダーを利用したいクラスで「require '/vender/autoload.php'」する
　　　　↓
 autoload.phpがrequireされると、autoload.php内で「require /vender/composer/autoload.php」される
　　　　↓
 autoload_real.phpのgetLoaderスタティックメソッドがコールされる
　　　　↓
 ¥comopser¥Autoload¥ClassLoaderのインスタンスを生成し、ロード対象定義ファイルの内容をプロパティとしてセットし、
 ClassLoaderのregisterメソッドをコールする
　　　　↓
 registerメソッドでオートローダー関数のコールバック関数にClassLoaderのloadClassメソッドを定義する
 ※loadClassメソッドは、ClassLoaderのプロパティとしてセットしたロード対象定義ファイルを利用して、クラスをインクルードする

```
![PHP](/picture/PHP.png "PHP")


# Composer

Composerはcomposer.jsonファイルに書き込んだり、読み込んでライブラリをダウンロードしたりする。  

composer.jsonの中身は大きく2分されていて、requireとrequire-devで構成されている。  
require-devだけに操作を適用したいときは--devをつける。
comoser.jsonには、ダウンロードしたいライブラリ名とバージョンを記載するが、バージョンは固定ではなく、  
「最新のもの」や、メジャーバージョンのみの指定も可能。  

操作する流れは以下  
「composer require」でcomposer.jsonに追記する  
「composer remove」でcomposer.jsonから消去する  
↓  
「composer update」でcomposer.jsonを読み込み、ライブラリをインストールする。  
実際にダウンロードした各ライブラリ名とバージョンをcomposer.lockに書き込む。  
デフォルトで--dev付き  
↓  
「composer install」でcomposer.lockを読み込み、まったく同じバージョンをダウンロードする。  
デフォルトで--dev付き  

update や install でglobalオプションをつけると、$COMPOSER_HOME/vender/bin配下にダウンロードする  

「composer dump-autoload」はオートローディングに関する情報ファイルを生成するコマンド。  
【参考】https://qiita.com/eidera/items/3e0b2b41253e1563be46  

# PHPUnit
Mockを使用する場合、  
PHPUnit標準のTestCaseクラスが提供するモック作成メソッドと  
ComposerでインストールできるPHPライブラリMockeryを利用する方法がある。  
  
## Mockery  
【参考】https://kore1server.com/202/Mockery+0.8.0+%E6%97%A5%E6%9C%AC%E8%AA%9E%E3%83%89%E3%82%AD%E3%83%A5%E3%83%A1%E3%83%B3%E3%83%88#creating-partial-mocks  
モックの種類は6つ  
- 名前付きモック  
パラメーターの名前がついたモックオブジェクト  
- クラスモック  
パラメーターに指定した名前がクラス名の場合に、そのクラスを継承したモックオブジェクトが作成される。  
指定のクラスがfinalである場合と、メソッドにfinalが指定されている場合は、挙動を設定しても無視される。  
- エイリアスモック  
指定したクラス名のエイリアスを、stdClassに対して作成して、そのstdClassオブジェクトを返却する。  
一般的にはpublic staticメソッドをモックできるように使用する。  
指定するクラス名は、その段階でまだロードされていない必要がある。  
@runInSeparateProcess  
@preserveGlobalState disabled  
の二つのアノテーションをつけると、別プロセスでテストされるので解決される。  
```  
$mock = \Mockery::mock('alias:MyNamespace\MyClass');  
```  
- インスタンスモック  
エイリアスモックとほぼ同じ。  
エイリアスモックでは、一度モックに対して挙動を設定しても、新しくエイリアス名でnewすると、未設定状態のモックが返却されるのに対し、  
インスタンスモックでは、挙動を設定した後は、エイリアス名でをnewするたびに同じ設定のモックが返却される。  
```  
$mock = \Mockery::mock('overload:MyNamespace\MyClass');  
```  
- パーシャル（部分）モック  
一部のメソッドだけ挙動を設定できる。  
```  
$mock = \Mockery::mock('MyNamespace\MyClass[foo,bar]');  
```  
- プロキシ（代理）モック  
finalクラスやfinal付きのメソッドの挙動を設定する必要がある場合に利用する。  
しかし、型の名前が変わってしまうので、タイプヒントを利用しているとエラーになる。  
```  
$mock = \Mockery::mock(new Foo);  
```  
  
実装によっては、コンストラクタがプライベートになっている場合がある。  
その場合はリフレクションとclosure::bindをうまく利用してnewすることができる。  
【参考】https://qiita.com/mpyw/items/b3d974c073e6484d51a4  
コツは closure::bindでクロージャ（第一引数）をインスタンス（第二引数）に紐づけて、そのクラス内から実行するように設定（第三引数）していること。  
今回は上記のクロージャ内でプライベートコンストラクタを実行させたかったので、一旦リフレクションを使ってコンストラクタを使わずにnewしている。  
http://php.net/manual/ja/closure.bind.php


# 技術  
【参考】https://qiita.com/tricogimmick/items/23fb5958b6ea914bbfb5  
- コールバック関数  
他の関数やメソッドの引数として渡される、関数やメソッドを指す。  
- 可変関数  
コールバック関数を実現するための方法の一つ。コールバック関数を受け取る側は関数名(foo)を$aaa受け取り、$aaa()と表現することで、「foo()」を実行できる。  
- 無名関数（クロージャ）  
関数名を省略した関数定義のこと。これをコールバック関数として渡すことができる。  
  
Aクラスのaメソッドをコールバック関数にするため、aという文字列をBクラスに渡しても、可変関数では実行することはできない。  
なぜなら、Bクラスにはaメソッドは定義されていないから。  
そのため「Aクラスのaメソッド」ということが分かるように配列にして渡す。[$Aclass, 'a']  
受け取った側ではその配列をcall_user_func()に渡すことで実行できる。  
  
※コールバック関数を受け取る側でタイプヒントが利用可能である。型は callable。  
  

## デプロイ  
***ポイントはアプリケーションのルートディレクトリをシンボリックリンクで指定しておくこと。***  
- 手順  
１．Git Pushでソースをサーバーにアップロード  
２．deplyerやシェルスクリプトでライブラリ等準備（composer install)  
３．アプリケーションのルートディレクトリやログのシンボリックリンクの向き先を切り替える  

## データベースアクセス
![PHP2](/picture/PHP2.png "PHP2")

 

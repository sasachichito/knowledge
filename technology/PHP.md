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

2. composer update(あるいはcomposer dump-autoload)
```

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

Packagist - Composer用のpackage集約サイト  

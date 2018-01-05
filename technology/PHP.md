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

### PECL(PHP Extension Community Library)はPHPエクステンションを配布するWebサイトの名前であり、パッケージング管理ツールのpearのことでもある。ピクルと発音する。peclコマンドは内部でpearを利用している。  
※PHPエクステンション(拡張モジュール)... PHPの機能を拡張するためのモジュール。 

PHPエクステンションを読み込む方法は2通りあり、  
PHPのコンパイル時に静的に組み込む(http://php.net/manual/ja/install.pecl.static.php)か、so(dll)として読む込むかである。  

so(dll)として読み込む場合、php.iniで指定するか、スクリプトの中でdll()関数を用いる。  
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




phar拡張モジュールは、pharストリームラッパーおよびPharクラスを提供し、PHPアーカイブ(phar)ファイルを操作することができる。  
Pharクラスを使用すると、pharファイルの作成や展開、そしてその中身を順に処理することなどができる。   
【参考】https://www.weblio.jp/content/Phar  

# リテラル

# オートローダー

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

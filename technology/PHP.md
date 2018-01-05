# ライブラリ
|ライブラリの種類|配布サイト|インストーラ|
|:--|:--|:--|
|PHP言語で書かれたライブラリ|PHER (http://pear.php.net/) |pear|
|PHP言語で書かれたライブラリ|Packagist (https://packagist.org/)	|Composer|
|C言語で書かれたライブラリ|PECL (https://pecl.php.net/) |pecl|

後者の「C言語で書かれたライブラリ」のことを「エクステンション」という。  


### PEAR (PHP Extension and Application Repository) はPHP言語で書かれたライブラリを提供するリポジトリの名前であり、そのパッケージ管理ツールのpearのことでもある。  

pearによるインストールはオペレーティング・システム上でグローバルに保管される欠点がある。  
つまり稼働している既存アプリケーションを新サーバーにをデプロイするとき、新サーバーで新たに依存ライブラリを用意しなければならない。  

その解決策としてComposerが開発された。

### PECL(PHP Extension Community Library)はPHPエクステンションを配布するWebサイトの名前であり、パッケージング管理ツールのpearのことでもある。    

【参考】http://dqn.sakusakutto.jp/2015/07/php_extension_pecl_phpize.html


# 拡張モジュール PHPエクステンション
PHPの機能を拡張するためのモジュール。ダイナミックリンクライブラリ。  

配布先としては、PECLなどが有名。※ (PECL)   


php.iniでPHPの起動時にその拡張機能を読み込むことができ、また、スクリプトの中でdl()関数を用いることによって動的に読み込むことができる。  

現在ロードされているモジュールは、get_loaded_extensions()関数を使うことによって取得できる。  


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

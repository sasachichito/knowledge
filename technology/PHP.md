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

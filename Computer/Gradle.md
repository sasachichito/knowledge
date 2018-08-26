# gradle  
コマンド名。マシンにgradleをインストールすると利用できる。  
引数にビルドスクリプトと、ビルドスクリプトに定義された実行タスク名を指定する。  
  
# build.gradle  
build.gradleはGradleがデフォルトで読み込むビルドスクリプト。このスクリプトを実行する場合は  
gradleコマンドにはスクリプトファイル名を指定する必要はない。  
  
Java用のプラグイン等を指定すると、自動でBuild tasks などが追加される。	  
  
# gradlew  
Gradleの環境を閉じ込めたラッパーファイル群で、マシン本体にGradleが入っていなくてもGradleによるビルドを行えるようにするためのもの。  
gradleコマンドと同じように扱える。  
このファイルは必ずバージョン管理システムで管理する。  
  
# gradlew.bat  
gradlewコマンドのwindows用。  
  
# settings.gradle  
マルチプロジェクトの設定を書ける。  
https://qiita.com/shiena/items/371fe817c8fb6be2bb1e  
  

## ヒープメモリとガベージコレクション  
【参考】  
http://gihyo.jp/dev/serial/01/jvm-arc/0009


![JMV](/picture/JVM.png "JVM")

①〜④からどれかを選択してJVMを起動する

## ヒープの使用状況とGCの稼働状況  
#### ・JDK付属のjstatを利用する  
【参考】https://thinkit.co.jp/story/2011/03/25/2054?page=0%2C1  
```  
jstat -gcutil -h10 708 1000  
```  
形式は以下。  
```  
jstat -出力オプション（-h[ヘッダーを出力する行間隔]） [プロセス番号] [出力間隔（ミリ秒）]  
```  
  
## スレッドの状態  
#### ・JDK付属のjstackを利用する  
【参考】http://aoking.hatenablog.jp/entry/20120629/1340965676  
http://d.hatena.ne.jp/yohei-a/20150101/1420112104  
```  
jstack 708  
```  
形式は以下。  
```  
jstack [プロセス番号]  
```  
  
## GUIの利用  
#### ・JDK付属のjconsoleを利用する。  
【参考】https://qiita.com/frost_star/items/428cc9c2be4fc53deb91  
ヒープやスレッド、オブジェクト等様々な情報を取得できる。  
リモートのJavaプロセスにも接続できるので便利。  
  
#### ・JDK付属のjvisualvmを利用する  
【参考】https://jyn.jp/java-jvisualvm/  
jconsoleの上位互換といったところか。  
  
## 技術  
#### jpsでTomcatのプロセスIDが見つからない  
【参考】http://tksmd.hatenablog.com/entry/20110119/p1  
Javaプロセスを探すのに使う/tmp/hsperfdata_<ユーザ名>/<プロセスID> というファイルが、  
jdk6u23 以前は /tmp 以下に固定で生成されていたが、jdk6u23 では java.io.tmpdir で指定されるようになった事が原因。  
・対策  
指定しているディレクトリを見つけて、オプションで指定する。  
```  
jps -J-Djava.io.tmpdir=/usr/java/tomcat/temp/   
```  


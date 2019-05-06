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
#### ※システムスレッドについて  
http://argius.hatenablog.jp/entry/2014/12/16/142115  

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

## JVMとスレッド  
JVMはプロセスである。  
JVMが持つスレッドも、通常のプロセスとスレッドの関係に他ならない。  
  
**大切なのは、プロセスとスレッドの基本を知ること。**  
【参考】http://moro-archive.hatenablog.com/entry/2014/09/11/013520  
プロセスとは、OSから見た **実行プログラムの単位** のこと。  
1つのプロセスが存在するとき、1つの実行プログラムがあることになる。  
  
プロセスは、プログラムを指しているので、実態はメモリ上のデータとしてある。  
プログラムはメモリを以下のように使っている。  
  
- **テキストセグメント**  
  - プログラムの命令列  
    実行されるプログラムそのもの ※読み取り専用  
- **データセグメント**  
  - PDA (Processor Data Area)  
    プロセッサの情報やプロセス管理用のデータ領域  
    スタックポインタ(sp)、プログラムカウンタ(pc) などがここに格納される  
    (補足)  
    スタックポインタ (sp): スタック領域のどこを見ているかを指す値  
    プログラムカウンタ (pc): プログラムのどこを実行しているかを指す値  
  - データ領域  
    - ヒープ領域  
    - 静的領域  
  - スタック領域  
    引数やローカルスコープのデータなどを一時的に置くところ  
  
プロセスは必ず一つはスレッドを内包しており、上記のプロセスの構成要素に含まれている。  
スレッドとは  

- スタック領域  
- SP (スタックポインタ)  
- PC (プログラムカウンタ)  

のことを指す。
あるスレッドが新しくスレッドを作成するときは、この三つをデータセグメント内にコピーする。  
スレッドは、  
PC (プログラムカウンタ)を使ってプロセスのテキストセグメントにアクセスしてプログラムを実行する。  
その際スレッド用に準備されたスタック領域とSP (スタックポインタ)を使う。  

### Javaにおけるスレッド  
参考：https://qiita.com/KeithYokoma/items/4e6e9bd4e44aab63424d  
  
Javaではスレッドとタスクという概念がある。  
スレッドはあくまで物理レベルのスレッドであり、実行内容はタスクとして切り出している。  
そのため「スレッドでタスクを実行する」という表現になる。  
  
**・Javaで「タスク」を表現する**  
RunnableやCallableインターフェースで表現される。  
【Runnable】https://docs.oracle.com/javase/jp/8/docs/api/java/lang/Runnable.html  
【Callable】https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Callable.html  
  
これらは関数型インターフェースで、実装クラスのインスタンスが「タスク」となる。これらは別のスレッドで実行される前提で設計されている。  
  
**・Javaで「スレッド」を表現する**  
Threadクラスで表現される。  
https://docs.oracle.com/javase/jp/7/api/java/lang/Thread.html  
  
Threadクラスをnewすれば「スレッド」が作成される。  
「タスク」を受け取って実行したり、Threadクラス自身がRunnableインターフェースを実装しているため「タスク」を実装して実行まですることもできる。  
  
**・Javaで「非同期処理の結果」を表現する**  
Futureクラスで表現される。  
https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Future.html  
  
スレッドは別スレッドで開始されたタスクの完了を待たずに処理を続行し、任意のタイミングでタスクの結果を取得する。  
これをソースコードで表現するためには、別スレッドのタスク開始と同時に未来の結果（つまりFuture）を受け取り、  
任意の場所でFutureに対して操作（キャンセル、タイムアウト設定、完了チェック、結果取得）を行う。  
  
### スレッドプール  
Javaにおいては、あらかじめ「スレッド」を作成してプールしておく仕組み。  
この段階で物理レベルでのスレッド（スタック領域,SP,PC）が作成されるためオーバーヘッドが小さくなる。  
  
スレッドプールとは「複数スレッドをプール・管理し、タスクを受け取ってスレッドで実行する」という表現になる。  
  
**・Javaでスレッドプールを表現する**  
「タスク（Runnable）」を受け取るインターフェース：Executer  
https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/Executor.html  
Executerを「非同期処理の結果（Future）」を扱えるように拡張したインターフェース：ExecutorService  
https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ExecutorService.html  
ExecutorServiceにいくつかのデフォルト実装を提供するabstractクラス：AbstractExecutorService  
https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/AbstractExecutorService.html  
AbstractExecutorServiceを継承したクラス：ForkJoinPool  
https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ForkJoinPool.html  
AbstractExecutorServiceを継承したクラス：ThreadPoolExecutor  
https://docs.oracle.com/javase/jp/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html  
  
・ForkJoinPoolとThreadPoolExecutorの違い  
https://miyakawataku.hatenablog.com/entry/20171228/1514472588  

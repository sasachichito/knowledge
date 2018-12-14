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
  
# 推移的な依存関係の問題  
Aプロジェクトを作成する。Bプロジェクトの成果物(jarB)に依存する。  
  
A→B:1.0→C:1.0  
A→D:1.0→C:2.0  
  
AプロジェクトのクラスパスにはjarB:1.0、jarC:1.0、jarD:1.0、JarC:2.0がダウンロードされる。  
AプロジェクトのソースコードでJarCのクラスを利用できるが、バージョン1.0と2.0のどちらを使えば良いかわからない。  
  
# リポジトリ  
Javaの成果物（jarやwarやpomファイル）が置いてある場所をJavaのリポジトリと呼ぶ。  
Javaのリポジトリには構成の規約の違いからmavenリポジトリとIvyリポジトリの2種類がある。（最近はほとんどmavenリポジトリ。以降はmavenリポジトリ前提で進める）  
リポジトリは成果物を配布するための場所と考えられ、その配布形態から大きく2種類に分かれる。  
  
ローカルリポジトリかリモートリポジトリである。  
ローカルリポジトリはローカルに、リモートリポジトリはサーバー上にJavaリポジトリがある。  
  
リモートリポジトリはさらにプライベートかパブリックかで分かれる。  
プライベートリポジトリはwebDABというHTTPプロトコルの進化系のようなプロトコルを提供するサーバーであればなんでも良い。  
Gradleで、プライベートリポジトリからJavaの成果物を取得するときはリポジトリまでのURLをフルパス指定すれば良い。  
プライベートリポジトリを管理しやすくするToolとしてArtifactoryやNexusがある。  
  
パブリックリポジトリはJavaリポジトリの標準として管理されている。  
Maven Centoral Repository、Jcenterの2つがある。  
Javaリポジトリの標準であるため、多くのビルドツールはデフォルトでこのリポジトリから成果物を取得できるようサポートされている。  
成果物の指定もフルパスではなくgroupIdやartifactIdなどで指定する。  
  
### Sonatype  
Maven Centoral Repositoryを管理している会社。  
  
### JFrog  
Bintrayというソフトウェアの配布を行う為のサービスを提供している会社。  
Bintrayでmavenリポジトリのプライベートリポジトリを作れるし、この会社はJcenterも管理している。  
  
# pomファイル  
javaの成果物の説明書。xmlで書かれる。  
成果物の名前や連絡先、依存関係が書いてある。  
  
依存関係は複雑であるので少し説明する。  
dependenciesタグでこの成果物が依存する別の成果物を記載する。  
依存の仕方の種類がいくつかあり、それを依存成果物単位でscopeタグで記載している。  
pomファイルの例）  
```  
<?xml version="1.0" encoding="UTF-8"?>  
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId></groupId>  
  <artifactId>expro</artifactId>  
  <version>unspecified</version>  
  <dependencies>  
    <dependency>  
      <groupId>org.apache.commons</groupId>  
      <artifactId>commons-configuration2</artifactId>  
      <version>2.4</version>  
      <scope>compile</scope>  
    </dependency>  
    <dependency>  
      <groupId>com.google.guava</groupId>  
      <artifactId>guava</artifactId>  
      <version>23.0</version>  
      <scope>runtime</scope>  
    </dependency>  
  </dependencies>  
</project>  
```  
依存関係の例として以下で考える。  
A→B:1.0→C:1.0  
A→D:1.0→C:2.0  
Bの成果物を作るときを考える。pomの例だと  
Bはcommons-configuration2（ABCでいうとC）に依存する。scopeがcompileである。  
つまりBの成果物の説明書には、BはCにcompileスコープで依存するよ、と言っている。  
  
Bを利用するAを考える。  
AでBを解決すると、jarB1.0とjarC1.0がAのマシンにダウンロードされクラスパスに追加される。  
Aはそのどちらのクラスも使える。  
  
scopeがruntimeだった場合。  
AでBを解決すると、jarB1.0とjarC1.0がAのマシンにダウンロードされる。ここまでは一緒。  
しかし、AはjarB1.0のクラスは使えるけど、jarC1.0のクラスが使えない。ここが違う。  
  
何が良いかというと。推移的な依存関係の問題をなくせること。  
Aは依存先（B）の依存先（C:1.0）を使えないので、同様に  
依存先（D）の依存先（C:2.0）も使えない。  
AがCを使いたければCを自分で解決しなければならない。  
  
# Gradleにおけるコンフィグレーション（compile,implementation）の使い分け  
Bを作っているとする。BはCに依存する。  
GradleのビルドスクリプトでCを解決するときに、コンフィグレーションを使う。  
コンフィグレーションは依存関係の種類を表し、pomのscopeに対応する。  
  
ビルドスクリプトで以下のように書けば、成果物BのpomにはCのscopeはcompileになる。  
```  
dependencies {  
    compile 'C'  
}  
```  
  
以下のように書けば、成果物BのpomにはCのscopeはruntimeになる。  
```  
dependencies {  
    implementation 'C'  
}  
```  
  
可能な限りimplementationを使おう。  
  
参考 https://qiita.com/opengl-8080/items/6ad642e0b016465891de  

# SAP社が提供するERPパッケージ
### 大企業向けERPパッケージのバージョン遍歴  
RF→R/2→R/3→ECC→SoH→S/4 HANA  
  
ECCのサポートが2025年に終了。  
  
### 中小企業向けERPパッケージ  
SAP Business One(B1)... ECCから機能を削ったを提供。  
SAP Business ByDesign... SaaSとして提供。  
  
### SAP NetWeaver  
ミドルウェア。ERPが標準で提供するABAPプログラムや、ユーザが開発したJava・ABAPプログラムの実行環境。  
JavaにおけるWebコンテナ・JREの役割だけでなく、  
DBやリモートサービス接続等を担う多くのコンポーネントで構成されている。  
  
# SAPが提供するERP以外のサービス  
### SAP HANA  
データベース。「S/4 HANA」で採用されている。  
もともとS/4 HANAだけで動くものだったが、クラウド上(AWS,Azure,GCP等)で利用できるDB製品になった。  
  
### CRM
顧客管理ソフト  
  
### BI  
BIソフト  
  
### SaaS  
SuccessFactors... 人材管理  
Concur... 経費精算管理  
等  
  
### PaaS  
SAP Cloud Platform... アプリケーション構築・稼働プラットフォーム  
  
# SAP GUI  
SAPの一般的なユーザインターフェース。3種類ある。  
  
・SAP GUI for Windows  
クライアントアプリ形式。Windowsで動く。  
  
・SAP GUI for Java  
クライアントアプリ形式。Java環境で動く。従ってマルチプラットフォーム。  
  
・SAP GUI for HTML  
ブラウザ(Web GUI)形式。従ってマルチプラットフォーム。  
※ソフト配布の必要性はないが機能に制約あり。  
  
# ABAP  
[参考](https://it-biz.online/sap/abap-abc/)  
  
## 構文  
・TYPEで型を定義  
ABAPディクショナリからデータエレメントとして定義することも可能  
  
・DATAで型の変数を定義  
DATA変数は3種類。  
単一の型の「変数」  
複数の型の「構造」  
複数の構造の「内部テーブル」  
  
・CONSTANTSで型の定数を定義  
  
・FORMでサブルーチン(メソッド)定義  
USING→引数  
CHANGING→戻り値  
  
・PERFORMでサブルーチン(メソッド)呼び出し  
  
・DEFINEでマクロを定義可能  
※ほぼ使われていない  
  
・INCLUDEでプログラムをインクルード  
  
## プログラムタイプ  
ABAPプログラムの作成時に選択する。  
プログラムの種別を表しており、作成するプログラムのソースコードに、対応する宣言命令を挿入する。  
  
・宣言命令  
[参考](https://help.sap.com/doc/saphelp_470/4.7/ja-JP/21/682f8b277e11d2954e0000e8353423/content.htm?no_cache=true)  
  
プログラムタイプ(宣言命令の種別)によって  
プログラムにどの処理ブロックを含めることができるのか、  
ABAP実行時環境によりそのプログラムはどのように処理および実行されるのか、  
独自のDynpro制御ロジックと連動できるのかどうか  
が定義される。  
[参考](https://help.sap.com/doc/saphelp_nw70/7.0.12/ja-JP/fc/eb2d5a358411d1829f0000e829fbfe/content.htm?no_cache=true)  
  
・処理ブロックとイベントキーワード  
処理ブロックはABAPプログラムのソースコードの一部を指す。  
ABAP実行環境はユーザーの操作に対応するイベントを発行し、対応する処理ブロックの実行を行う。  
対応する処理ブロックはイベントキーワードによって決定する。  
[参考](https://help.sap.com/doc/saphelp_46c/4.6C/ja-JP/9f/db9a0735c111d1829f0000e829fbfe/content.htm?no_cache=true)  
  
・プログラムタイプ一覧  
TYPE 1: 実行可能プログラム  
TYPE M: モジュールプール  
TYPE F: 汎用グループ  
TYPE K: クラスプール  
TYPE J: インタフェースプール  
TYPE S: サブルーチンプール  
TYPE T: タイププール  
TYPE I: インクルードプログラム  
[参考](https://help.sap.com/doc/saphelp_470/4.7/ja-JP/e4/2adbd7449911d1949c0000e8353423/content.htm?no_cache=true)  
TYPE1、Mのみ単体で実行可能。  
[ABAPプログラムからTYPE1、Mの呼び出し方](https://help.sap.com/doc/saphelp_470/4.7/ja-JP/9f/db9d7535c111d1829f0000e829fbfe/content.htm?no_cache=true)  
  
・TYPE 1: 実行可能プログラム  
レポートプログラムと呼ばれる。  
プログラム名の入力、トランザクションコードの使用で実行可能。  
ABAP実行環境は固定された順序でイベントを発行し、一連の処理ブロックを実行。  
  
・TYPE M: モジュールプール  
ダイアログプログラムあるいはDynproと呼ばれる。  
トランザクションコードを使用してのみ開始可能。画面が起動する。  
画面自体とその(Dynpro)制御ロジックで構成され、制御ロジックは複数のPBO/PAIダイアログモジュールで構成される。  
ABAP実行環境は(Dynpro)制御ロジックに従ってイベント発行、対応する処理ブロックを実行する。  
[参考](https://help.sap.com/doc/saphelp_470/4.7/ja-JP/9f/db9cdc35c111d1829f0000e829fbfe/content.htm?no_cache=true)  
  
・TYPE F: 汎用グループ  
汎用モジュールのコンテナプログラム。  
  
・TYPE K: クラスプール  
グローバルクラスのコンテナプログラム。  
  
・TYPE J: インタフェースプール  
グローバルインターフェースのコンテナプログラム。  
  
・TYPE S: サブルーチンプール  
サブルーチンのコンテナプログラム。  
  
・TYPE T: タイププール  
タイプ(型)のコンテナプログラム。  
  
・TYPE I: インクルードプログラム  
他のプログラムタイプのプログラムからインクルードされて利用できるプログラム。  
  
## クラス  
ローカルクラスとグローバルクラスがある。  
  
・ローカルクラス  
プログラムの中で定義されたクラス  
そのプログラム内でのみ利用可能。  
  
・グローバルクラス  
クラスビルダ(Tr-cd:SE24)で定義されたクラス。  
(プログラムタイプがクラスプール)  
全てのプログラムから利用可能。  
  
## 汎用モジュール  
汎用グループ→staticクラス  
汎用モジュール→staticメソッド  
  
IMPORTING→引数  
EXPORTING→戻り値  
  
Tr-cd:SE37 汎用モジュールエディタで定義(F8押下でテストも可能)  
(プログラムタイプが汎用グループ)  
  
CALL FUNCTIONで汎用モジュール呼び出し  
  
## リモートAPI  
ABAPプログラムをリモートプログラムから呼び出すインターフェース。  
  
### RFC  
リモートファンクションコールの略。  
リモートシステムの汎用モジュールを呼び出すプロトコル。3種類ある。  
・sRFC  
・tRFC  
・qRFC  
[参考](https://help.sap.com/doc/saphelp_nw70/7.0.12/ja-JP/6f/1bd5b6a85b11d6b28500508b5d5211/content.htm?no_cache=true)  
tRFCはトランザクション処理のための特性(同時実行制御とACID)に対応している(?)  
  
RFC汎用モジュール(RFCで呼び出される汎用モジュール)を作成するには  
通常の汎用モジュール作成手順の中で明示的に指定する。  
  
プログラムからRFC汎用モジュールを呼び出すには  
CALL FUNCTIONにDESTINATION句をつけて、システムに登録済みのリモートホストを指定する。  
[参考](https://help.sap.com/doc/saphelp_470/4.7/ja-JP/22/04250b488911d189490000e829fbbd/content.htm?no_cache=true)  
  
RFC汎用モジュールは同一SAPのABAPプログラムから利用することも可能。  
  
・BAPI  
SAPに標準で用意されているRFC対応汎用モジュール。  
SAPの標準機能(実態はABAPプログラム)のAPIとして動作する。  
  
### バッチインプット  
大容量のデータセットを SAP システムに転送する際に使用するプロトコル。  
  
### GUI  
SAP GUIとSAP間のプロトコルを外部プログラムで実装する方式。  
  
## ABAPディクショナリ（データディクショナリ/ディクショナリ/DDIC）  
[参考](https://help.sap.com/saphelp_tm92/helpdata/en/cf/21ea0b446011d189700000e8322d00/frameset.htm)  
ABAPディクショナリ、データディクショナリ、ディクショナリ、DDICは同義。DDICはdata dictionaryの略。  
大きく3種類の機能を持つ。  
・データベースの管理  
・型の管理  
・その他の管理  
  
### データベースの管理  
ABAPディクショナリはSAPシステムの中央データベースのテーブルを管理する。  
ディクショナリにはテーブルのメタ記述が含まれ、これらを使用して中央データベースを管理する。  
  
ビューやテーブルを持ち、カラムには型(エレメント)を割り当てる必要がある。  
カラムはテーブルのフィールドとも呼ばれる。  
  
各テーブルは各オブジェクトモデル(DDDにおける集約)に対応し、実際にデータが格納される。  
例）顧客テーブル  
テーブルを型としてABAPプログラムから利用できる。  
  
またロック制御のためにロックオブジェクトを定義することもある。  
  
### 型の管理  
エレメント、構造、テーブルの3種類の型を定義できる。  
※ここでのテーブルはデータベース管理のテーブルとは意味が異なるため注意。  
エレメントはデータベース管理におけるテーブルのフィールドとして利用される。  
  
エレメントにはドメインとよばれる不変条件を紐づけることができる。  
これらの型はABAPプログラムから利用できる。  
  
エレメントは基本型か参照型にすることができる。  
・基本型  
ABAPディクショナリで事前定義しているデータ型か、  
それにドメインを紐づけたエレメント。  
  
・参照型  
グローバルなクラス・インターフェースの参照を持てるエレメント。  
  
# SAP Fiori  
SAP GUIに代わるUI。  
[参考](https://www.sapjp.com/blog/archives/12657)  
  
# OData  
HTTPの設計原則(RESTに近い)  
[参考](https://qiita.com/tami/items/411a226d1ea6bb25b5f1)  
  
SAP Netweaver GatewayサーバでODataリクエストを受け付けて、SAPにRFCでバックポスト可能。  
[参考](https://qiita.com/tami/items/a87a6f67be928c78e4e8)  
  
# 調査中  
OLE  
EDI  
IDoc  

# クラスベースの例外
## CX_STATIC_CHECK 
（Javaにおける検査例外）  
この種類の例外を送出するメソッドやモジュールはIFで明示的に宣言する必要あり。   
呼び出し元はキャッチ・処理しなければコンパイルエラーとなる。  

## CX_DYNAMIC_CHECK 
（Javaにおける非検査例外 ）  
この種類の例外を送出するメソッドやモジュールはIFで明示的に宣言する必要はない。  
呼び出し元でキャッチ・処理しない場合ランタイムエラーとなる。  

## CX_NO_CHECK 
（Javaにおける非検査例外 ）
この種類の例外を送出するメソッドやモジュールはIFで明示的に宣言する必要はない。  
呼び出し元でキャッチ・処理しない場合上位の呼び出し元に自動で転送される。  
※最上位の呼び出し元まで転送された結果キャッチ・処理されなければランタイムエラーとなる。  

# Windows＆DockerでSAP NetWeaver AS ABAP 7.52 SP04のDeveloper Editionを構築する  
  
### SAP社の人が提供しているDockerfileを用意  
```  
$ git clone https://github.com/nzamani/sap-nw-abap-trial-docker.git  
$ cd sap-nw-abap-trial-docker/  
$ mkdir sapdownloads  
```  
  
### Developer Editionのファイルを取得して配置する  
① https://developers.sap.com/trials-downloads.html からSAP ABAP AS Part1～11をダウンロードする。  
② .rarをsapdownloadsに展開  
※rar圧縮のため、どれか一つ(Part1等)を解凍すれば他のrarファイルも自動で解凍・結合される  
  
### NW ABAP 7.52コンテナ作成  
```  
# イメージ作成  
$ docker build -t nwabap:7.52 .  

※※※ 1 次のような権限系のエラーが出た場合
Sending build context to Docker daemon  14.83GB
Error response from daemon: Error processing tar file(exit status 1): write /sapdownloads/server/TAR/x86_64/sapmnt.tgz-aa: read-only file system

以下のように権限を付与する（Git Bashではchmodが動作しないため以下のコマンドをPower Shellから実行する）
icacls 'C:\Users\Path\To\sap-nw-abap-trial-docker\sapdownloads\*' /grant Everyone:F

※※※ 2 Docker Hostのディスク容量が足りない場合(build中のCOPYタスクで15GBは消費する)
2.1. Dockerfileの以下の記述をコメントアウト
COPY install.exp /tmp/sapdownloads/
COPY sapdownloads /tmp/sapdownloads/
WORKDIR /tmp/sapdownloads
RUN chmod +x install.sh install.exp

2.2. Dockerfileのあるディレクトリに.dockerignoreファイルを作成し、以下を記述(dockerデーモンに送信するコンテキストファイルのうち除外するものを記載)
sapdownloads

※※※ 3 次のようなzypperのエラーが出た場合
Retrieving repository 'Non-OSS Repository' metadata [.Repository 'Non-OSS Repository' is invalid.
[repo-non-oss|http://download.opensuse.org/distribution/leap/15.2/repo/non-oss/] Valid metadata not found at specified URL

Dockerfileの以下の記述をコメントアウト
RUN zypper --non-interactive install --replacefiles which hostname expect net-tools iputils wget vim iproute2 unrar less tar gzip uuidd tcsh libaio
RUN mkdir /run/uuidd && chown uuidd /var/run/uuidd && /usr/sbin/uuidd
```

```
# コンテナ作成＆Bash起動  
$ docker run -p 8000:8000 -p 44300:44300 -p 3300:3300 -p 3200:3200 -h vhcalnplci --name nwabap752 -it nwabap:7.52  

※※※ ディスク容量対策でDockerfileにコメントアウトを入れた場合は以下
docker run --mount type=bind,src=/Path/To/sap-nw-abap-trial-docker/sapdownloads,dst=/tmp/sapdownloads -p 8000:8000 -p 44300:44300 -p 3300:3300 -p 3200:3200 -h vhcalnplci --name nwabap752 -it nwabap:7.52
```

```
# NW ABAP 7.52インストール  
vhcalnplci:/# /usr/sbin/uuidd
vhcalnplci:/# /tmp/sapdownloads/install.sh
vhcalnplci:/# exit  

※※※ zypperのエラー対策でDockerfileにコメントアウトを入れた場合は先に以下を実行
vhcalnplci:/# zypper --non-interactive install --replacefiles which hostname expect net-tools iputils wget vim iproute2 unrar less tar gzip uuidd tcsh libaio
vhcalnplci:/# mkdir /run/uuidd && chown uuidd /var/run/uuidd && /usr/sbin/uuidd

※※※ 次のようなエラーが出た場合
The option vm.max_map_count was added to the configuration file
sysctl: setting key "vm.max_map_count": Read-only file system
Error: Could not reload sysctl configuration

Dockerホストの設定値を変更する必要がある模様: 参考: http://gogs.nicuessa.com:880/mirror-ideas/sap-nw-abap-trial-docker/src/master/README.md
Linux:
sysctl -w vm.max_map_count=1000000

macOS wit Docker for Mac (FYI see also here):
screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
sysctl -w vm.max_map_count=1000000

Windows and macOS with Docker Toolbox
docker-machine ssh
sudo sysctl -w vm.max_map_count=1000000
```  
  
### NW ABAP 7.52起動  
```  
$ docker start -i nwabap752  
vhcalnplci:/# /usr/sbin/uuidd  
vhcalnplci:/# su npladm  
vhcalnplci:/# startsap ALL  
```  
  
### NW ABAP 7.52停止  
```  
vhcalnplci:/# su npladm  
vhcalnplci:/# stopsap ALL  
vhcalnplci:/# exit  
```  

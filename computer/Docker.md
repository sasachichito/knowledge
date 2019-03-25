## Dockerイメージとコンテナ

### AUFS(AnotherUnionFileSystem)... 
別のファイルシステムを差分として適用できるファイルシステムのこと。  

### Dockerイメージ... 
読み取り専用のファイルシステム（AUFS）レイヤの集合とメタ情報（どのコマンドを実行するか、どのポートを開くか等）。  

### Dockerコンテナの作成... 
Dockerイメージレイヤ群の最上位に書き込み可能なレイヤを追加する。（このタイミングでメタ情報を上書きすることも可能）。  

### Dockerコンテナの起動... 
メタ情報か、`docker run`のパラメータで指定されたプロセスを、隔離されたシステムリソース上で実行する。  

### Dockerコンテナのイメージ化... 
コンテナの最上位レイヤをコミットし読み取り専用にする。


![イメージとコンテナ](/picture/Docker.png "イメージとコンテナ")
  
## DockerfileとDockerCompose.yml
- Dockerfile  
イメージを作るためのファイル。ベースとなるイメージに独自のレイヤを追加する。  
- DockerCompose.yml  
コンテナを起動するためのファイル。複数のコンテナのメタ情報をまとめて記載する。  

## Dockerネットワーク

Linuxは、認識しているNICをポートにすることでブリッジにすることができる。 （このLinuxマシンをブリッジホストという） 
いわゆるネットワーク機器のブリッジにはMAC・IPアドレスは必要ないため、ブリッジホストのNICのMACアドレスと、割り当てていたIPアドレスは無視されてしまう。  
つまり、もしもブリッジホストのNICが全てポートになった場合、ポート接続先マシンからブリッジホストにアクセスする手段がなくなってしまう。  
  
そこでブリッジホストに対してMAC・IPアドレスを割り当てることができ、ポート接続先マシンからはそれを利用してアクセスする。  
  
コンテナからパケットが外に出るときはグローバルIPとして振舞う必要があるのでIPマスカレードする。  
パケットがグローバルIP宛に来たときは、単純にブリッジホストが受け取るので、ポートフォワードでコンテナのデーモンにアクセスする。  


![ネットワーク](/picture/Docker2.png "ネットワーク")


## docker run の i, tオプション

![i,tオプション](/picture/Docker3.png "i,tオプション")


## Docker swarm

![swarm](/picture/Docker4.png "swarm")


# メモ
```
############### Dockerでjmeterを実行する ###############
# Dockerfile
FROM openjdk:8-jdk-alpine

RUN mkdir /jmeter \
    && cd /jmeter \
    && wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.1.1.tgz \
    && tar --strip-components=1 -xvf apache-jmeter-5.1.1.tgz \
    && rm apache-jmeter-5.1.1.tgz

EXPOSE 1099

CMD /jmeter/bin/jmeter-server -Jserver.rmi.ssl.disable=true

# イメージ作成
# Dockerファイルがあるディレクトリで以下を実行
$ docker build -t myjmeter .

# コンテナ起動
$ docker run -d -p 1099:1099 myjmeter

# jmxをコンテナ内にコピー
# 予め作成しておいたjmxファイルをコピーする
# Jmeterコンテナ→ホスト上のアプリにリクエストを投げるため
# jmx作成時のリクエストの向き先ホスト名は「host.docker.internal」で指定している
$ docker cp ~/MyTest.jmx 9c8efe4089b0:/tmp

# Jmeter実行
$ docker exec 9c8efe4089b0 /jmeter/bin/jmeter -n -t /tmp/MyTest.jmx -l log.jtl -e -o report

# レポートを取得
$ docker cp 9c8efe4089b0:/report .

# レポートを閲覧
$ cd report
$ opne index.html
```
```
############### Dockerでjmeterをmaster-slave構成で実行する ###############
# Dockerfile
$ cat Dockerfile 
FROM openjdk:8-jdk-alpine

RUN mkdir /jmeter \
    && cd /jmeter \
    && wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.1.1.tgz \
    && tar --strip-components=1 -xvf apache-jmeter-5.1.1.tgz \
    && rm apache-jmeter-5.1.1.tgz

EXPOSE 1099

CMD /jmeter/bin/jmeter-server
$

# イメージ作成
$ docker build -t jmeter .

# master起動
$ docker run --name master -d -p 10990:1099 jmeter

# slave起動
$ docker run --name slave1 -d -p 10991:1099 jmeter
$ docker run --name slave2 -d -p 10992:1099 jmeter
$ docker run --name slave3 -d -p 10993:1099 jmeter
$ docker run --name slave4 -d -p 10994:1099 jmeter
$ docker run --name slave5 -d -p 10995:1099 jmeter

# MyTest.jmxをmasterに配置
$ docker cp ~/MyTest.jmx master:/tmp

# jmeter実行
$ docker exec master /jmeter/bin/jmeter \
-Jserver.rmi.ssl.disable=true \
-n -t /tmp/MyTest.jmx \
-l log_`date +%Y%m%d%H%M%S`.jtl \
-e -o report_`date +%Y%m%d%H%M%S` \
- R  host.docker.internal:10991,host.docker.internal:10992,host.docker.internal:10993,host.docker.internal:10994,host.docker.internal:10995
```
```
############### DockerHubにアップロード ###############
$ docker login
$ docker build -t sachito/jmeter .
$ docker push sachito/jmeter:latest
```

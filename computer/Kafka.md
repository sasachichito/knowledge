# Kafka  
Apache Kafkaは分散メッセージングシステムである。  
Confulent PlatformとはKafkaを（コアとして）含むデータストリーム基盤システムである。  
参考： https://qiita.com/kimutansk/items/a1cfbca2b92f0e5d7b9b  
  
細かな仕組み： https://qiita.com/sigmalist/items/3b512e2ab49b07271665  
  
## Confulent Platform  
GitHub: https://github.com/confluentinc  
  
### Zookeeper  
分散システムのコーディネートサービスである。  
分散システムの各インスタンスの設定・情報を管理し、総合的な判断を下すための機能が提供されている。  
主な機能：  
```  
・設定の共有  
・分散ロック  
・リーダー選出  
・アクティブなインスタンス取得  
```  
  
分散メッセージングシステムであるKafkaも、Zookeeperを使用する格好となる。  
Kafkaの使用目的：  
```  
・トピック情報の管理  
・パーティション・レプリカ等の数・配置の決定  
・コントローラー(プロデューサーのリクエストを受け付けるブローカーインスタンス)選出  
・アクティブなブローカー取得  
等  
```  
  
### Schema Registory  
参考：https://docs.confluent.io/current/schema-registry/index.html  
スキーマを管理するサービスである。いくつかのスキーマ言語に対応している。  
  
スキーマ言語とは、データの「型」を定義する言語仕様である。例としてJson SchemaやProtobuf(Protocol Buffersの略)が挙げられる。  
Json Schemaと名前は似ているが、Jsonは型ではなくそのインスタンスの記述形式(シリアライゼーション形式)である。  
Protobufで定義した型のインスタンスを、Json形式にシリアライズしてサーバ間で通信できるし、  
Protobuf周辺のライブラリを使えばインスタンスをJsonにせず直接バイナリにシリアライズして通信も可能である。  
参考： https://qiita.com/g0e/items/9a4f886897fd46f107a8  
https://qiita.com/arumi8go/items/a9530cbd39ff545a7bbb  
  
Jsonにシリアライズするとインスタンスを人間が読めるメリットはある。  
しかしJson文字列をバイナリに変換するため、インスタンスから直接効率的にバイナリに変換する方法と比較して  
データサイズやデシリアライズのコストが大きくなる。  
  
(サーバA)インスタンス→Json文字列→バイナリ ～network～ バイナリ→Json文字列→インスタンス(サーバB)  
  
(サーバA)インスタンス→バイナリ ～network～ バイナリ→インスタンス(サーバB)  
  
対応するスキーマ  
```  
Avro:   
スキーマ言語でありシリアライズ/デシリアライズ用ライブラリを提供している。  
インスタンスをシリアライズした結果のバイナリにスキーマ情報を含めないためデータサイズは小さい。  
デシリアライズの際はスキーマ定義を取得する必要がある。  
Schema Registoryによりプロデューサーとコンシューマーでスキーマ定義を共有できるため、  
型の変更はスキーマ定義の修正のみ対応可能で、シリアライズ/デシリアライズを行うプログラムの変更は不要となる。  
  
Protocol Buffers:  
スキーマ言語でありシリアライズ/デシリアライズ用ライブラリを提供している。  
インスタンスをシリアライズした結果のバイナリにスキーマ情報を含める。  
  
Json Schema:  
  
```  
参考(avro vs protobuf)： https://www.slideshare.net/seiyamizuno35/serialization-systems  
  
### Broker  

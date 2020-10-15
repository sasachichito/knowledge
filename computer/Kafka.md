# Kafka  
Apache Kafkaは分散メッセージングシステムである。  
Confulent PlatformとはKafkaを（コアとして）含むデータストリーム基盤システムである。  
参考： https://qiita.com/kimutansk/items/a1cfbca2b92f0e5d7b9b  
  
細かな仕組み： https://qiita.com/sigmalist/items/3b512e2ab49b07271665  
  
### オフセット(位置情報)  
コンシューマー(以降cons)が管理する情報であり、パーティション毎に以下のオフセットを持つ。  
```  
Current offset (position) ... 次に読み出すレコードのオフセット  
Committed offsets... 処理完了したレコードのオフセット  
```  
Commited offsetsの更新タイミングはconsの実装による。  
これらのoffsetsはcons独自のDBに持つこともあれば、brokerのトピックに入れることも可能。  
consに障害が発生した場合、復帰後Current offsetをCommited offsetsまで戻して処理を再開する。  
  
パーティションのoffsetsはconsグループ内で共有される(はず)  
  
# Confulent Platform  
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
  
### Rest Proxy API  
API仕様: https://docs.confluent.io/5.5.1/kafka-rest/api.html  
```  
※環境：Windows & Docker(Confulent Platform v5.5.1) & GitBash  
  
# トピック作成＆メッセージ送信  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \  
-H "Accept: application/vnd.kafka.v2+json" \  
--data '{"records":[{"value":{"foo":"bar"}}]}' "http://localhost:8082/topics/jsontest"  
  
(response sample)  
{  
  "offsets": [  
    {  
      "partition": 0,  
      "offset": 0,  
      "error_code": null,  
      "error": null  
    }  
  ],  
  "key_schema_id": null,  
  "value_schema_id": null  
}  
  
# トピックのパーティション数の変更 参考: https://docs.confluent.io/5.5.1/kafka/post-deployment.html#modifying-topics  
[localhost]$ exec winpty bash  
[localhost]$ docker exec -it broker bash  
root@broker:/# kafka-topics --bootstrap-server localhost:9092 --alter --topic jsontest --partitions 3  
root@broker:/# exit  
  
# 追加でメッセージ送信  
※keyなし(メッセージはラウンドロビンで各パーティションに格納される)  
[localhost]$ for i in `seq 10`  
do  
  curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \  
  -H "Accept: application/vnd.kafka.v2+json" \  
  --data '{"records":[{"value":{"xxx":"'"$i"'"}}]}' "http://localhost:8082/topics/jsontest"  
done  
  
※keyあり(同じkeyのメッセージは同一パーティションに格納される)  
[localhost]$ for i in `seq 10`  
do  
  curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \  
  -H "Accept: application/vnd.kafka.v2+json" \  
  --data '{"records":[{"key":1234,"value":{"xxx":"'"$i"'"}}]}' "http://localhost:8082/topics/jsontest"  
done  
  
# コンシューマーグループ & コンシューマー作成(※ステートフルAPI)  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"format": "json", "auto.offset.reset": "earliest"}' http://localhost:8082/consumers/my_json_consumer  
  
(response sample)  
{  
  "instance_id": "rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae",  
  "base_uri": "http://rest-proxy:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae"  
}  
  
# サブスクリプション登録  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" --data '{"topics":["jsontest"]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/subscription  
  
# サブスクリプション確認  
[localhost]$ curl -w "\n" -sS -X GET -H "Content-Type: application/vnd.kafka.v2+json" \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/subscription  
  
(response sample)  
{"topics":["jsontest"]}  
  
# サブスクライブ  
[localhost]$ curl -w "\n" -sS -X GET -H "Accept: application/vnd.kafka.json.v2+json" \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/records  
  
(response sample)  
[  
  {  
    "topic": "jsontest",  
    "key": 1234,  
    "value": {  
      "xxx": "yyy"  
    },  
    "partition": 0,  
    "offset": 2  
  },  
  {  
    "topic": "jsontest",  
    "key": 1234,  
    "value": {  
      "zzz": "aaa"  
    },  
    "partition": 0,  
    "offset": 3  
  },  
...  
]  
  
# コンシューマー削除  
[localhost]$ curl -w "\n" -sS -X DELETE -H "Content-Type: application/vnd.kafka.v2+json" \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae  
  
```  
  
コンシューマーのパーティションアサインメント方法は2種類ある。  
```  
# 自動アサイン(コンシューマーの購読するトピックを指定(サブスクリプション登録)し、パーティションは自動でアサインさせる)  
※先述のサブスクリプション登録方法を参照  
  
# 手動アサイン(コンシューマーの購読するトピックとパーティションを指定する)  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"partitions":[{"topic":"test","partition":0},{"topic":"test","partition":1}]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/assignments  
  
# アサインメント確認  
[localhost]$ curl -w "\n" -sS -X GET -H "Content-Type: application/vnd.kafka.v2+json" \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/assignments  
  
(response sample)  
{"partitions":[{"topic":"jsontest","partition":0},{"topic":"jsontest","partition":1},{"topic":"jsontest","partition":2}]}  
  
```  
  
コンシューマーのCommited Offsetsを変更可能。  
参考: https://docs.confluent.io/5.5.1/kafka-rest/api.html#post--consumers-(string-group_name)-instances-(string-instance)-offsets  
```  
# 指定  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"offsets":[{"topic":"test","partition":0,"offset":20},{"topic":"test","partition":1,"offset":30}]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/offsets  
  
# 確認  
[localhost]$ curl -w "\n" -sS -X GET -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"partitions":[{"topic":"test","partition":0},{"topic":"test","partition":1}]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/offsets  
```  
  
コンシューマーのCurrent Offsetsを変更可能。  
参考: https://docs.confluent.io/5.5.1/kafka-rest/api.html#post--consumers-(string-group_name)-instances-(string-instance)-positions  
```  
# 指定  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"offsets":[{"topic":"test","partition":0,"offset":20},{"topic":"test","partition":1,"offset":30}]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/positions  
  
# 最初  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"partitions":[{"topic":"test","partition":0},{"topic":"test","partition":1}]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae//positions/beginning  
  
# 最後  
[localhost]$ curl -w "\n" -sS -X POST -H "Content-Type: application/vnd.kafka.v2+json" \  
--data '{"partitions":[{"topic":"test","partition":0},{"topic":"test","partition":1}]}' \  
http://localhost:8082/consumers/my_json_consumer/instances/rest-consumer-d7150740-f18c-4304-af0c-25d3688978ae/positions/end  
  
```

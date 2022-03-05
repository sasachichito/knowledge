```
キーワード：
protobuf
gprc

foo.proto
　protoc
　　protoc-gen-go


protobufはデータ形式（後方互換性、通信サイズ小）
protobufで通信するのがgrpc
.protoはスキーマ定義書、これをprotocに渡すとクライアント・サーバーの実装のスケルトンが生成される。
goサーバーを作りたければprotocのプラグイン(protoc-gen-go)を入れる。

RPC: remote procedure call


後方互換性のある修正は以下のようなもの。このような修正はクライアントの実装に影響を及ぼさない。
{
	foo string
}
↓
{
	foo string
	fee int
}

既存項目がなくなるような（後方互換性のない）修正は、リリース戦略でカバーしなければならない。
```

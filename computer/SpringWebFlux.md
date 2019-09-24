# Spring WebFlux  
参考 : https://www.infoq.com/jp/articles/Servlet-and-Reactive-Stacks-Spring-Framework-5/  
https://www.slideshare.net/TakuyaIwatsuka/spring-5  
https://www.slideshare.net/ArisaSasaki/jsug-20185-spring-io-108053900  
Spring MVCと並ぶフレームワーク。  
Spring MVC : Servlet API(Servlet仕様の実装)を利用する。  
Spring WebFlux : Spring Web API(Servlet 3.1・Reactive Streams仕様の実装)を利用する。  
  
WebFluxでは[Reactive Programming](https://github.com/sasachichito/knowledge/blob/master/computer/Reactive.md)をサポートする。  
WebFluxでシステム全体をRPで実装することもできるし、Spring MVCにリアクティブライブラリとして導入して、  
DBやリモートサービス接続を非同期ノンブロッキングに行うこともできる。  
  
## 特徴  
大前提として、ブロッキングIO＋マルチスレッドよりも  
ノンブロッキングIO＋シングルスレッドの方が効率が良い。  
  
Reactive Programmingは後者の実装をわかりやすく書くことが目的であり、  
Spring WebFluxはそのフレームワークである。  
  
Spring WebFluxを採用した場合、  
`クライアント↔︎HTTP↔︎OS↔︎WEBコンテナ(Servlet3.1)↔︎Spring WebFlux↔︎アプリ↔︎外部API`  
この流れの中のIOが全てノンブロッキングとなるため後者を完璧に実現できる。  
  
Spring MVCを採用した場合は、  
`クライアント↔︎HTTP↔︎OS↔︎WEBコンテナ(Servlet3.0)↔︎Spring MVC↔︎アプリ↔︎外部API`  
となり`HTTP↔︎OS↔︎WEBコンテナ`間がブロッキングになるため不可能。  
しかしながら`アプリ↔︎外部API`で部分的にリアクティブなライブラリを利用することはできる。  
  
## 外部APIについて  
参考 : https://qiita.com/kilvis/items/fb18be963da6cac03ee9  
Reacive Streamsは決して遅い外部APIから救われるようなものではない。がマルチスレッドからは救われる。  
マルチスレッドは制御が大変で効率が悪い。  
  
リアクティブなライブラリを利用すれば`アプリ↔︎外部API`で  
シングルスレッドで結果をReactive StreamsのProducer(Mono,Flux)で取得できる。（この段階では通信していない）  
そしてランタイムまでProducerを返す(@ContorollerでProducerをreturnする)とランタイムがSubscribeしてデータが流れ出す。（外部APIとの通信が始まる）  
  
もし外部APIが複数ありビジネスロジックとして結果をまとめ上げる必要がある場合でもシングルスレッドなのでメインスレッドはブロックできないし  
マルチスレッドにもすべきでない。（マルチスレッドにしてしまったら本末転倒）  
  
その場合は外部API接続〜ビジネスロジックをストリーム処理で記述したProducerを定義して、それをランタイムに戻す。  
これによりノンブロッキングで複数の外部APIに問い合わせてビジネスロジックを実行してくれる。（これがReactive Streamsの目指す世界）  
  
## Servlet API  
参考 : https://qiita.com/kazuki43zoo/items/8be79f98621f90865b78  
https://qiita.com/kimullaa/items/c083757078406da17825  
  
### Servlet 3.0 Async Servlet  
処理中にworkerスレッドを解放するための仕組み。  
`AsyncContext asyncContext = httpServletRequest.startAsync(httpServletRequest, httpServletResponse);`  
でAsyncContextを取得し、workerスレッド（Tomcatがリクエストを受け付けるためのスレッドプール）とは別の、独自のスレッドで処理を行う。  
  
これにより長時間workerスレッドを占有することがなくなるため、全体のスレッド数を減らすことができる。  
  
レスポンスは一括で返したり、chunckedで返せる。  
  
Servletのリクエスト読み込み・レスポンス書き込みI/Oはブロッキングである。  
`HttpServletRequest#getInputStream()`と`HttpServletResponse#OutputStream()`を操作している。  
  
### Servlet 3.1 Servlet non-blocking  
Servletのリクエスト読み込み・レスポンス書き込みI/Oをノンブロッキングにするための仕組み。  
  
## Mono Flux  
参考 : https://qiita.com/sugibuchi/items/e933ae4b5ec512054de2  
Spring5はReactor Coreに依存している。  
Reactor CoreはReactive Streams仕様（Publisher, Subscription, Subscriber等）の実装でもある。  
MonoとFluxはReactor Coreで提供されるPublisherの実装。  
  
## WebClient  
参考 : https://qiita.com/toastkidjp/items/e888782a0f7369b3dfc8  
Spring Frameworkのspring-webに追加された`org.springframework.web.client.reactive.WebClient`のこと。  
HTTP 通信の結果をMonoとして受け取れる。  

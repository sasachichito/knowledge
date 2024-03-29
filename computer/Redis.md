## NoSQLとは  
【参考】https://japan.zdnet.com/article/35061140/  
  
## redisでタイムライン機能の実現  
【参考】https://www.slideshare.net/prir/2012-16  
タイムラインの特徴は、ユーザーによって表示内容が違うということ。  
#### 実現するには2種類の方法がある。  
- ***1.表示内容を動的に作成する***  
ユーザーがアクセスしたときに、そのユーザー用の表示内容を動的に作成する  
- ***2.表示内容事前に作成しておく***  
非同期バッチで各ユーザー用の表示内容を事前に作っておく  
  
***1.動的に作成する*** 方法では  
アクセスのたびに処理が動くのでレスポンスに時間がかかることや、  
友達の数に比例して処理が重くなるので致命的。  
  
***2.事前に作成しておく*** 方法では  
まったくアクセスしないユーザーの分も作られるので無駄が多い。  
  
#### そこで折衷案としてredisを使う。  
- 事前に作成しておくもの  
redisはバリューのデータ型でソート済みセットというものがあり、これを利用して  
ユーザーの投稿を時間でソートして持っておく。  
- 動的に作成するもの  
ユーザーがタイムラインを参照するとき、事前に作成しておいた友達の投稿を全員分マージするだけ  
- redis独自  
レスポンス内容をredisでキャッシュしておくことでレスポンスを早くできる。  

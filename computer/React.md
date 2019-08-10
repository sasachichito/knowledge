# React  
JavaScriptライブラリ  
コンポーネントと呼ばれる概念を用いる。  
  
### Virtual DOM  
DOM(Document Object Model)はブラウザで用いられるXML文書のオブジェクトモデル。  
ブラウザはレンダリングの中でDOMツリーを構築し描画している。  
jsはDOMを操作するがパフォーマンスが悪いため、  
Reactでは仮想DOMを操作し、内部でDOMとの最小限の差分を自動更新する仕組みを持つ。  
  
### Flux  
クライアントサイドのアーキテクチャの一種。  
データを一方通行にして簡易化する。  
  
### Redux  
Fluxの実装。reduxパッケージで提供されるjsライブラリ。  
扱う概念  
```  
Action... イベントを表すオブジェクト  
Reducer... Actionを受け取り内容に応じてStateを変更する関数  
Store... reduxパッケージで提供されるオブジェクトで内部にReducerを一つ持つ  
  
データと処理の流れはAction→Store→Reducer→State→Subscriber  
```  
アプリケーションではActionをStoreに渡すだけで済み、State管理を隠蔽できる。  
  
ReactでReduxを利用する場合、  
```  
1. イベント発生時にActionを生成してStoreのdispatchメソッドに渡すことでStateを更新  
2. StoreからStateを取得して利用する  
3. StoreのsubscribeメソッドでrenderApp(react-domのrender関数を実行するメソッド)を設定してState変更→描画を実現  
```  
などが方法となる。  
  
Reactでもっと簡単にReduxを利用するためにreact-reduxパッケージが提供されている。  
  
#### ・react-redux  
ReactとReduxを組み合わせるのを手助けするライブラリ。  
本来ReactはViewのみを扱うためActionやStoreを意識すべきではなく、  
```  
1. イベント発生時には対応する関数（イベントハンドラ）を呼ぶ  
2. StateはPropsで渡される  
3. State変更、再描画などのsubscribe処理は書かない  
```  
とあるべき。  
  
react-reduxはこれを実現する。  
react-reduxによりReactコンポーネントには必要なStateやイベントハンドラがPropsで渡されsubscribe処理は隠蔽される。  
  
扱う概念  
```  
Containerコンポーネント... ReactコンポーネントをラップしてStoreやActionをPropsとして渡す役割  
Presentationalコンポーネント... Reduxに依存しない純粋なReactコンポーネント  
```  
  
index.jsではContainerコンポーネントを操作する。  
  
コンテナコンポーネントは内部でconnectを使って、Presentationalコンポーネントをコネクトしてexport defaultする。  
```  
・mapStateToProps... 引数にStoreのStateを受け取り、必要な情報をオブジェクトで返す。  
・mapDispatchToProps... 引数にStoreのdispatch関数を受け取り、それをラップした関数をオブジェクトで返す  
・connect... 引数にmapStateToProps, mapDispatchToPropsを受け取り、ラッパー関数※を返す  
※引数にコンポーネントを受け取りコネクトする（コネクトされたラッパーコンポーネントを返す）  
```  
  
これによりPresentationalコンポーネントでRedux依存がなくなる。  
  
### JSX  
JavaScriptを拡張させた言語。  
jsとHTMLを同時に扱える。  
  
### Babel  
トランスパイラ(ソースコードからソースコードへ変換するツール)。  
JSXからJavaScriptへ変換する。CLIやwebpackなどから利用。  
  
### webpack  
モジュールバンドラー。  
様々なソースファイルを束ねてブラウザで実行可能な静的なJavaScriptファイルを出力する。  
束ねる際に処理を挟めるをLoaderという仕組みがあり、JSXからjsへの変換はこれで行なっている。(babel-loader)  
  
### コンポーネント  
コンポーネントは大きく2種類。  
```  
・関数コンポーネント  
・クラスコンポーネント  
```  
コンポーネントはプロパティと、(クラスコンポーネントの場合は)状態を持つ。  
プロパティ(props)... 親コンポーネントから引き継ぐ読み取り専用プロパティ  
状態(state)... コンポーネントの状態を保持する読み書き可能プロパティ  
  
### イベント  
JSXでイベントの発生とイベントハンドラ(関数)を設定できる。  
イベントハンドラ内のthisの参照を決定するbind処理がある。  
  
### ルーティング  
URLとコンポーネントを紐づけることでSPAを実現する。  
  
ルーティングのライブラリ  
・react-router  
react-router-domパッケージで提供されるライブラリ  
  
・react-router-redux  
react-routerをreduxに最適化したライブラリ。react-routerと一緒に利用する。  
ルーティング情報をStoreのStateで管理できる。  
  
## npm  
##### -gオプション  
インストールの種類を決める（グローバルかローカル）  
参考 https://qiita.com/kijitoraneko/items/175ef29d45d155b3f405  
  
グローバルインストール：  
-gを付ける `npm install -g hogehoge`  
npmのインストール場所にパッケージをインストールする。  
`/Users/me/.nodebrew/current/lib/node_modules`この辺り。  
  
ローカルインストール：  
-gを付けない  
カレントディレクトリのnode_modules内にインストールする。  
  
パッケージが実行ファイル(js)を含んでいる場合、  
それらのシンボリックリンクを`node_module/.bin`配下に配置する。  
  
##### saveオプション  
依存関係の種類を決める（開発用か本番用）  
参考 https://qiita.com/havveFn/items/c5beda8572aa8c1e6be6  
https://qiita.com/heyheyww/items/092fcbc490a249a2d05c  
  
開発用：  
--save-devを付ける `npm install --save-dev hogehoge`  
package.jsonは以下になる。  
```  
{  
  "devDependencies": {  
    "hogehoge": "^6.26.0"  
  }  
}  
```  
  
本番用 :   
--saveを付けるか何もつけない  
```  
{  
  "dependencies": {  
    "hogehoge": "^6.26.0"  
  }  
}  
```  
  
package.jsonをgit管理しておき、cloneして`npm install --production`とすると  
開発用のパッケージはインストールされない。  
  
## 同一生成元ポリシー  
セキュリティ・プライバシーの観点から、Webページがその生成元となるOrigin以外のサーバーと通信できない仕組み。  
ただしimgやjsはその範疇ではない。  
Origin以外と通信するための仕組みとしてJSONPとCORSの2種類がある。  
  
### JSONP  
jsがOrigin以外から取得できることを利用した方法。  
外部サーバーから取得したいデータ(X)をjsとして取得する。  
  
具体的な方法 :   
例えば外部サーバーから取得するjs(A)の処理内容を、クライアントのjs(B)で利用しているグローバル変数にデータ(X)を設定するものにする。  
簡単に見えるがB側がAの処理実行タイミングを制御しにくいため、グローバル変数に値が設定されていることを担保するのが難しい。  
そこで通常使われる方法は、Aの処理内容をB側で定義済みの関数呼び出しにしておき、その引数にXを設定する。  
こうすればBにとっては、Aを取得する=Xで関数を実行する、となるのでシンプルである。  
※関数名はサーバーへのリクエストパラメータで渡してあげれば、サーバー側が関数名を知っておく必要はなくなる。  
  
JSONPはセキュリティ上の問題から非推奨であり、CORSが推奨されている。  
  
### CORS  
Origin以外のサーバーと通信する仕組み。  
参考 https://qiita.com/tomoyukilabs/items/81698edd5812ff6acb34  
https://qiita.com/masaoki/items/dea5843c9baf59bee2dc  
  
## ECMAScript  
JavaScriptの言語仕様。ESと略される。  
多くのエディションがあり現時点でES2018が最新である。  
全てのブラウザが常に最新ESに対応しているわけではないので、  
トランスパイラで旧ESに変換することもある。  
  
ESとは別にCommonJSと呼ばれる言語仕様もある。  
  
### ES modules  
ES2015で策定されたJavaScriptファイルから別のJavaScriptファイルを読み込む仕様。  
参考 https://sbfl.net/blog/2017/07/26/es-modules-basics/  
別のjsでexportした関数やクラスを、利用したいjsでimportできる。  
```  
export function hello() {  
    console.log('hello!');  
}  
```  
```  
import { hello } from './module.js';  
```  
export defaultとすると、import側が「hello」という関数名を知らずに済む。  
```  
export default function() {  
    console.log('hello!');  
}  
```  
```  
import hogehoge from './module.js';  
```  
  
  

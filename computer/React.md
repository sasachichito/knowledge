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
  
### Redux  
Fluxの実装。  
  
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
コンポーネントはプロパティと状態を持つ。  
プロパティ(props)... 親コンポーネントから引き継ぐ読み取り専用プロパティ  
状態(state)... コンポーネントの状態を保持する読み書き可能プロパティ  
  
### イベント  
JSXでイベントの発生とイベントハンドラ(関数)を設定できる。  
イベントハンドラ内のthisの参照を決定するbind処理がある。  
  
### ルーター  
URLとコンポーネントを紐づける。  
   
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

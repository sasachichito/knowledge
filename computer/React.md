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

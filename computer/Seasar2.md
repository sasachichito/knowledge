## 前知識  
- ビルド  
jarコマンドでwarファイルを作成すること。  
【参考】https://qiita.com/Qui/items/14961678ef939673f744  
- クラスパスルート  
warファイル内の/WEB-INF/classesディレクトリ配下を指す。  
ビルド前のプロジェクトの段階では、プロジェクト名/src/main/resources配下にあるファイルが  
ビルド時にクラスパスルートに配置される設定になっていることが多い。  
  
## 環境の切り替え  
クラスパスルートにあるenv.txtを使用する。  
ut,ct,it,productのいずれかの値を記載する。使い方は4種類ある。  
- diconファイル内で使用する  
`#ENV`でアクセスし、分岐処理を書く。  
- diconファイルの切り替えで使用する  
「app_ut.dicon」のように、「_環境名」のファイルを用意すると、環境名付きのdiconファイルが使用される。
- Javaコードで使用する  
`Env.getValue()`でアクセスし、分岐処理を書く。  
- SMARTデプロイの切り替え  
ENVの値によって実行モードが決定する。  
  
ビルドの段階でwarファイルに含めるenv.txtを切り替える運用方法も見られる。  
  
## SMART deploy  
コンポーネントを自動で登録できる機能（コンポーネントの自動登録）と、
コンテナの初期化（コンポーネント登録）タイミングが異なる三つの実行モードを切り替えることができる仕組みのこと。  

## コンポーネントの自動登録  
SMART deployの仕組みで「ルートパッケージ」配下のクラスを、命名規則に従ってコンポーネントに登録できる。  
「ルートパッケージ」はconvention.diconで定義する。  
※SMART deployに対応するのはルートパッケージ配下のコンポーネントに限定される。  
自動登録の命名規則を定義したクラスは、creator.diconに列挙されている。  
  
自動登録すると、コンポーネントは以下のどれかのグループに属する。  

| コンポーネントグループ | 対応する命名規則定義クラス |
|:-----------|:------------|
|Action|ActionCreator|
|Converter|ConverterCreator|
|Dao|DaoCreator|
|Dto|DtoCreator|
|Dxo|DxoCreator|
|Helper|HelperCreator|
|Interceptor|InterceptorCreator|
|Logic|LogicCreator|
|Page|PageCreator|
|Service|ServiceCreator|
|Validator|ValidatorCreator|

【参考】http://s2container.seasar.org/2.4/ja/S2.4SmartDeployConfig.html  
  
  
SMART deploy以外の方法としては  
ComponentAutoRegister（いくつか種類がある）クラスをdiconファイルにコンポーネントタグで登録し、  
その記載の中でパッケージ名やJar名、クラス名を正規表現等で指定して、一括で登録する。  
  
## DI  
やり方は3種類ある。  
- diconファイルにコンポーネント（クラス）を記載する  
基本の使い方であり、クラスをコンテナに登録、コンテナから取得できる。  
diconファイルの記載やdiconファイル自体を環境によって切り替えることで、コンテナに登録するクラスを変更できる。  
- 自動バインディングを利用する  
あるコンポーネントに対するDIを、diconファイルに記載せずに行う。  
componentタグのautoBinding属性で、そのコンポーネントにどの程度自動バインディングするかを指定する。（省略されている場合はauto）  
自動バインディングで解決される場合でも、diconファイルに明示的に記載している場合は、そちらが優先される。  
- 外部バインディングを利用する  
HTTPリクエストのパラメーター名と同じプロパティに値が自動的にセットされる。  
これを利用するクラスはdiconファイルにその旨を記載する必要がある。 
  
## S2AOP  
コンテナに登録されているコンポーネントにAOPを適用する仕組みのこと。  
実現には２種類ある。  
#### インターセプター  
いわゆるAOP。  
MethodInterceptorインターフェースを実装したクラスをコンポーネントに登録することで独自のインターセプターを作成できる。  
※Seasar2が用意しいているインターセプターは、aop.diconに用意されている。トランザクション関連はj2ee.diconに用意されている。  
適用方法  
・適用したいコンポーネントのdiconファイルの記載にaspectタグを挿入する。  
・自動登録されたコンポーネントの場合、diconファイルに記載が無いので、アノテーションを付与する。  
  
#### インタータイプ  
通常のAOPではなく、クラスの構造を変更することで実現する。  
InterTypeインターフェースを実装することで独自のインターセプターを作成できる。  
※Seasar2が用意しいているインタータイプもある。  
適用方法  
・適用したいコンポーネントのdiconファイルの記載にinterTypeタグを挿入する。  
・自動登録されたコンポーネントの場合、diconファイルに記載が無いので、アノテーションを付与する。  
  
## AOPの自動適用  
前提として、コンポーネントの自動登録等で、自作のインターセプターやインタータイプをコンポーネントとして登録しておく。  
  
コンポーネントの自動登録によって登録された各グループのクラスにAOPを自動適用するためには  
customizer.diconにCustomizerChainクラスをコンポーネントとして登録し、それにAOPを適用したいコンポーネントグループに対応したコンポーネント名をつける。  
CustomizerChainコンポーネントのaddCustomizerメソッドでAspectCustomizerを登録する。  
AspectCustomizerはComponentCustomizerインターフェースを実装したクラスで、いくつか種類があり様々なAOPを実現する。  

| コンポーネントグループ | 対応するCustomizerChainコンポーネント名 |
|:-----------|:------------|
|Action|actionCustomizer|
|Converter|converterCustomizer|
|Dao|daoCustomizer|
|Dto|dtoCustomizer|
|Dxo|dxoCustomizer|
|Helper|helperCustomizer|
|Interceptor|logicCustomizer|
|Logic|interceptorCustomizer|
|Page|pageCustomizer|
|Service|serviceCustomizer|
|Validator|validatorCustomizer|

【参考】http://s2container.seasar.org/2.4/ja/S2.4SmartDeployConfig.html  

その他の方法としては  
AspectAutoRegisterクラスをdiconファイルにコンポーネントタグで登録し、その記載の中でクラスのパターンを正規表現等で指定して  
インターセプターやインタータイプを自動で適用できる。※InterfaceAspectAutoRegisterもある。  
  
## S2JDBC  
Seasar2で使われるJDBCのこと。(JDBCとは https://github.com/sasachichito/Knowledge/blob/master/computer/Java.md#jdbc)  
Seasar2ではS2JDBCインターフェースを実装したJDBCドライバマネージャーを使用する。  
Java標準のjava.sql.DriverManagerクラスとの違いはわかっていない。  
  
### 2WaySQL  
プログラムが外部SQLファイルを使用してDBアクセスするため、  
プログラムでもSQLファイル本体でもクエリを実行できることから2WaySQLと呼ばれる。  
これ自体は標準のJDBCにもある機能。  
  
#### 使い方  
SQLファイルに`/*パラメーター名*/`と入れると、プログラムから値を設定できる。  
**SQLファイル単体で実行するとコメント扱いになる。**  
コメント扱いになると、以下のような記述だとエラーになる  
```  
WHERE id = /*id*/  
```  
これを回避するため以下のように、コメント **直後** にパラメーターを埋め込む  
```  
WHERE id = /*id*/1000  
```  
この埋め込み値はプログラム実行時には削除される。SQLファイルとして実行する場合は有効なので、これで2WaySQLが実現される。  
##### 注意点  
INやNOT INの場合は、プログラムは値を配列かリストで渡す必要がある。  
LIKEの場合は、プログラムは値にワイルドカードを含める必要がある。  
`/*パラメーター名*/`はプレースホルダを利用しているだけなので、カラム名を指定するところでは利用できない。  
カラム名を指定したいときは、プレースホルダではなく文字列置換を利用すればよい。  
`/*$パラメーター名*/`とすると文字列置換になるが、SQLインジェクションが発生する可能性があるので注意。  
##### 条件分岐  
```  
WHERE  
/*IF 条件式1*/  
id = /*id*/1000  
/*END*/  
/*IF 条件式2*/  
AND name = /*name*/'tom'  
/*END*/  
```  
としてプログラムから実行すると、条件式を満たすときだけ、IF～END間のクエリが適用される。  
※SQLファイルとして実行するとid = 1000で実行される。  
プログラムから実行するとき、条件式2だけ満たす場合や、どちらの条件式も満たさない場合は、不正なクエリとなってしまう。  
それを防ぐためには  
```  
/*BEGIN*/  
	WHERE  
	/*IF 条件式1*/  
	id = /*id*/1000  
	/*END*/  
	/*IF 条件式2*/  
	AND name = /*name*/'tom'  
	/*END*/  
/*END*/  
```  
とつける。すると  
BEGIN～END間で最初に条件式を満たしたときに、先頭のANDやORを消してくれたり、  
ひとつも条件式を満たさないときにBEGIN～END間を一切出力しない動作をする。  
  
ELSE句もあるが、SQLファイルとしての整合性を保つため、書き方が特殊になる。  
```  
WHERE  
/*IF 条件式1*/  
id = /*id*/1000  
-- ELSE id = 1000  
/*END*/  
```  

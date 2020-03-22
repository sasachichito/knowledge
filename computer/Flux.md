```
Fluxとは
ステート、アクション、イベント駆動

MVC、MVVMとの関係は？

HTML5との関係は？

ReactiveStreamsとの関係は？

話の流れ
MVの課題(Viewがモデルの利用法/データ形式を知り、見た目と合わせる責務が発生)→MVCでそれらを解決(どのように？)
MVCの課題(コントローラーが肥大化)→MVVMでそれらを解決(どのように？)
MVVMの課題(Viewが)→Fluxでそれらを解決(どのように？データを一方通行化)


[参考1](https://qiita.com/shinkuFencer/items/f2651073fb71416b6cd7)
[参考2](https://qiita.com/knhr__/items/5fec7571dab80e2dcd92#fn1)
[参考3](https://namu-r21.hatenablog.com/entry/2018/03/21/232726)
【MVC】
Model: ビジネスロジックを記述する。
View: Modelの表示と入力の受付。
Controller: Viewの入力を受け取って判断し、Modelを起動する。

MVCには原初MVCとModel2MVCという2種類があるが、そのどちらも目的は同じ。以下の現象を防ぐこと。
コントローラーがないとViewが入力に対してどのモデルを利用すればよいか判断しなければならず、Modelの表示に専念できない。
しようとするとコードが肥大化する。

【MVP】
省略

【MVVM】
ViewModel = Viewの制約（UIの都合）を吸収するためだけのプログラム。
つまりデータバインディングは必須ではない。
```
![代替テキスト](/picture/Flux.png)

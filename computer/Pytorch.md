### requires_grad
tensorが持つフラグ変数。Trueにするとtensorで計算処理を行なった際に、裏でその計算内容を、個別の小さな関数の組み合わせ（合成関数）として整理する（=計算グラフを作る）。  
これにより連鎖律が適用できるため、勾配の計算コストが下がり、`tensor.backword`で勾配を取得できる。  

Falseにすると計算グラフを作らないため、勾配を取得できない。
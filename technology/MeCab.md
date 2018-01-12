参考 https://shimi-dai.com/install-mecab-on-linux/  

# インストール
Dockerfileの例  
```
# git
RUN apt-get install -y git

# mecab core library
RUN apt-get update -y \
    && apt-get install -y \
    libmecab-dev \
    mecab-ipadic \
    mecab-ipadic-utf8 \
    mecab 
    
# php-mecab
RUN git clone https://github.com/rsky/php-mecab.git \
    && cd php-mecab/mecab \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && docker-php-ext-enable mecab \
    && cd .. \
    && rm -rf php-mecab

# mecab辞書
RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git \
    && cd mecab-ipadic-neologd/ \
    && mkdir -p /usr/lib/mecab/dic \
    && ./bin/install-mecab-ipadic-neologd -n -y \
    && cd .. \
    && rm -rf mecab-ipadic-neologd
```
# 辞書  
ユーザー辞書とシステム辞書がある  
【参考】https://qiita.com/hiro0217/items/cfcf801023c0b5e8b1c6  
http://www.mwsoft.jp/programming/nlp/mecab_dictionary_customize.html  
【公式】http://taku910.github.io/mecab/#feature  

まずはMeCabの設定を確認するため`mecabrc`ファイルを見る。(今回は/etc/mecabrcにあった)  
`dicdir = `でシステム辞書を指定、  
`userdic = `でユーザー辞書を指定している。  

任意のシステム辞書をベースにして、独自の追加設定を反映したものがユーザー辞書となる。  
ユーザー辞書を作成するコマンド
```
$ /usr/lib/mecab/mecab-dict-index \
  -d システム辞書のパス \
  -u 出力するユーザ辞書 \
  -f CSVの文字コード \
  -t 辞書の文字コード \
  作成したCSVファイル
```
上記の、「作成したCSVファイル」というのが、独自の追加設定となる。  

形態素解析をする時、システム辞書は必ず利用され、1度に1つのシステム辞書しか指定できない。 
ユーザ辞書は任意で使用し、1度に複数（カンマ区切り）利用することもできる。  

システム辞書に直接独自の設定を追加することもできる。  
その場合、mecab-dict-indexを使うか、辞書を再度makeする。  
makeの場合、**自動的にフォルダ内の拡張子がCSVのファイルが辞書に追加される
```
# 前に作ったname.csvをIPA辞書の配下に置く
$ cp name.csv mecab-ipadic-2.7.0-20070801/
$ cd mecab-ipadic-2.7.0-20070801

# 初make時は、configureとmakeをする
$ ./configure --with-charset=utf8
$ make
$ sudo make install

# 2度目以降はmake cleanとmakeをする
$ make clean
$ make
$ sudo make install
```

















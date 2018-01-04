参考 https://shimi-dai.com/install-mecab-on-linux/

【Gitをインストールする】
RUN apt-get install -y git
↓
↓
↓
【MeCabをインストールする】
MeCabはオープンソースの形態素解析エンジン
すごいざっくり説明すると「文章を意味単位で単語にぶつ切りにする」ライブラリ
RUN git clone https://github.com/taku910/mecab.git \
    && cd mecab/mecab \
    && ./configure  --enable-utf8-only \
    && make && make check \
    && make install \
    && ldconfig
↓
↓
↓
【php_MeCabをインストールする】
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
↓
↓
↓
【辞書をダウンロードし登録する】★ここでエラーになっている？
デフォルトのままだと、微妙に違和感のある区切りかたをしたりするので、
より賢い辞書を使いましょう。とのこと。
# mecab辞書
RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git \
    && cd mecab-ipadic-neologd/ \
    && mkdir -p /usr/lib/mecab/dic \
    && ./bin/install-mecab-ipadic-neologd -n -y \
    && cd .. \
    && rm -rf mecab-ipadic-neologd


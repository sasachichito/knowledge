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

まずはMeCabの設定を確認するため`mecabrc`ファイルを見る。(今回は/etc/mecabrcにあった)  
`dicdir = `でシステム辞書を指定、  
`userdic = `でユーザー辞書を指定している。  





















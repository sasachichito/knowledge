## Gitのサーバーとクライアント
GitではWEBサーバーやDBサーバーのように、必ずしもデーモンプログラムが待ち受けているわけではない。  

Gitサーバーの実態はベアリポジトリであり、それがリモートリポジトリと呼ばれる。  

※ベアリポジトリとは作業スペースがないリポジトリのことで、hookが作成できるなど、リモート用途になっている。  

サーバー(ベアリポジトリ、リモートリポジトリ)への接続方法が4つある。  

1. Localプロトコル  

リモートリポジトリをローカルリポジトリと同じディスク上の別のディレクトリに置き、　
`file://`でアクセスする。

2. SSHプロトコル  

SSH認証を利用して、物理的に離れたマシン上にあるリモートリポジトリに、  
`ssh://`でアクセスする。  

3. Gitプロトコル  

Git に標準で付属する特別なデーモン。専用のポート (9418) をリッスンし、  
SSH プロトコルと同様のサービスを提供するが、認証は行わない。  
`git://`でアクセスする。

4. HTTP/Sプロトコル  

`http://`や`https://`でアクセスする。  


【参考】
https://git-scm.com/book/ja/v1/Git-サーバー-プロトコル


## Gitの設定ファイルは以下の3つ  

```
# システムレベル /mingw64/etc/gitconfig
$ git config --system -l
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
http.sslbackend=openssl
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
credential.helper=manager
$


# ユーザーレベル ~/.gitconfig
$ git config --global -l
credential.helper=manager
core.autocrlf=false
$


# リポジトリレベル .git/config
$ git config --local -l
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=git@gitlab.sockets.tv:msdbcoreproject/msdb-gateway.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.develop.remote=origin
branch.develop.merge=refs/heads/develop
branch.feature/#9_create_es_mappingcommand.remote=origin
branch.feature/#9_create_es_mappingcommand.merge=refs/heads/feature/#9_create_es_mappingcommand
$
```

※同じプロパティがあった場合の優先順位 システムレベル < ユーザーレベル < リポジトリレベル  
システムレベル...マシン上の全てのリポジトリに影響する  
ユーザーレベル...そのユーザーでログインした場合に影響する  
リポジトリレベル...そのリポジトリにのみ影響する  

上記のようにgit configコマンドで確認・編集しても良いし、.gitconfigや.git/configファイルを直接参照・編集しても良い。    

## CRLFの設定について  

上記の設定の段階で、core.autocrlf=false になっている
  
ユーザーレベルでtrueに設定（他にはfalse, inputがある）
[aaa@bbb .git]$ git config --global core.autocrlf false

【参考】  
公式  
https://git-scm.com/book/ja/v1/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%81%AE%E8%A8%AD%E5%AE%9A  
true, false, inputについて  
https://qiita.com/uggds/items/00a1974ec4f115616580

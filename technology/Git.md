## Gitの設定ファイルは以下の3つ  

```
# システムレベル /mingw64/etc/gitconfig
[aaa@bbb msdb-gateway]$ git config --system -l
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
http.sslbackend=openssl
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
credential.helper=manager
[aaa@bbb msdb-gateway]$


# ユーザーレベル ~/.gitconfig
[aaa@bbb msdb-gateway]$ git config --global -l
credential.helper=manager
core.autocrlf=false
[aaa@bbb msdb-gateway]$


# リポジトリレベル .git/config
[aaa@bbb .git]$ git config --local -l
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
[aaa@bbb .git]$
```

※同じプロパティがあった場合の優先順位 システムレベル < ユーザーレベル < リポジトリレベル  
システムレベル...マシン上の全てのリポジトリに影響する  
ユーザーレベル...そのユーザーでログインした場合に影響する  
リポジトリレベル...そのリポジトリにのみ影響する  

上記のようにgit configコマンドで確認・編集しても良いし、.gitconfigや.git/configファイルを直接参照・編集しても良い。    

## CRLFの設定について  

この段階で、core.autocrlf=false になっている
  
ユーザーレベルでtrueに設定（他にはfalse, inputがある）
[aaa@bbb .git]$ git config --global core.autocrlf false

【参考】  
公式  
https://git-scm.com/book/ja/v1/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%81%AE%E8%A8%AD%E5%AE%9A  
true, false, inputについて  
https://qiita.com/uggds/items/00a1974ec4f115616580

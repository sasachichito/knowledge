メモ
```
・ユーザー
k8sを操作するアカウントはUserAccountとServiceAccountの2種類がある。
UserAccountはk8sが管理するものではない。ServiceAccountはk8sが管理している。
UserAccountはクラスタで一意、ServiceAccountはNameSpace毎に一意である。
Podには必ず一つのServiceAccountが紐づく。（指定しない場合はdefault ServiceAccountが紐づく）

・認証・認可
k8sのAPIを叩くとき、認証・認可が行われる。
# 認証
kubectlなどでクラスタ外から叩くときはkubeconfig（UserAccount）の認証情報を利用して認証する。
pod内からのcurlなどでクラスタ内から叩くときはServiceAccountのトークンを利用して認証する。
# 認可
様々な認可方法がありRBACが推奨されている。

・RBAC
UserAccountやServiceAccountをロールと紐付けている。
# NamespaceレベルのRBAC用リソース
Role
RoleBinding
# クラスタレベルのRBAC用リソース
ClusterRole
ClusterRoleBinding
```

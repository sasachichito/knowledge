メモ
```
ローカル環境にk8s構築するには2つの選択肢がある。
### Docker For Desktop
GUIから操作して構築
ダッシュボード表示：https://github.com/kubernetes/dashboard
### Minikube
・minikube取得
minikube-installer.exeでインストール
https://github.com/kubernetes/minikube/releases/latest
・Hyper-Vでminikube構築
https://minikube.sigs.k8s.io/docs/reference/drivers/hyperv/
管理者権限でPowerShell起動：Start-Process powershell.exe -Verb runas
minikube start --vm-driver=hyperv --memory='1024mb'
デフォルトのメモリ2000mbだと空き不足で構築失敗したため明示的に指定。
Hyper-Vマネージャー上から見ると結局2000mbで構築されているよう。
```
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

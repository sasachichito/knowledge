# SCP  
SCPはOpenStack(IaaS構築ソフトウェア)と、CloudFoundry、Neo(PaaS構築ソフトウェア)で構成され、  
ユーザーに対し以下を提供する。  
・PaaS  
・SAPによってPaaS上に構築されたサービス(SaaS)  
  
ユーザーはSCPの使用に際し、CloudFoundry(CF)版かNeo版かを選択する。  
  
Neo版はSAPによって運用されているサーバ上で稼働し、  
CF版はAWSやAzure、GCPなどのサードパーティデータセンタプロバイダ上で稼働する。  
  
NeoとCFでは、利用できる機能や製品バージョンが異なる。  
https://blogs.sap.com/2019/05/23/sdi%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6cloud-foundry%E7%92%B0%E5%A2%83%E3%81%AEhana-db%E3%81%A8neo%E7%92%B0%E5%A2%83%E3%81%AEhana-db%E3%82%92%E6%8E%A5%E7%B6%9A%E3%81%99%E3%82%8B%E6%96%B9/  
  
GitはNeo版、ABAPの環境はCF版にある模様  
https://blogs.sap.com/2020/03/28/service-mapping-of-sap-cloud-platform-neo-and-cloud-foundry-environments/  
  
【その他参考リンク】  
https://blogs.sap.com/2019/02/24/sap-cloud-platform-environment-cloud-foundry-vs-neo/  
https://techtarget.itmedia.co.jp/tt/news/2001/09/news04.html  
  
### SAP Cloud SDK  
S/4HANA Cloud, SuccessFactors, C/4HANA等のSCPサービスと対話するためのプログラミング言語ライブラリであり、  
現時点でJava, JavaScriptに対応している。  
  
### SAP Cloud Application Programming Model  
SCP上のCDS、Java、Node.js実行環境で稼働するアプリケーションを開発するプログラミングモデルである。  
JavaとNode.jsそれぞれにSCPのSaaSと対話するためのライブラリを提供している。  
  
# マルチターゲットアプリケーション(MTA)  
SCPにおける概念であり、  
HTML・Node.js・Java・HANADB(DDL)など異なる環境にデプロイされるアプリのソースコードを一つにまとめたパッケージ。  
  
# SAP Solution Manager  
SAPシステム群の管理ツールであり、Solmanと略される。  
NetWeaverやERP(S4等)の機能ではなく、別途インストールして構築したりSaaSを利用する必要がある。  
  
CTS/CTS+（拡張移送/修正システム）、cCTS（セントラルCTS）、QGM（クオリティゲート管理）、  
Transport Analysys/Change Diagnostics（変更診断）、Retrofit（デュアルランドスケープ同期）、  
ChaRM（変更依頼管理）など様々な機能を搭載している。  
  
※CTS/CTS+は当然NetWeaverにも搭載されている。  
  
これらの機能は、アプリケーションへの変更の要請から、承認、そして最終的な実施（リリース）に至るまでをワークフロー化し、  
その過程における全ての状況を文書化する。  
  
# SAP Data Service  
データ活用基盤ツールである。  
NetWeaverやERP(S4等)の機能ではなく、別途インストールして構築したりSaaSを利用する必要がある。  
  
オブジェクトプロモーション機能では、1つ以上のData Servicesオブジェクト(本ツール上での設定・開発物)を  
本ツールのDEV環境からQAまたはPROD環境にデプロイ可能。  
  
### cCTS  
https://blogs.sap.com/2014/01/28/central-cts/  
  
### ChaRM  
https://blogs.sap.com/2014/09/22/basic-procedure-for-change-management-charm/  
  
### QGM  
https://corealm.com/blog/what-is-quality-gate-management/  
  
# CTS+  
CTS+は、Enhanced Change and Transport Systemを指す。  
従来の移送を拡張し、NonABAPコンテンツ(Data Serviceオブジェクトも含む)も移送可能にしたものである。  
  
・オンプレ環境でのCTS+のユースケース  
①ABAPやNonABAPコンテンツの移送  
②SCPで開発したMTAをオンプレ環境への移送  
  
・SCP環境でのCTS+のユースケース  
①MTAの移送  
※SCP環境の各種開発物でMTAをサポートしているものが少ないため、CTS+ではなく後述のTMSを使用することが想定される。  
  
CTS+はSolution ManagerのChaRMやQGMと連携することが可能。  
  
CTS+は文脈によって以下の異なる意味を持つ。  
・NetWeaverの機能としてのCTS+  
・Solution Managerの機能としてのCTS+  
・Data Serviceのオブジェクトプロモーション機能としてのCTS+  
  
# TMS  
TMSはSAP Cloud Platform Transport Management Serviceを指す。  
SCP上(NeoとCF両方)での開発コンテンツ移送で使用する(オンプレでは使用不可)。  
  
https://blogs.sap.com/2020/01/31/interplay-of-sap-cloud-platform-transport-management-cts-and-charm-in-hybrid-landscapes/  
  
# CI/CD  
SAPのPiperと呼ばれるプロジェクトでCI/CDのツール(JenkinsサーバーのDockerイメージ等)や  
ライブラリの作成(Jenkinsのプラグイン等)が行われている。  
https://sap.github.io/jenkins-library/  
  
ここによるとデプロイメントパイプラインの実現方法の種類は大きく以下に分かれる模様  
・移送が不要な場合、CIサーバーからDEV/QA/PRODのデプロイを全てキックする  
・移送が必要な場合、CIサーバーからSAPツール(オンプレ:Solman、SCP:TMS)に制御を渡す  
  
※CIサーバー = Jenkins等

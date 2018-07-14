# seleniumとは  
いくつか種類がある  
【参考】https://blog.trident-qa.com/2013/05/so-many-seleniums/  
  
Selenium Webdriverを利用するのが良さそう。  
テストスクリプトをプログラミング言語で書ける。  
  
Selenium WebdriverはJavaScriptを使用せずブラウザの拡張機能やOSの機能を利用するため、  
Selenium RCのようにWebサーバーから受け取ったHTMLにJavaScriptを埋め込む中継サーバー(Selenium Server)が必要ない。  
  
Selenium IDEのように画面操作からテストスクリプトを自動生成したいがSelenium Webdriverにその機能はない。  
Selenium IDEが生成したテストスクリプトはSelenium RCのコマンドで記載されているため、Selenium Webdriverでは利用できない。  
（Selenium Webdriver用にエクスポートできるという話もある）  
  
Selenium BuilderというSelenium IDEの後継を利用すれば、画面操作からSelenium Webdriverで利用できるテストスクリプトを生成できる(しかも好きな言語で)  
  
# 良い使い方  
Selenium Webdriverをインストール  
↓  
FireFoxにSelenium Builder(Selenium IDEの後継)をインストール https://cloudear.jp/blog/?p=1042  
↓  
画面操作し、Selenium Webdriver用テストスクリプトを好きなプログラミング言語で自動生成し、バージョン管理  
↓  
Selenium Webdriverでテストスクリプトを実行  
  

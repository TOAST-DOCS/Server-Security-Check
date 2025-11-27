## Security > Server Security Check > コンソール使用ガイド

ここでは点検Agent実行手順を説明します。 

## Agent実行手順

インスタンスOS、点検の種類、点検基準を選択し、Agent実行スクリプトを呼び出します。

![serversecuritycheck_01_20201022_ja.png](https://static.toastoven.net/prod_serversecuritycheck/serversecuritycheck_01_20201022_ja.png)

### Linux系列Agent

1\. 実行スクリプトをコピーするにはクリップボードコピーをクリックします。

2\. 実行対象インスタンスターミナルに接続します。

3\. 管理者権限でAgentスクリプトを作成して実行します。

* root権限を取得します。
* viエディタなどでスクリプトを作成します。
* 作成したスクリプトファイルの権限を変更します。
* ファイルを実行します。
```
[root@centos7 ~]# cd ~
[root@centos7 ~]# sudo su
[root@centos7 ~]# vi agent.sh
[root@centos7 ~]# chmod 744 agent.sh
[root@centos7 ~]# ./agent.sh
OS Security Check Success! :)
```


## Server Security Check サービスゲートウェイ連携
サービスゲートウェイを利用すると、NHN Cloud内部でクライアントとServer Security Checkが通信する際、外部インターネットを経由せず、内部ネットワークで通信できます。
Server Security Checkサービスゲートウェイを連携する方法は次のとおりです。

1. **Network > Service Gateway** ページへ移動して、**+ サービスゲートウェイ作成** をクリックします。
2. 作成するサービスゲートウェイの **名前**、**VPC**、**サブネット** を入力し、**サービス** を **Server Security Check** に選択した後、**確認** をクリックすると、Server Security Checkサービスゲートウェイが作成されます。

![ssc_sg_251117.png](https://static.toastoven.net/prod_serversecuritycheck/ssc_sg_251117.png)

### ホスト登録
インスタンスでServer Security Check EndpointのIPを見つけられるように、ホストファイルにServer Security CheckサービスゲートウェイのIPアドレスとServer Security Check Endpointドメインを入力します。
Server Security CheckサービスゲートウェイのIPアドレスは、**Network > Service Gateway** ページで確認できます。

**Windows**

C:\Windows\System32\drivers\etc\hostsファイルを開き、以下の内容を追加します。

```
{Server Security Check サービスゲートウェイIPアドレス} api-serversecuritycheck.alpha-nhncloudservice.com
```

**Linux**

/etc/hostsファイルを開き、以下の内容を追加します。

```
{Server Security Check サービスゲートウェイIPアドレス} api-serversecuritycheck.alpha-nhncloudservice.com
```



## お問い合わせ

### お問い合わせ対象

1\. Agent実行失敗に関するお問い合わせ

2\. 点検結果に対する誤検知申告

### お問い合わせ方法

1\. お問い合わせ方法: **サポート > 1：1お問い合わせ**

2\. 対応時間：平日09:00～18:00

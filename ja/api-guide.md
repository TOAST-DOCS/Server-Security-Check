## Security > Server Security Check > APIガイド

Server Security Check Public APIについて説明します。

## 共通の準備事項

APIを使用するには、APIエンドポイントとトークンが必要です。

### APIエンドポイント

| リージョン | エンドポイント |
| --- | ----- |
| 全てのリージョン | https://kr1-server-security-check.api.nhncloudservice.com |

### 認証トークンの発行

Server Security Checkは、APIの認証・認可のためにNHN Cloudトークンを利用します。
[NHN Cloud APIの呼び出しと認証](https://docs.nhncloud.com/ja/nhncloud/ja/public-api/api-authentication/)を参照し、認証トークンの使用に必要な情報を確認します。

## API利用の共通情報

### APIリクエスト共通情報

APIを使用するには、以下の情報が必要です。

* トークン発行後、APIヘッダにトークン情報を含めます。

| 名前 | 区分 | 型 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| x-nhn-authorization | Header | String | O | トークン |

* サービスAppkey
    * Server Security Checkコンソール右上の**URL & Appkey**メニュー、またはプロジェクト管理の**利用中のサービス**から確認できます。
    * サービスのURL PathにAppKeyが含まれます。

### APIレスポンス共通情報

* APIリクエストへの応答として、以下のようなレスポンスコードを返すことがあります。
    * 200 OK
    * 400 Bad Request
    * 401 Unauthorized
    * 404 Not Found
    * 413 Payload Too Large
    * 405 Method Not Allowed
    * 500 Internal Server Error
* 全てのレスポンスコードは、共通のレスポンスボディを含みます。
    * 共通レスポンスボディ

| 名前 | 区分 | 型 | 説明 |
| --- | --- | --- | --- |
| header | Body | Object |  |
| header.isSuccessful | Body | Boolean | true:正常<br>false:エラー |
| header.resultCode | Body | Integer | 0:正常<br>その他:エラー |
| header.resultMessage | Body | String | "SUCCESS":正常<br>その他:エラー原因メッセージ |

* <span style="color: rgb(49, 51, 56);">共通レスポンスボディ以外の詳細な応答結果は、レスポンス本文のヘッダを参照してください。</span>

> [注意] APIの応答に、ガイドに記載されていないフィールドが含まれることがあります。これらのフィールドはNHN Cloudの内部目的で使用され、事前の通知なく変更される可能性があるため、使用しないでください。

## Server Security Check

### 点検結果の要約照会

指定した期間の点検結果を要約して照会します。
最大1か月以内の点検結果を照会できます。

```
GET "/ssc/v1.0/appKey/{appKey}/inspection_result/summary"
x-nhn-authorization: {token-id}
```

<br>

##### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 区分 | 型 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| appKey | URL Path | String | O | サービスAppkey |
| regionCode | Query | String | O | リージョン情報(KR1, KR2, ...) |
| language | Query | String | X | KO, EN, JA(デフォルト値: KO) |
| from | Query | <span style="color: rgb(49, 51, 56);">DateTime</span> | O | 検索開始時間<span style="color: rgb(49, 51, 56);">(</span><span style="color: oklch(0.3039 0.04 213.68);">YYYY-MM-DDTHH:mm:ss±hh:mm</span><span style="color: rgb(49, 51, 56);">)</span><br>例: 2025-06-17T00:00:00%2B09:00 |
| to | Query | <span style="color: rgb(49, 51, 56);">DateTime</span> | O | 検索終了時間<span style="color: rgb(49, 51, 56);">(</span><span style="color: oklch(0.3039 0.04 213.68);">YYYY-MM-DDTHH:mm:ss±hh:mm</span><span style="color: rgb(49, 51, 56);">)</span><br>例: 2025-06-17T23:59:59%2B09:00 |
| page | Query | Integer | X | 照会するページ番号(デフォルト値: 1) |
| limit | Query | Integer | X | 照会するページサイズ(デフォルト値: 10、最大: 1000) |
| kind | Query | ENUM | X | 点検種類(OS, WAS)<br>現在はOSのみサポート |
| bss | Query | ENUM | X | 点検基準("M":主要情報通信基盤施設、 "F":電子金融基盤施設) |

##### レスポンス

| 名前 | 区分 | 型 | 説明 |
| --- | --- | --- | --- |
| usageStasNo | Body | String | 点検結果のシリアルナンバー |
| instanceName | Body | String | 点検対象インスタンスの名前 |
| os | Body | ENUM | 点検対象インスタンスのOS<br>(Windows、Linux) |
| systemVersion | Body | String | 点検対象インスタンスのOSバージョン |
| bss | Body | ENUM | 点検基準("M":主要情報通信基盤施設、 "F":電子金融基盤施設) |
| scriptVersion | Body | String | 点検スクリプトのバージョン |
| executionTime | Body | DateTime | 点検実行時間 |
| checkCount | Body | Integer | 点検項目数 |
| weakCount | Body | Integer | 脆弱点の数 |
| level3WeakCount | Body | Integer | 脆弱性レベル「高」の数 |
| level2WeakCount | Body | Integer | 脆弱性レベル「中」の数 |
| level1WeakCount | Body | Integer | 脆弱性レベル「低」の数 |

<details>
<summary><span>例</span></summary>

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "success": true
    },
    "results": [
        {
            "usageStasNo": 1,
            "instanceName": "test-ubuntu-1",
            "os": "Linux",
            "kind": "OS",
            "systemVersion": "ubuntu Server 22.04 LTS",
            "bss": "M",
            "scriptVersion": "G-1",
            "executionTime": "2025-07-17T11:42:20+09:00",
            "checkCount": 65,
            "weakCount": 15,
            "level3WeakCount": 5,
            "level2WeakCount": 5,
            "level1WeakCount": 5
        },
        {
            "usageStasNo": 2,
            "instanceName": "test-ubuntu-2",
            "os": "Linux",
            "kind": "OS",
            "systemVersion": "ubuntu Server 22.04 LTS",
            "bss": "M",
            "scriptVersion": "G-1",
            "executionTime": "2025-07-16T15:11:23+09:00",
            "checkCount": 65,
            "weakCount": 15,
            "level3WeakCount": 5,
            "level2WeakCount": 5,
            "level1WeakCount": 5
        }
    ],
    "page": {
        "itemPerPage": 20,
        "page": 1,
        "totalCount": 2
    }
}
```

<br>

</details>

### 点検結果の詳細照会

点検結果の要約を照会した後、点検結果番号で特定の点検結果を詳細に照会します。

```
GET "/ssc/v1.0/appKey/{appKey}/inspection_result/details/{usageStasNo}"
x-nhn-authorization: {token-id}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 区分 | 型 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| appKey | URL | String | O | サービスAppkey |
| usageStasNo | URL | Integer | O | 点検結果番号 |
| language | Query | String | X | KO, EN, JA(デフォルト値: KO) |

#### レスポンス

| 名前 | 区分 | 型 | 説明 |
| --- | --- | --- | --- |
| categoryName | Body | String | 点検分類 |
| resultId | Body | String | 分析結果ID |
| weakLevel | Body | ENUM | 脆弱性レベル("H"、"M"、"L") |
| weakLevelName | Body | String | 脆弱性レベルのenum名 |
| resultCode | Body | String | 点検周期の設定 |
| itemName | Body | String | 項目名 |
| manageMethod | Body | String | 対応策 |

<details>
<summary><span>例</span></summary>

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS",
        "success": true
    },
    "results": [
        {
            "categoryName": "1. アカウント管理",
            "resultId": "U-01",
            "weakLevel": "H",
            "resultCode": "X",
            "weakLevelName": "高"、
            "itemName": "rootアカウントのリモート接続制限"、
            "manageMethod": "1. 「/etc/securetty」ファイルからpts/0～pts/xの設定を削除またはコメントアウト<br>2. 「/etc/pam.d/login」ファイルを修正または新規挿入<br>auth required /lib/security/pam_securitty.so"
        },
        {
            "categoryName": "1. アカウント管理",
            "resultId": "U-02",
            "weakLevel": "H",
            "resultCode": "X",
            "weakLevelName": "高"、
            "itemName": "パスワードの複雑性の設定"、
            "manageMethod": "[Linux - RHEL5]<br>1. パスワード複雑性設定ファイルの確認<br># /etc/pam.d/system-auth、/etc/login.defsの内容を内部ポリシーに合わせて編集<br>2. /etc/pam.d/system-authファイルの設定<br>※次の行にパスワードポリシーを設定<br>- パスワードポリシー設定例<br>password requisite /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1<br>3. /etc/login.defsファイルの点検<br>pass_warn_age = 7 (パスワード有効期限の警告)<br>pass_max_days = 60 (パスワードの最大使用期間設定)<br>pass_min_day = 1 (パスワードの最小変更期間設定)<br><br>[Linux - RHEL7]<br>1. パスワード複雑性設定ファイルの確認<br># /etc/pam.d/system-auth、/etc/login.defsの内容を内部ポリシーに合わせて編集<br>2. /etc/pam.d/system-authファイルの設定<br>※次の行にパスワードポリシーを設定<br>- パスワードポリシー設定例<br>password requisite /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1<br>または<br>password requisite /lib/security/$ISA/pam_pwquality.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1<br>3. /etc/login.defsファイルの点検<br>pass_warn_age = 7 (パスワード有効期限の警告)<br>pass_max_days = 60 (パスワードの最大使用期間設定)<br>pass_min_day = 1 (パスワードの最小変更期間設定)<br><br>[Linux - Ubuntu]<br>1. パスワード複雑性設定ファイルの確認<br>/etc/pam.d/common-authを内部ポリシーに合わせて編集<br>2. /etc/pam.d/common-authファイルの設定<br>※次の行にパスワードポリシーを設定<br>- パスワードポリシー設定例<br>password requisite /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1"
        },
        {
            "categoryName": "1. アカウント管理",
            "resultId": "U-03",
            "weakLevel": "H",
            "resultCode": "X",
            "weakLevelName": "高"、
            "itemName": "アカウントロックのしきい値設定"、
            "manageMethod": "1. viエディタで「/etc/pam.d/system-auth」ファイルを開く<br>2. 以下のように修正または新規挿入<br>/etc/pam.d/system-authファイルに以下を追加する。<br>auth required /lib/security/pam_tally2.so deny=5 unlock_time=120 no_magic_root<br>account required /lib/security/pam_tally2.so no_magic_root reset"
        },
        {
            "categoryName": "1. アカウント管理",
            "resultId": "U-04",
            "weakLevel": "H",
            "resultCode": "O",
            "weakLevelName": "高"、
            "itemName": "パスワードファイルの保護"、
            "manageMethod": "1. /shadowファイルの存在確認<br>(通常は/etcディレクトリ内に存在)<br># ls /etc<br>2. /etc/passwdファイル内の2番目のフィールドが「x」と表示されているか確認<br># cat /etc/passwd<br>root:x:0:0:root:/root:/bin/bash"
        },
        {
            "categoryName": "2. ファイル及びディレクトリ管理",
            "resultId": "U-05",
            "weakLevel": "H",
            "resultCode": "O",
            "weakLevelName": "高"、
            "itemName": "rootのホームディレクトリ及びパスディレクトリの権限とパス設定"、
            "manageMethod": "1. viエディタを利用してrootアカウントの設定ファイル(~/.profileと/etc/profile)を開く<br># vi /etc/profile<br>2. 以下のように修正<br>(修正前) PATH=.:$PATH:$HOME/bin<br>(修正後) PATH=$PATH:$HOME/bin:."
        }
    ]
}
```

<br>

</details>

<br>

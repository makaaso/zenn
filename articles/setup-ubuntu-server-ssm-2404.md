---
title: "UbuntuのEC2にSession Managerで接続するための初期設定" # title
emoji: "🦛" # icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["aws", "ubuntu", "sessionmanager"]
published: true
---
AWSのEC2をUbuntuで起動した時にSession Managerで接続するための初期設定の備忘録となります。

# 環境
- AMI: Ubuntu24.04(ami-0b20f552f63953f0e)

# 設定方法
## 初期状態
UbuntuのEC2を起動した直後の状態はこちら。
![](https://storage.googleapis.com/zenn-user-upload/f95eee82e39e-20241129.png)

解決するために下記の項目を対応する必要があります。
- EC2インスタンスにSession Managerで接続するための権限を付与したIAMロールをアタッチ
- UbuntuにSession Managerプラグインをインストール

## EC2インスタンスにSession Managerで接続するための権限付与
初期状態は以下の通り。
![](https://storage.googleapis.com/zenn-user-upload/8ad4f27cdd87-20241129.png)

### IAMロール作成
IAMロールが存在しない場合、以下の手順で作成。

`Create role` を選択。
![](https://storage.googleapis.com/zenn-user-upload/783cbfb98649-20241129.png)

下記項目を選択後、 `Next` を選択。
- Trusted entity type: `AWS service`
- Use case： `EC2 Role for AWS Systems Manager`

![](https://storage.googleapis.com/zenn-user-upload/45ae9c339f12-20241129.png)

`Next` を選択。
![](https://storage.googleapis.com/zenn-user-upload/25bc9e9eb62b-20241129.png)

Role nameを入力後、 `Create role` を選択。
![](https://storage.googleapis.com/zenn-user-upload/2a4debb7cbc4-20241129.png)

作成完了。
![](https://storage.googleapis.com/zenn-user-upload/17e7c934d252-20241129.png)

### IAMロールをEC2インスタンスにアタッチ

`Modify IAM role` を選択。
![](https://storage.googleapis.com/zenn-user-upload/f3cc9b831739-20241129.png)

IAM role選択後、　`Update IAM role` を選択。
![](https://storage.googleapis.com/zenn-user-upload/dca3ef75b548-20241129.png)

## Session Managerプラグインインストール
User Dataを更新するため、EC2インスタンスを停止。
![](https://storage.googleapis.com/zenn-user-upload/26fac57000f0-20241129.png)

![](https://storage.googleapis.com/zenn-user-upload/3424fca61cbe-20241129.png)

User Dataに Session Managerプラグインをインストールするコマンドを設定。
```
#!/bin/bash
mkdir /tmp/ssm
cd /tmp/ssm
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo systemctl enable amazon-ssm-agent
```

設定後、 `Save` を選択。
![](https://storage.googleapis.com/zenn-user-upload/02f9af4e95c8-20241129.png)

EC2インスタンスを起動。
![](https://storage.googleapis.com/zenn-user-upload/48584ac6eab9-20241129.png)

Session Manager経由でEC2インスタンスに接続
![](https://storage.googleapis.com/zenn-user-upload/c2e65cb6dc80-20241129.png)

接続できることを確認。
![](https://storage.googleapis.com/zenn-user-upload/768b929b6b30-20241129.png)

# 最後に
- User DataでSession Managerプラグインをインストールできるのはありがたい

# 参考
- [AWSのマネジメントコンソールでSession Managerで仮想サーバーにログインするためのIAMロールを作成する](https://qiita.com/h_horiguchi/items/813d5944e270aa0e2069)
- [Debian Server と Ubuntu Server での Session Manager プラグインのインストール](https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/install-plugin-debian-and-ubuntu.html)
- [Amazon EC2 Linux インスタンスの起動時に SSM Agent をインストールする方法を教えてください。](https://repost.aws/ja/knowledge-center/install-ssm-agent-ec2-linux)

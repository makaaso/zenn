---
title: "[AWS]UbuntuのEC2にSessionManagerで接続するための初期設定" # title
emoji: "🦛" # icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["aws", "ubuntu", "ssm"]
published: false
---
AWSのEC2をUbuntuで起動した時にSessionManagerで接続するための初期設定の備忘録となります。


# 環境
- AMI: Ubuntu24.04(ami-0b20f552f63953f0e)

# 設定方法
## 初期状態
UbuntuのEC2を起動した直後の状態はこちら。
![](https://storage.googleapis.com/zenn-user-upload/f95eee82e39e-20241129.png)

## ufw有効化
```
sudo ufw enable
```

## IPv4ポート許可設定
```
sudo ufw allow proto tcp to 0.0.0.0/0 port 22
```

## ポート22許可
```
sudo ufw allow 22
```

## ポート22禁止
```
sudo ufw deny 22
```

## 特定のIPから22許可
```
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 22
```

## ルール一覧表示
```
sudo ufw status numbered
```

## ルール削除
```
sudo ufw delete 1
```

# ログ出力先
```
/var/log/ufw.log
```

# 最後に
- とりあえずの初期設定はここまでで適宜調整していこうと思います。本記事がどなたかのお役に立てば幸いです。

# 参考
- [AWSのマネジメントコンソールでSession Managerで仮想サーバーにログインするためのIAMロールを作成する](https://qiita.com/h_horiguchi/items/813d5944e270aa0e2069)
- [Debian Server と Ubuntu Server での Session Manager プラグインのインストール](https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/install-plugin-debian-and-ubuntu.html)

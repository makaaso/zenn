---
title: "Ubuntu Server 24.04のUFW初期設定" #title
emoji: "🦛" #icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ufw", "ubuntu"]
published: true
---
Ubuntuにデフォルトでインストールされている、UFW（Uncomplicated FireWall）の利用を検討しました。
Ubuntu Server 24.04をインストールした後のUFWの初期設定についてまとめておきます。

# 環境
こちらの記事 [Ubuntu Server 24.04 を VirtualBox にインストール](https://zenn.dev/makaaso/articles/install-ubuntu-2404-virtualbox) の環境が前提
- Chip: Apple M2 Max
- macOS: Sequoia 15.0.1
- VirtualBox: 7.1.4
- Ubuntu: Ubuntu Server 24.04

# 設定コマンド
## ufw状態確認
```
sudo ufw status
sudo ufw status verbose
```

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
- [Ubuntu 22.04サーバー構築入門](https://zenn.dev/uchidaryo/books/ubuntu-2204-server-book/viewer/ufw)

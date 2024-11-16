---
title: "Ubuntu Server 24.04のOpenSSH初期設定" #title
emoji: "🦛" #icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["openssh", "ubuntu"]
published: true
---
Ubuntu Server 24.04をインストールした後のOpenSSHの初期設定についてまとめておきます。

# 環境
こちらの記事 [Ubuntu Server 24.04 を VirtualBox にインストール](https://zenn.dev/makaaso/articles/install-ubuntu-2404-virtualbox) の環境が前提
- Chip: Apple M2 Max
- macOS: Sequoia 15.0.1
- VirtualBox: 7.1.4
- Ubuntu: Ubuntu Server 24.04

# 設定コマンド
## パスワード認証の無効化
```
sudo vi /etc/ssh/sshd_config.d/50-cloud-init.conf
----------
PasswordAuthentication no
----------
```

## SSHパラメータ修正
```
sudo vi /etc/ssh/sshd_config
----------
PermitRootLogin no
PermitEmptyPasswords no
MaxAuthTries 6
----------
```

## SSHサービス再起動
```
sudo service ssh restart
```

## SSH鍵生成
```
ssh-keygen
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
```

## 接続元から接続確認
```
ssh -i ~/.ssh/id_ed25519 -p 2222 ubuntu@localhost
```
※本環境はVirtualBoxのポートフォワーディング設定で、ホストOSのポート2222をゲストOSのポート22にフォワーディングしているため、コマンドでポート2222を指定しています。

# 初期設定パラメータ
|SSHパラメータ|説明|
|:-----------|:------------|
|PasswordAuthentication|パスワード認証の有効/無効|
|PermitRootLogin|rootログインの許可/禁止|
|PermitEmptyPasswords|空パスワードの有効/無効|
|MaxAuthTries|認証を試みることのできる最大回数|

# 最後に
- とりあえずの初期設定はここまでで適宜調整していこうと思います。本記事がどなたかのお役に立てば幸いです。

# 参考
- [Ubuntu Server 24.04 LTS インストール後の初期設定1 SSH メモ](https://note.com/wgetstart/n/nf11961c60910)
- [ご存じですか? sshd_config の PermintRootLogin の各種パラメータについて](https://qiita.com/ine1127/items/b50b9a8f831736cf14ea)
- [セキュアなSSHサーバの設定](https://qiita.com/comefigo/items/092137ac40f319cb14fa)
- [今さらながらのSSH纏め](https://qiita.com/leomaro7/items/b0f0babc3b68dd8be982)

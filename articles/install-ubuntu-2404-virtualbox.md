---
title: "Ubuntu Server 24.04 を VirtualBox にインストール" #title
emoji: "🦛" #icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["virtualbox", "apple silicon", "ubuntu", "arm"]
published: true
---
VirtualBox7.1がリリースされて、ようやくApple SiliconのPCでもVirtualBoxが利用できるようになりましたので、早速 `Ubuntu Server 24.04` をインストールしてみました。
こちら [「to skip startup.nsh or any other key to continue.」 の対応方法](https://zenn.dev/makaaso/articles/to-skip-startup-nsh) の記事のようにamdとarmを間違えないようお気をつけください。

# 環境
- Chip: Apple M2 Max
- macOS: Sequoia 15.0.1
- VirtualBox: 7.1.4
- Ubuntu: Ubuntu Server 24.04

# インストール手順
## ISOイメージ準備
まずはarm版をダウンロード [Ubuntu Server for ARM](https://ubuntu.com/download/server/arm)。 
![](https://storage.googleapis.com/zenn-user-upload/67c5a405d539-20241111.png)

## VirtualBoxのVM作成
新規を選択。
![](https://storage.googleapis.com/zenn-user-upload/6023e54fb196-20241111.png)

名前とオペレーティングシステム
- ISOイメージ：先ほどダウンロードしたISOイメージを選択
- 自動インストールをスキップ：チェック

![](https://storage.googleapis.com/zenn-user-upload/7a166bbf1ceb-20241111.png)

自動インストール
- 指定なし

![](https://storage.googleapis.com/zenn-user-upload/dbef0980b798-20241111.png)

ハードウェア
- 要件に合わせて設定

![](https://storage.googleapis.com/zenn-user-upload/3ab114b6e9e7-20241111.png)

ハードディスク
- 要件に合わせて設定

![](https://storage.googleapis.com/zenn-user-upload/431923e67fda-20241111.png)

`完了`を押すと画面が戻るので`起動`を選択。

![](https://storage.googleapis.com/zenn-user-upload/3bb346e965a0-20241111.png)

## Ubuntu Server 24.04 インストール
`Try or Install Ubuntu Server`を選択。
![](https://storage.googleapis.com/zenn-user-upload/777caa582296-20241111.png)

起動画面に変わるのでしばらく待機。
![](https://storage.googleapis.com/zenn-user-upload/e79e0ecc1118-20241111.png)

設定する言語を選択。
![](https://storage.googleapis.com/zenn-user-upload/b001103dbad3-20241111.png)

最新のバージョンにアップデートするかの確認。今回は`Continue without updating`を選択。
![](https://storage.googleapis.com/zenn-user-upload/db927a155320-20241111.png)

キーボードのレイアウトを選択。
![](https://storage.googleapis.com/zenn-user-upload/723d2441fec1-20241111.png)

インストールタイプを選択。
![](https://storage.googleapis.com/zenn-user-upload/dea77576f485-20241111.png)

ネットワーク設定の確認。今回はデフォルトのまま。
![](https://storage.googleapis.com/zenn-user-upload/0f41aebfeb60-20241111.png)

プロキシアドレスの確認。今回はデフォルトのまま。
![](https://storage.googleapis.com/zenn-user-upload/e6b6056b41b4-20241111.png)

Ubuntuパッケージのミラーサイトを設定。ここでは[日本国内のダウンロードサイト](https://www.ubuntulinux.jp/ubuntu/mirrors#archivemirror-ja)からメンテナンスを`Japanese Team`が担当している富山大学のサイトを登録。
![](https://storage.googleapis.com/zenn-user-upload/1e729ad4346b-20241111.png)

ストレージの設定。構成はデフォルトのまま。ここではDBも運用する想定のため暗号化を設定。
![](https://storage.googleapis.com/zenn-user-upload/55c27150373c-20241111.png)

ストレージ設定の確認。問題なければ`Done`を選択。
![](https://storage.googleapis.com/zenn-user-upload/5211204b166f-20241111.png)

Diskフォーマットの確認。問題なければ`Countinue`を選択。
![](https://storage.googleapis.com/zenn-user-upload/3ba86f66cb48-20241111.png)

プロファイル設定。ユーザ名・サーバー名などほ入力。
![](https://storage.googleapis.com/zenn-user-upload/ea1c8121f207-20241111.png)

[Ubuntu Pro](https://ubuntu.com/pro)にアップグレードするかの確認。ここではしないのスキップを選択。
![](https://storage.googleapis.com/zenn-user-upload/9383da8d8aac-20241111.png)

記事執筆時点で、VirtualBoxのGuest Additionsによるクリップボード共有が成功していないため、OpenSSH serverをインストール。
![](https://storage.googleapis.com/zenn-user-upload/36e363b27959-20241111.png)

必要なパッケージをインストール。ここでは何もインストールしないで`Done`を選択。
![](https://storage.googleapis.com/zenn-user-upload/4b1fd17263b2-20241111.png)

インストール開始。
![](https://storage.googleapis.com/zenn-user-upload/980189718780-20241111.png)

再起動。
![](https://storage.googleapis.com/zenn-user-upload/cce0b2869dd7-20241111.png)

エンターキーを押す。
![](https://storage.googleapis.com/zenn-user-upload/e86d659d16df-20241111.png)

サーバ起動中。
![](https://storage.googleapis.com/zenn-user-upload/2d00138a9d3b-20241111.png)

起動完了。
![](https://storage.googleapis.com/zenn-user-upload/c343212203e8-20241111.png)

## VirtualBoxのポートフォワーディング設定
ここではSSHで接続できるように設定していきます。

VMを選択して`設定`を選択。
![](https://storage.googleapis.com/zenn-user-upload/6e2e21410b57-20241111.png)

`ネットワーク`の`ポートフォワーディング`を選択。
![](https://storage.googleapis.com/zenn-user-upload/e15dade26a9e-20241111.png)

ホストポート`2222`をゲストポート`22`にフォワーディングするよう登録。
![](https://storage.googleapis.com/zenn-user-upload/4fe8d8163d06-20241111.png)

VMの設定変更を保存。
![](https://storage.googleapis.com/zenn-user-upload/2e8931d14a70-20241111.png)

ターミナルで接続できることを確認。
```
% ssh -p 2222 ubuntu@localhost
The authenticity of host '[localhost]:2222 ([127.0.0.1]:2222)' can't be established.
ED25519 key fingerprint is SHA256:YYjmbsxcdOhVxrxXj1NrrHzbf34jP02TrwGcXtmFAW4.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[localhost]:2222' (ED25519) to the list of known hosts.
ubuntu@localhost's password:
Welcome to Ubuntu 24.04.1 LTS (GNU/Linux 6.8.0-48-generic aarch64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Mon Nov 11 02:44:06 AM UTC 2024

  System load:             0.0
  Usage of /:              40.1% of 10.69GB
  Memory usage:            8%
  Swap usage:              0%
  Processes:               97
  Users logged in:         0
  IPv4 address for enp0s8: 10.0.2.15
  IPv6 address for enp0s8: fd00::a00:27ff:fe75:9a62

Expanded Security Maintenance for Applications is not enabled.

45 updates can be applied immediately.
To see these additional updates run: apt list --upgradable

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

# 最後に
- 執筆時点でVirtualBoxのGuest Additionsのクリップボード共有設定が成功しないので調査中となります。成功したら共有させて頂きます。

# 参考
- [macOS（Apple シリコン）でVirtualBoxを使用する方法](https://qiita.com/Brutus/items/fc58e00738a2556020ed)
- [VirtualboxのゲストOSにホストOSから（SSH）接続する](https://qiita.com/zaburo/items/963a436d8a247eb6dbc3)
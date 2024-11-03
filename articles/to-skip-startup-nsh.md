---
title: "「to skip startup.nsh or any other key to continue.」 の対応方法" #title
emoji: "🦛" #icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["virtualbox", "apple silicon", "ubuntu", "arm", "uefi"]
published: true
---
VirtualBox7.1がリリースされて、ようやくApple SiliconのPCでもVirtualBoxが利用できるようになりました。
早速 `Ubuntu Server 24.04` をインストールしてみると `UEFI Interactive Shell v2.2` 起動して以下のメッセージが表示されてインストールが開始されませんでした。
```
 Press ESC in 1 seconds to skip startup.nsh or any other key to continue.` と表示されて、インストールが始まりませんでした。
```

# 環境
- Chip: Apple M2 Max
- macOS: Sequoia 15.0.1
- VirtualBox: 7.1.4
- Ubuntu: Ubuntu Server 24.04

# 失敗までの道のり
[Canonical Ubuntu](https://jp.ubuntu.com/download) からダウンロードしたISOイメージを選択 `自動インストールをスキップ` が有効にならないけど一旦進めてみる。
![](https://storage.googleapis.com/zenn-user-upload/110f8a39202a-20241102.png)

起動してみる。
![](https://storage.googleapis.com/zenn-user-upload/a7695c6d3a96-20241102.png)

UEFI Interactive Shellが起動されてしまう。。。
![](https://storage.googleapis.com/zenn-user-upload/4476c31278f7-20241102.png)

# 原因
なんと `arm` と `amd` を間違えていた！

# 成功までの道のり
まずはarm版をダウンロード [Ubuntu Server for ARM](https://ubuntu.com/download/server/arm)。 arm版のISOイメージを利用すると `自動インストールをスキップ` のチェックボックスが有効になった。
![](https://storage.googleapis.com/zenn-user-upload/42b08721153b-20241103.png)

ユーザー情報などを入力して起動。
![](https://storage.googleapis.com/zenn-user-upload/469bf77405a3-20241103.png)

Ubuntuのインストール画面になったので、このままインストールを進める。
![](https://storage.googleapis.com/zenn-user-upload/fd531e83ad96-20241103.png)

# 最後に
armとamdは文字は少し似てますが全然別物なので気をつけましょう。

# 参考
- [macOS（Apple シリコン）でVirtualBoxを使用する方法](https://qiita.com/Brutus/items/fc58e00738a2556020ed)
- [M1MacBookのUTMにUbuntuServerをインストールする際、つまづいた事柄](https://note.com/kaitokei/n/nf2b6e34de8b1)

---
title: "VSCodeのZenn Editorでプレビューが表示されない時の対応方法" # title
emoji: "🦛" # icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vscode", "zenn"]
published: true
---
VSCodeの拡張機能 `Zenn Editor` でプレビューが表示されなかった時の対応についてメモです。

# 環境
- Chip: Apple M2 Max
- macOS: Sequoia 15.0.1
- VSCode: 1.95.3

# 事象
`Zenn Editor` でプレビュー表示すると
![](https://storage.googleapis.com/zenn-user-upload/259bf44ba91e-20241129.png)

以下のメッセージが出力される。
![](https://storage.googleapis.com/zenn-user-upload/c9d99877a4c1-20241129.png)

# 原因
ファイル名 `setup-ubuntu-server-ssm-24.04.md` でファイル名にコロン(.)が含まれていたためファイル読み込みに失敗。

# 対応
ファイル名のコロン(.)を削除。

# 最後に
この記事がどなたかのお役に立てれば幸いです。

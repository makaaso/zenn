---
title: "CursorのRCE脆弱性と対策方法" #title
emoji: "🦛" #icon
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Cursor", "Security"]
published: true
---
CursorにRCE（Remote Code Execution：リモートコード実行）の脆弱性が発見されました。この記事では、脆弱性の詳細と対策方法をまとめます。

**⚠️ 重要：この脆弱性により、悪意のあるプロジェクトを開くだけで任意のコードが実行される可能性があります。**

# 環境
- Chip: Apple M2 Max
- macOS: Sequoia 15.0.1

# 脆弱性の詳細

## 概要
CursorはVS CodeベースのAI搭載コードエディタです。プロジェクトを開いた際に、そのプロジェクトに設定された処理を自動実行する機能があります。

## 問題となる設定
設定項目 `security.workspace.trust.enabled` が **false** に設定されていると、以下の問題が発生します：

- **動作**：ワークスペースの信頼確認を行わない
- **結果**：自動実行前の確認ダイアログが表示されない
- **注意**：設定名から誤解しやすいですが、`false` は「信頼しない」ではなく「確認しない」という意味です

## 攻撃シナリオ
1. 攻撃者が悪意のあるコードを含むプロジェクト（リポジトリ）を作成
2. ユーザーがそのプロジェクトをCursorで開く
3. 確認ダイアログなしに任意のコードが自動実行される
4. **結果：RCE（リモートコード実行）が成立**

# 対策方法

以下の手順で設定を変更し、脆弱性を修正できます。

## 手順

### 1. 設定画面を開く
右上の `歯車マーク` をクリックします。
![](https://storage.googleapis.com/zenn-user-upload/be93f0aff2b4-20250926.png)

### 2. エディター設定を開く
`Editor Settings` の `Open` をクリックします。
![](https://storage.googleapis.com/zenn-user-upload/fa775a95de60-20250926.png)

### 3. セキュリティ設定を有効化
1. 検索ボックスに `security.workspace.trust.enabled` と入力
2. 該当項目 `VS Code 内でワークスペースの信頼を有効にするかどうかを制御します` にチェックを入れる
![](https://storage.googleapis.com/zenn-user-upload/15e4aba9b635-20250926.png)

### 4. Cursorを再起動
設定を反映させるため、Cursorを再起動します。

## 設定後の動作
この設定を有効にすると、新しいプロジェクトを開く際に信頼確認のダイアログが表示されるようになり、ユーザーが明示的に許可した場合のみコードが実行されます。

# まとめ

AIを活用したコードエディタは非常に便利ですが、セキュリティ設定を適切に管理することが重要です。今回のような脆弱性を防ぐため、以下の点を心がけましょう：

- **信頼できないプロジェクトを開く際は注意する**
- **セキュリティ関連の設定を定期的に確認する**
- **自動実行機能の動作を理解して利用する**

## 追加の対策

- 不明なソースからのプロジェクトは、仮想環境やサンドボックスで開くことを検討
- 定期的にCursorのアップデートを確認し、セキュリティパッチを適用

# 参考資料
- [CursorのRCE脆弱性について - Constella Security](https://constella-sec.jp/blog/hobokomo-securitynews/tips-1056/)
- [VS Code Workspace Trust - Microsoft Docs](https://code.visualstudio.com/docs/editor/workspace-trust)

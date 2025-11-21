# クイックスタートガイド

5分でClaudeCode Command Templateを使い始めるためのガイドです。

## 📋 前提条件

- [ClaudeCode](https://claude.com/claude-code) がインストール済み
- [GitHub CLI](https://cli.github.com/) がインストール済み（GitHub機能を使う場合）
- Git リポジトリが初期化済み

## 🚀 セットアップ（2分）

### 1. テンプレートのコピー

```bash
# このリポジトリをクローンまたはダウンロード
git clone https://github.com/m96-chan/ClaudCodeTemplate.git

# .claudeディレクトリを自分のプロジェクトにコピー
cp -r ClaudCodeTemplate/.claude /path/to/your/project/

# プロジェクトディレクトリに移動
cd /path/to/your/project
```

### 2. GitHub CLIの認証（GitHub機能を使う場合）

```bash
gh auth login
```

ブラウザで認証を完了してください。

## ✨ 最初のコマンドを実行（3分）

### シナリオ: 新規プロジェクトの立ち上げ

#### ステップ1: GitHubの初期設定

```bash
# ClaudeCodeを起動
claude

# 以下のコマンドを実行
/gh-template
```

これにより以下が自動作成されます:
- ✅ PR/Issueテンプレート
- ✅ ラベル（優先度・タイプ・ステータス）
- ✅ マイルストーン（v0.1.0, v1.0.0, v2.0.0）

**所要時間**: 約1分

#### ステップ2: プロジェクトボードの作成（オプション）

```bash
/gh-project
```

カンバンボード（Backlog → Todo → In Progress → In Review → Done）が作成されます。

**所要時間**: 約30秒

#### ステップ3: Issueの一括作成

README.mdに主要機能を記載してから:

```bash
/gh-issues-from-readme
```

README.mdの内容を分析し、以下を自動で行います:
- 📝 機能ごとにIssueを作成
- 🏷️ 適切なラベルを付与
- 🎯 マイルストーンを設定
- ✅ ユーザー確認後に一括作成

**所要時間**: 約1分

#### ステップ4: 開発開始

```bash
/gh-issue 1
```

最初のIssueから開発を開始します。以下が自動で実行されます:
- ブランチ作成
- TDD での実装
- テスト・Lint実行
- コミット作成
- PR作成

**所要時間**: 実装内容による

## 💡 よく使うコマンド

### コード品質の改善

```bash
# コードの改善提案を受ける
/improve

# テストコードを生成
/test

# Lintエラーを自動修正
/fix-lint

# リファクタリング実行
/refactor

# ドキュメント生成
/docs
```

### GitHub連携

```bash
# バグ報告のIssue作成
/gh-bug-create ログイン時にエラーが発生する

# 機能要求のIssue作成
/gh-feature-create ユーザープロフィール編集機能

# PRレビュー
/pr-review 42
```

### プロジェクト理解

```bash
# プロジェクトの説明を生成
/explain
```

## 📚 次のステップ

### 詳細を学ぶ

- [README.md](README.md) - 詳細な使い方
- [COMMANDS.md](how_to_vibe_coding/COMMANDS.md) - 全コマンドリファレンス
- [ARCHITECTURE.md](how_to_vibe_coding/ARCHITECTURE.md) - システム設計
- [CONTRIBUTING.md](CONTRIBUTING.md) - 貢献方法

### カスタマイズ

コマンドは `.claude/commands/` ディレクトリのMarkdownファイルで定義されています。
プロジェクトに合わせて自由に編集できます。

```bash
# 例: gh-templateコマンドをカスタマイズ
code .claude/commands/gh-template.md
```

### コミュニティ

- [GitHub Issues](https://github.com/m96-chan/ClaudCodeTemplate/issues) - バグ報告・機能提案
- [GitHub Discussions](https://github.com/m96-chan/ClaudCodeTemplate/discussions) - 質問・相談

## 🎯 実践例

### 例1: Webアプリケーションの開発

```bash
# 1. GitHubセットアップ
/gh-template
/gh-project

# 2. README.mdに機能を記載
# - ユーザー認証
# - ダッシュボード
# - プロフィール管理

# 3. Issueを一括作成
/gh-issues-from-readme

# 4. 開発開始
/gh-issue 1

# 5. コード品質チェック
/test
/fix-lint
/docs
```

### 例2: ライブラリの開発

```bash
# 1. プロジェクト説明を生成
/explain

# 2. テストコード生成
/test

# 3. ドキュメント生成
/docs

# 4. PRレビュー
/pr-review 1
```

## ❓ トラブルシューティング

### コマンドが見つからない

```bash
# .claudeディレクトリが正しい場所にあるか確認
ls -la .claude/commands/
```

### GitHub CLI認証エラー

```bash
# 認証状態を確認
gh auth status

# 再認証
gh auth login
```

### 権限エラー

`.claude/settings.local.json` を確認:

```json
{
  "permissions": {
    "allow": [
      "Bash(gh:*)",
      "Bash(git:*)"
    ]
  }
}
```

## 🎉 完了!

これでClaudeCode Command Templateの基本的な使い方がわかりました。

質問や問題があれば、[Issue](https://github.com/m96-chan/ClaudCodeTemplate/issues)で報告してください。

Happy Coding! 🚀

# ClaudeCode Command Template

ClaudeCodeのカスタムコマンド(スラッシュコマンド)のテンプレート集です。このテンプレートを使用することで、開発効率を向上させ、GitHub Issueとの連携やTDD開発をスムーズに行えます。

## 概要

このプロジェクトは、ClaudeCodeで使用できる便利なカスタムコマンドのテンプレートを提供します。プロジェクトのテンプレートとして使用することで、すぐに高度な開発ワークフローを導入できます。

## 利用可能なコマンド

### GitHub Issue関連

- `/gh-issue <issue番号>` - GitHub Issueを元にTDDでタスクを遂行し、PRを作成する
- `/gh-bug-create <バグ内容>` - バグ報告のGitHub Issueを作成する
- `/gh-feature-create <機能内容>` - 機能要求のGitHub Issueを作成する
- `/gh-template` - .github/ディレクトリにPR/Issueテンプレートを作成する

### コード品質関連

- `/explain` - プロジェクトの主な機能、技術スタック、ディレクトリ構成を説明する
- `/improve` - コードの改善提案(可読性、パフォーマンス、エラーハンドリング)を提示する
- `/test` - 現在の実装に対するテストコードを生成する

## セットアップ

### 1. テンプレートの使用

このリポジトリをテンプレートとして新しいプロジェクトを作成するか、既存のプロジェクトに`.claude`ディレクトリをコピーしてください。

```bash
# 既存のプロジェクトに適用する場合
cp -r .claude /path/to/your/project/
```

### 2. GitHub CLIの設定

GitHub関連のコマンドを使用する場合は、GitHub CLIの認証が必要です。

```bash
gh auth login
```

### 3. GitHubテンプレートの作成

初回は以下のコマンドでGitHubテンプレートを作成してください。

```
/gh-template
```

これにより以下のファイルが作成されます:
- `.github/PULL_REQUEST_TEMPLATE.md`
- `.github/ISSUE_TEMPLATE/bug_report.md`
- `.github/ISSUE_TEMPLATE/feature_request.md`

## 使い方

### 基本的なワークフロー

1. 機能要求を作成:
   ```
   /gh-feature-create ユーザー認証機能の追加
   ```

2. Issueを元に開発:
   ```
   /gh-issue 123
   ```
   このコマンドは以下を自動で行います:
   - Issueの内容を理解
   - mainブランチを最新に更新
   - 新しいブランチを作成
   - TDDでタスクを実施
   - テスト・Lintの実行
   - コミット作成
   - PRの作成

3. コード改善の提案を受ける:
   ```
   /improve
   ```

4. テストコードを生成:
   ```
   /test
   ```

### バグ修正のワークフロー

1. バグ報告を作成:
   ```
   /gh-bug-create ログイン時に500エラーが発生する
   ```

2. Issueを元に修正:
   ```
   /gh-issue 124
   ```

## カスタマイズ

各コマンドは`.claude/commands/`ディレクトリ内のMarkdownファイルで定義されています。プロジェクトのニーズに合わせて自由に編集できます。

詳しいカスタマイズ方法は[how_to_vibe_coding/COMMANDS.md](how_to_vibe_coding/COMMANDS.md)を参照してください。

## ディレクトリ構造

```
.
├── .claude/
│   ├── commands/          # カスタムコマンド定義
│   │   ├── explain.md
│   │   ├── improve.md
│   │   ├── test.md
│   │   ├── gh-issue.md
│   │   ├── gh-bug-create.md
│   │   ├── gh-feature-create.md
│   │   └── gh-template.md
│   └── settings.local.json
├── .github/
│   └── workflows/         # GitHub Actions用
├── how_to_vibe_coding/    # ドキュメント(テンプレート使用時は削除可能)
│   └── COMMANDS.md        # コマンドマニュアル
├── LICENSE
└── README.md
```

## 注意事項

- このテンプレートを他のプロジェクトで使用する際は、`how_to_vibe_coding/`ディレクトリは削除して構いません
- `.claude/`ディレクトリのみをコピーすれば、すべてのコマンドが利用可能です
- GitHub関連のコマンドを使用するには、リポジトリがGitHubに存在している必要があります

## ライセンス

MIT License - 詳細は[LICENSE](LICENSE)ファイルを参照してください

## 貢献

このテンプレートへの改善提案やバグ報告は、Issueまたはプルリクエストでお願いします。

## 参考リンク

- [ClaudeCode公式ドキュメント](https://github.com/anthropics/claude-code)
- [GitHub CLI](https://cli.github.com/)

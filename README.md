# ClaudeCode Command Template

ClaudeCodeのカスタムコマンド(スラッシュコマンド)のテンプレート集です。このテンプレートを使用することで、開発効率を向上させ、GitHub Issueとの連携やTDD開発をスムーズに行えます。

## 概要

このプロジェクトは、ClaudeCodeで使用できる便利なカスタムコマンドのテンプレートを提供します。プロジェクトのテンプレートとして使用することで、すぐに高度な開発ワークフローを導入できます。

## 利用可能なコマンド

### GitHub Issue関連

- `/gh-template` - GitHub テンプレート、ラベル、マイルストーンを作成する
- `/gh-project` - GitHub Projectsを作成し、Issueと連携する
- `/gh-issues-from-readme` - README.mdから必要なIssueを一括作成する
- `/gh-issue <issue番号>` - GitHub Issueを元にTDDでタスクを遂行し、PRを作成する
- `/gh-bug-create <バグ内容>` - バグ報告のGitHub Issueを作成する
- `/gh-feature-create <機能内容>` - 機能要求のGitHub Issueを作成する

### コード品質関連

- `/explain` - プロジェクトの主な機能、技術スタック、ディレクトリ構成を説明する
- `/improve` - コードの改善提案(可読性、パフォーマンス、エラーハンドリング)を提示する
- `/test` - 現在の実装に対するテストコードを生成する
- `/refactor` - コードの改善提案に基づいて即座にリファクタリングを実施する
- `/fix-lint` - プロジェクトのLintエラーを検出し、自動的に修正する
- `/docs` - 現在のコードを解析し、包括的なドキュメントを生成する
- `/pr-review <PR番号>` - GitHub Pull Requestの内容をレビューし、改善提案を提示する

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

### 3. GitHubの初期設定

初回は以下のコマンドでGitHub関連の初期設定を行ってください。

```
/gh-template
```

これにより以下が作成されます:
- **テンプレートファイル**:
  - `.github/PULL_REQUEST_TEMPLATE.md`
  - `.github/ISSUE_TEMPLATE/bug_report.md`
  - `.github/ISSUE_TEMPLATE/feature_request.md`
- **ラベル**: 優先度、タイプ、ステータスラベル（README.mdから追加抽出）
- **マイルストーン**: v0.1.0, v1.0.0, v2.0.0（README.mdから追加抽出）

さらに、プロジェクト管理を強化する場合:

```
/gh-project
```

これによりGitHub Projectsが作成されます:
- カンバンボード（Backlog/Todo/In Progress/In Review/Done）
- カスタムフィールド（優先度、見積もり、担当領域）
- 既存Issueとの連携（オプション）

## 使い方

### プロジェクト立ち上げ時のワークフロー

1. GitHubの初期設定:
   ```
   /gh-template
   ```
   → テンプレート、ラベル、マイルストーンを一括作成

2. GitHub Projectsの作成（オプション）:
   ```
   /gh-project
   ```
   → カンバンボードとカスタムフィールドを設定

3. README.mdから一括でIssueを作成:
   ```
   /gh-issues-from-readme
   ```
   このコマンドは以下を自動で行います:
   - README.mdの内容を分析
   - 必要な機能をIssueとして抽出
   - 優先順位を付けて分類
   - 適切なラベルとマイルストーンを自動設定
   - ユーザー確認後、一括作成
   - 推奨される開始順序を提案

4. 最初のIssueから開発を開始:
   ```
   /gh-issue 1
   ```

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

5. Lintエラーを自動修正:
   ```
   /fix-lint
   ```

6. コードをリファクタリング:
   ```
   /refactor
   ```

7. ドキュメントを生成:
   ```
   /docs
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

### PRレビューのワークフロー

1. Pull Requestをレビュー:
   ```
   /pr-review 125
   ```
   このコマンドは以下を自動で行います:
   - PRの詳細情報を取得
   - 変更内容の差分を確認
   - コード品質・機能性・テスト・セキュリティ・パフォーマンスの観点でレビュー
   - 構造化されたレビュー結果を表示

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
│   │   ├── refactor.md
│   │   ├── fix-lint.md
│   │   ├── docs.md
│   │   ├── gh-template.md
│   │   ├── gh-project.md
│   │   ├── gh-issues-from-readme.md
│   │   ├── gh-issue.md
│   │   ├── gh-bug-create.md
│   │   ├── gh-feature-create.md
│   │   └── pr-review.md
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

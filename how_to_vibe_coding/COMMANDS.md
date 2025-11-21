# コマンドマニュアル

このドキュメントでは、ClaudeCode Command Templateで提供されるすべてのカスタムコマンドの詳細な使用方法を説明します。

## 目次

- [GitHub Issue関連コマンド](#github-issue関連コマンド)
  - [/gh-issue](#gh-issue)
  - [/gh-bug-create](#gh-bug-create)
  - [/gh-feature-create](#gh-feature-create)
  - [/gh-template](#gh-template)
- [コード品質関連コマンド](#コード品質関連コマンド)
  - [/explain](#explain)
  - [/improve](#improve)
  - [/test](#test)

---

## GitHub Issue関連コマンド

### /gh-issue

**説明**: GitHub Issueを元にTDDでタスクを遂行し、PRを作成する

**構文**:
```
/gh-issue <issue番号>
```

**引数**:
- `<issue番号>`: 処理するGitHub IssueのID(例: 123)

**動作フロー**:
1. `gh issue view`でIssueの内容を取得
2. Issueの内容を理解・分析
3. mainブランチにチェックアウトし、`git pull`で最新状態を取得
4. Issue内容に基づいた適切な名前でブランチを作成・チェックアウト
5. TDD(テスト駆動開発)に基づいてタスクを遂行
6. テストとLintを実行し、すべてのテストが通ることを確認
7. 適切な粒度でコミットを作成
8. `.github/PULL_REQUEST_TEMPLATE.md`に従ってPRを作成
9. PR descriptionに`Closes #<issue番号>`を記載

**使用例**:
```
/gh-issue 42
```

**前提条件**:
- GitHub CLIが認証済み(`gh auth status`で確認)
- リポジトリがGitHubに存在する
- `.github/PULL_REQUEST_TEMPLATE.md`が存在する

**注意事項**:
- PRのdescriptionテンプレート内のコメントアウト部分は自動的に削除されます
- テストが失敗する場合は、PRは作成されません

---

### /gh-bug-create

**説明**: バグ報告のGitHub Issueを作成する

**構文**:
```
/gh-bug-create <バグの内容>
```

**引数**:
- `<バグの内容>`: バグの概要説明(詳細は対話形式で入力)

**動作フロー**:
1. 引数の内容を読み解き、バグの詳細を分析
2. ユーザーと対話しながら設計を確認
3. `.github/ISSUE_TEMPLATE/bug_report.md`を元にTDDができる文書を作成
4. `gh issue create`でIssueを作成
5. 作成過程で使用した一時ファイルをクリア

**使用例**:
```
/gh-bug-create ログイン時に500エラーが発生する
```

**前提条件**:
- GitHub CLIが認証済み
- `.github/ISSUE_TEMPLATE/bug_report.md`が存在する

**注意事項**:
- TDD(テスト駆動開発)で対応できるように、バグの再現手順が明確に記載されます
- ユーザーの確認を得てからIssueを作成します

---

### /gh-feature-create

**説明**: 機能要求のGitHub Issueを作成する

**構文**:
```
/gh-feature-create <機能の内容>
```

**引数**:
- `<機能の内容>`: 機能の概要説明(詳細は対話形式で入力)

**動作フロー**:
1. 引数の内容を読み解き、機能の設計を行う
2. ユーザーと対話しながら設計を確認
3. `.github/ISSUE_TEMPLATE/feature_request.md`を元にTDDができる文書を作成
4. `gh issue create`でIssueを作成
5. 作成過程で使用した一時ファイルをクリア

**使用例**:
```
/gh-feature-create ユーザープロフィール編集機能の追加
```

**前提条件**:
- GitHub CLIが認証済み
- `.github/ISSUE_TEMPLATE/feature_request.md`が存在する

**注意事項**:
- TDD(テスト駆動開発)で実装できるように、受け入れ基準が明確に記載されます
- ユーザーの設計承認後にIssueを作成します

---

### /gh-template

**説明**: .github/ディレクトリにPR/Issueテンプレート一式を作成する

**構文**:
```
/gh-template
```

**引数**: なし

**動作フロー**:
1. タスク内容(GitHubテンプレートファイルの作成)を理解
2. mainブランチにチェックアウトし、`git pull`で最新状態を取得
3. 適切な名前でブランチを作成・チェックアウト(例: feat/add-github-templates)
4. 以下のファイルを`.github/`ディレクトリ内に作成:
   - `.github/PULL_REQUEST_TEMPLATE.md`
   - `.github/ISSUE_TEMPLATE/bug_report.md`
   - `.github/ISSUE_TEMPLATE/feature_request.md`
5. 作成したファイルを適切な粒度でコミット
6. PRを作成(テスト・Lintは不要)

**使用例**:
```
/gh-template
```

**前提条件**:
- GitHub CLIが認証済み
- リポジトリがGitHubに存在する

**注意事項**:
- 作成されるテンプレートは、TDDができるような内容になっています
- PRのdescriptionは`.github/PULL_REQUEST_TEMPLATE.md`に従います

---

## コード品質関連コマンド

### /explain

**説明**: プロジェクトについて初心者向けに説明する

**構文**:
```
/explain
```

**引数**: なし

**動作内容**:
プロジェクトについて以下の観点で説明します:
- 主な機能
- 使用している技術
- ディレクトリ構成
- 重要なファイル

**使用例**:
```
/explain
```

**前提条件**: なし

**注意事項**:
- 初めてプロジェクトを見る人でもわかるように、簡潔に説明されます
- プロジェクトのオンボーディングに最適です

---

### /improve

**説明**: 現在のコードに対する改善提案を提示する

**構文**:
```
/improve
```

**引数**: なし

**動作内容**:
現在のコードを確認し、以下の観点で改善提案を行います:
1. コードの可読性
2. パフォーマンス
3. エラーハンドリング

改善案は具体的なコード例と共に提示され、実装の優先度(高/中/低)も付けられます。

**使用例**:
```
/improve
```

**前提条件**: なし

**注意事項**:
- ファイルやディレクトリを指定したい場合は、事前に該当ファイルを開いておくとより的確な提案が得られます
- 提案は優先度付きで提示されるため、重要度に応じて実装できます

---

### /test

**説明**: 現在の実装に対するテストコードを生成する

**構文**:
```
/test
```

**引数**: なし

**動作内容**:
現在の実装に対して、以下のケースを含むテストコードを生成します:
- 正常系のテスト
- 異常系のテスト
- エッジケースのテスト

テストフレームワークはプロジェクトで使用しているものを自動で選択し、各テストケースにはコメントで説明が追加されます。

**使用例**:
```
/test
```

**前提条件**: なし

**注意事項**:
- すでにテストが存在する場合は、そのテストは作成されません
- 生成されるテストは、プロジェクトの既存のテストスタイルに合わせられます
- テストフレームワークが設定されていない場合は、一般的なフレームワークが提案されます

---

## カスタマイズガイド

### コマンドの編集方法

各コマンドは`.claude/commands/`ディレクトリ内のMarkdownファイルで定義されています。

**ファイル構造**:
```markdown
---
description: コマンドの説明
---

コマンドの実行内容をここに記述します。
$ARGUMENTSでユーザーからの引数を参照できます。
```

**例**: カスタムコマンドの作成
```markdown
---
description: データベースのマイグレーションを実行する
---

以下の手順でマイグレーションを実行してください:

1. データベース接続を確認する
2. マイグレーションファイルを確認する
3. $ARGUMENTSで指定されたマイグレーションを実行する
4. 実行結果を報告する
```

### 新しいコマンドの追加

1. `.claude/commands/`に新しい`.md`ファイルを作成
2. ファイル名がコマンド名になります(例: `deploy.md` → `/deploy`)
3. frontmatterで`description`を設定
4. コマンドの動作を記述

### コマンドのベストプラクティス

- **明確な手順**: コマンドの動作を番号付きリストで明確に記述する
- **引数の活用**: `$ARGUMENTS`を使ってユーザー入力を活用する
- **確認ステップ**: 重要な操作の前にユーザー確認を入れる
- **クリーンアップ**: 一時ファイルは必ずクリアする
- **エラーハンドリング**: 想定されるエラーケースを考慮する

---

## トラブルシューティング

### GitHub CLIの認証エラー

**エラー**: `gh: To get started with GitHub CLI, please run: gh auth login`

**解決方法**:
```bash
gh auth login
```

画面の指示に従ってGitHubアカウントと認証してください。

### テンプレートファイルが見つからない

**エラー**: `.github/PULL_REQUEST_TEMPLATE.md: No such file or directory`

**解決方法**:
```
/gh-template
```

このコマンドで必要なテンプレートを作成してください。

### Issue番号が無効

**エラー**: `could not resolve to an Issue`

**解決方法**:
- Issue番号が正しいか確認
- リポジトリが正しいか確認
- GitHub CLIの認証状態を確認(`gh auth status`)

---

## よくある質問(FAQ)

### Q1: コマンドを複数のプロジェクトで使いたい

A: `.claude`ディレクトリを各プロジェクトにコピーするだけで使用できます。

```bash
cp -r /path/to/ClaudCodeTemplate/.claude /path/to/your/project/
```

### Q2: コマンドをプロジェクト固有にカスタマイズしたい

A: `.claude/commands/`内のMarkdownファイルを直接編集してください。変更は即座に反映されます。

### Q3: GitHub以外のIssue管理ツールに対応させたい

A: コマンドファイルを編集し、`gh`コマンドの代わりに使用しているツールのCLIコマンドに置き換えてください。

### Q4: テストフレームワークが自動検出されない

A: `/test`コマンド実行後、使用したいテストフレームワークを明示的に指定してください。

---

## 参考リンク

- [ClaudeCode公式ドキュメント](https://github.com/anthropics/claude-code)
- [GitHub CLI](https://cli.github.com/)
- [GitHub Issueテンプレート](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests)
- [PRテンプレート](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

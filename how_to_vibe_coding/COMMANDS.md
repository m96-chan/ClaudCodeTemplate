# コマンドマニュアル

このドキュメントでは、ClaudeCode Command Templateで提供されるすべてのカスタムコマンドの詳細な使用方法を説明します。

## 目次

- [GitHub Issue関連コマンド](#github-issue関連コマンド)
  - [/gh-template](#gh-template)
  - [/gh-project](#gh-project)
  - [/gh-issues-from-readme](#gh-issues-from-readme)
  - [/gh-issue](#gh-issue)
  - [/gh-bug-create](#gh-bug-create)
  - [/gh-feature-create](#gh-feature-create)
- [コード品質関連コマンド](#コード品質関連コマンド)
  - [/explain](#explain)
  - [/improve](#improve)
  - [/test](#test)
  - [/refactor](#refactor)
  - [/fix-lint](#fix-lint)
  - [/docs](#docs)
  - [/pr-review](#pr-review)

---

## GitHub Issue関連コマンド

### /gh-template

**説明**: GitHub テンプレート、ラベル、マイルストーンを一括作成する

**構文**:
```
/gh-template [オプション]
```

**引数**:
- `--skip-labels`: ラベルの作成をスキップ
- `--skip-milestones`: マイルストーンの作成をスキップ
- `--templates-only`: テンプレートファイルのみ作成（ラベル・マイルストーンをスキップ）

**動作フロー**:
1. 前提条件を確認:
   - GitHub CLIが認証済み(`gh auth status`)
   - リポジトリが存在する
   - リモートリポジトリが設定されている
2. README.mdが存在する場合は読み取り、以下を分析:
   - プロジェクトの目的と概要
   - 主要な機能リスト
   - 技術スタック
   - マイルストーンになりそうなフェーズ・バージョン情報
3. ブランチを準備:
   - mainにチェックアウトし、pullで最新を取得
   - 新しいブランチを作成（例: feat/add-github-templates）
4. テンプレートファイルを作成:
   - `.github/PULL_REQUEST_TEMPLATE.md`
   - `.github/ISSUE_TEMPLATE/bug_report.md`
   - `.github/ISSUE_TEMPLATE/feature_request.md`
   - 既存ファイルがある場合は上書き確認
5. ラベルを作成:
   - **優先度**: priority:p0/p1/p2/p3
   - **タイプ**: type:feature/bug/docs/test/refactor/setup/chore
   - **ステータス**: status:todo/in-progress/review/blocked/done
   - **README.mdから抽出**: プロジェクト固有のラベル
6. マイルストーンを作成:
   - デフォルト: v0.1.0, v1.0.0, v2.0.0
   - README.mdから抽出: バージョン番号、フェーズ名
7. コミットとPRを作成
8. 作成結果を報告

**使用例**:
```
/gh-template
/gh-template --skip-labels
/gh-template --templates-only
```

**前提条件**:
- GitHub CLIが認証済み
- リポジトリがGitHubに存在する

**作成されるラベル一覧**:

**優先度（Priority）**:
- `priority:p0` - Critical（赤: #d73a4a）
- `priority:p1` - High（オレンジ: #e99695）
- `priority:p2` - Medium（黄: #fbca04）
- `priority:p3` - Low（緑: #0e8a16）

**タイプ（Type）**:
- `type:feature` - 新機能（青: #0075ca）
- `type:bug` - バグ修正（赤: #d73a4a）
- `type:docs` - ドキュメント（灰: #7057ff）
- `type:test` - テスト（緑: #0e8a16）
- `type:refactor` - リファクタリング（紫: #d876e3）
- `type:setup` - セットアップ（茶: #8b4513）
- `type:chore` - 雑務（灰: #fef2c0）

**ステータス（Status）**:
- `status:todo` - 未着手（白: #ffffff）
- `status:in-progress` - 進行中（黄: #fbca04）
- `status:review` - レビュー中（青: #0075ca）
- `status:blocked` - ブロック中（赤: #d73a4a）
- `status:done` - 完了（緑: #0e8a16）

**デフォルトマイルストーン**:
- `v0.1.0 - Initial Setup` - プロジェクトの初期セットアップ
- `v1.0.0 - MVP` - 最小機能プロダクト
- `v2.0.0 - Full Release` - 完全版リリース

**注意事項**:
- ラベルやマイルストーンが既に存在する場合はエラーを無視して継続します
- README.mdが存在しない場合はデフォルトのラベル・マイルストーンのみ作成します
- マイルストーンの期限は現在の日付から適切に計算されます
- 既存のテンプレートファイルがある場合は上書き確認が行われます

**次のステップ**:
- GitHub Projectsを作成: `/gh-project`
- Issueを一括作成: `/gh-issues-from-readme`

---

### /gh-project

**説明**: GitHub Projectsを作成し、カンバンボードとカスタムフィールドを設定する

**構文**:
```
/gh-project [オプション]
```

**引数**:
- `--name=[name]`: プロジェクト名を明示的に指定
- `--org=[organization]`: 組織レベルのプロジェクトを作成
- `--template=[kanban|roadmap|table]`: レイアウトテンプレートを指定
- `--link-issues`: 既存のIssueをすべてプロジェクトに追加
- `--with-iterations`: 2週間スプリント（イテレーション）を設定

**動作フロー**:
1. 前提条件を確認:
   - GitHub CLIが認証済み(`gh auth status`)
   - リポジトリが存在する
   - リモートリポジトリが設定されている
2. README.mdが存在する場合は読み取り、以下を分析:
   - プロジェクトの目的と概要
   - 主要な機能リスト
   - 開発フェーズやマイルストーン
3. プロジェクトボードのレイアウトを決定:
   - 推奨: カンバンボード（Backlog/Todo/In Progress/In Review/Done）
   - 代替: ロードマップビュー、テーブルビュー
4. プロジェクト名と説明を決定
5. GitHub Projectを作成（リポジトリまたは組織レベル）
6. カスタムフィールドを追加:
   - **Priority**: P0/P1/P2/P3（単一選択）
   - **Estimate**: 数値（ストーリーポイントまたは日数）
   - **Area**: Frontend/Backend/Database/etc.（単一選択）
   - **Status**: Backlog/Todo/In Progress/In Review/Done（単一選択）
   - **Iteration**: 2週間スプリント（オプション）
7. プロジェクトビューを設定:
   - カンバンボードビュー（デフォルト）
   - ロードマップビュー（オプション）
   - テーブルビュー
8. 既存Issueと連携（オプション）
9. 自動化ワークフローの推奨設定を提示
10. 作成結果を報告

**使用例**:
```
/gh-project
/gh-project --org=my-organization
/gh-project --link-issues
/gh-project --with-iterations
/gh-project --name="Q1 2024 Roadmap"
```

**前提条件**:
- GitHub CLIが認証済み
- リポジトリがGitHubに存在する
- 適切な権限（リポジトリの書き込み権限または組織の管理者権限）

**作成されるカスタムフィールド**:

**Priority（優先度）** - 単一選択:
- P0 - Critical 🔴
- P1 - High 🟠
- P2 - Medium 🟡
- P3 - Low 🟢

**Estimate（見積もり）** - 数値:
- ストーリーポイントまたは時間（単位: 日）

**Area（担当領域）** - 単一選択:
- Frontend
- Backend
- Database
- Infrastructure
- Documentation
- Testing
- (README.mdから抽出した追加領域)

**Status（ステータス）** - 単一選択:
- Backlog
- Todo
- In Progress
- In Review
- Done

**推奨される自動化設定**:
1. Issue作成時 → Todoに自動追加
2. PR作成時 → In Progressに自動移動
3. PRマージ時 → Doneに自動移動
4. Issueクローズ時 → Doneに自動移動

**注意事項**:
- GitHub Projects (Beta) は GitHub CLI で完全にサポートされていない機能があるため、一部の設定は Web UI で行う必要があります
- 既存のプロジェクトと名前が重複する場合は、ユーザーに確認が求められます
- 大量のIssue（50件以上）を連携する場合は、ユーザーに確認が求められます
- 自動化ワークフローは GitHub Projects UI で設定することが推奨されます

**次のステップ**:
- Issueを一括作成してプロジェクトに追加: `/gh-issues-from-readme`
- WebUIでプロジェクトを開く: `https://github.com/users/[owner]/projects/[number]`

---

### /gh-issues-from-readme

**説明**: README.mdからプロジェクトの内容を分析し、必要なIssueを一括作成する

**構文**:
```
/gh-issues-from-readme [オプション]
```

**引数**:
- `--dry-run`: Issueを実際に作成せず、作成予定のリストのみ表示
- `--priority=[p0|p1|p2|p3]`: 指定した優先度以上のIssueのみ作成
- `--category=[setup|feature|docs|test]`: 特定のカテゴリのIssueのみ作成
- `--milestone=[name]`: 作成するすべてのIssueに指定したマイルストーンを設定
- `--auto-approve`: ユーザー確認をスキップして自動作成（非推奨）

**動作フロー**:
1. 前提条件を確認:
   - GitHub CLIが認証済み(`gh auth status`)
   - リポジトリが存在する
   - リモートリポジトリが設定されている
   - README.mdが存在する
2. README.mdを読み取り、以下を分析:
   - プロジェクトの目的と概要
   - 主要な機能リスト
   - 技術スタック
   - セットアップ手順
   - TODOリストやRoadmap
3. プロジェクト構造を分析:
   - ディレクトリ構造
   - 既存のファイル
   - 依存関係ファイル(package.json、requirements.txt等)
4. 以下のカテゴリでIssueリストを作成:
   - **セットアップ関連** (P0): 開発環境、CI/CD、依存関係
   - **コア機能** (P1): README記載の主要機能
   - **拡張機能** (P2): 追加機能
   - **ドキュメント** (P2): API doc、コントリビューションガイド
   - **テスト・品質** (P1): ユニットテスト、統合テスト、Lint設定
5. ユーザーに確認を求める:
   - 作成予定のIssueリストを表示
   - 追加・削除・優先度変更の確認
6. 承認後、各Issueを`gh issue create`で作成:
   - 適切なラベルを付与(`priority:p0`, `type:feature`等)
   - テンプレートに従った詳細な説明
   - TDDで対応できる具体的なタスクと受け入れ基準
7. 作成結果を報告:
   - 作成されたIssueの一覧
   - 推奨される開始順序
   - 次のステップの提案

**使用例**:
```
/gh-issues-from-readme
/gh-issues-from-readme --dry-run
/gh-issues-from-readme --priority=p1
/gh-issues-from-readme --category=feature
```

**前提条件**:
- GitHub CLIが認証済み
- リポジトリがGitHubに存在する
- README.mdが存在する

**作成されるIssueの構造**:
```markdown
## 概要
[この機能/タスクの目的と概要]

## 背景
[なぜこの機能が必要か]

## タスク
- [ ] タスク1
- [ ] タスク2
- [ ] タスク3

## 受け入れ基準
- 基準1が満たされている
- 基準2が満たされている

## 技術的な考慮事項
- [使用する技術やライブラリ]
- [注意すべきポイント]

## 参考資料
- [関連ドキュメントやリンク]
```

**注意事項**:
- Issue作成前に必ずユーザーの確認を得ます
- 大量のIssue（20件以上）を作成する場合は、段階的な作成が提案されます
- README.mdの内容が不明確な場合は、ユーザーに詳細確認されます
- 既存のIssueと重複しないように`gh issue list`で確認されます
- TDD（テスト駆動開発）で対応できるように、各Issueに具体的なタスクと受け入れ基準が記載されます

**推奨される使用タイミング**:
- 新規プロジェクトを立ち上げた直後
- README.mdを大幅に更新した後
- プロジェクトの方向性を再定義した後

---

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

### /refactor

**説明**: コードの改善提案に基づいて即座にリファクタリングを実施する

**構文**:
```
/refactor
```

**引数**: なし

**動作フロー**:
1. 現在のコードを読み取る
2. 既存のテストコードを確認する
3. 改善ポイントを特定:
   - コードの可読性の問題
   - パフォーマンスの問題
   - エラーハンドリングの不足
   - 重複コードの存在
   - 命名規則の不統一
   - 複雑度の高い箇所
4. リファクタリングを実施
5. テストを実行して動作確認
6. Lintを実行してコード品質を確認
7. 変更内容を報告

**使用例**:
```
/refactor
```

**前提条件**: なし

**注意事項**:
- リファクタリングは既存の動作を変更しません
- テストが通らない状態でコミットされません
- 一度に大きな変更をせず、小さな単位で進めます
- 必要に応じてコミットが分割されます

---

### /fix-lint

**説明**: プロジェクトのLintエラーを検出し、自動的に修正する

**構文**:
```
/fix-lint [オプション]
```

**引数**:
- `--check-only`: エラーの検出のみで修正は行わない
- `--auto-only`: 自動修正のみ実行(手動修正はスキップ)
- `--type=[tool]`: 特定のLintツールのみ実行(例: `--type=eslint`)
- `--files=[pattern]`: 特定のファイルパターンのみ対象(例: `--files=src/**/*.ts`)

**動作フロー**:
1. プロジェクトの設定ファイルを確認(package.json、pyproject.toml等)
2. 使用されているLintツールを特定
3. Lintを実行してエラーを検出
4. エラーを自動修正可能/手動修正が必要に分類
5. 自動修正可能なエラーを修正:
   - **JavaScript/TypeScript**: `eslint --fix`, `prettier --write`
   - **Python**: `ruff check --fix`, `black .`, `isort .`
6. 手動修正が必要なエラーを修正
7. テストを実行して既存機能が壊れていないことを確認
8. 修正結果をレポート形式で報告

**使用例**:
```
/fix-lint
/fix-lint --check-only
/fix-lint --type=eslint
```

**前提条件**: なし

**注意事項**:
- 修正前にgit statusを確認し、未コミットの変更がある場合は警告されます
- 大量の変更が発生する場合は、確認が求められます
- 修正後は必ずテストが実行されます
- Lintの設定ファイル自体は変更されません

---

### /docs

**説明**: 現在のコードを解析し、包括的なドキュメントを生成する

**構文**:
```
/docs [オプション]
```

**引数**:
- `--inline`: インラインドキュメントのみ生成
- `--external`: 外部ドキュメントファイルのみ生成
- `--format=<形式>`: ドキュメント形式を明示的に指定
- `--lang=<言語>`: ドキュメントの言語を指定(デフォルト: 日本語)
- `--examples`: より多くの使用例を含める

**動作フロー**:
1. 対象コードの読み取り
2. ドキュメント内容を決定:
   - **関数・メソッド**: 目的、パラメータ、戻り値、例外、使用例
   - **クラス**: 目的、プロパティ、メソッド、継承関係、使用例
   - **モジュール**: 目的、機能概要、API、依存関係、使用方法
   - **プロジェクト全体**: 概要、機能、アーキテクチャ、セットアップ手順
3. ドキュメント形式を選択:
   - JSDoc(JavaScript/TypeScript)
   - docstring(Python)
   - RDoc(Ruby)
   - JavaDoc(Java)
   - Markdown(README、ガイド)
4. ドキュメントを生成・配置
5. 品質基準を満たしているか確認

**使用例**:
```
/docs
/docs --inline
/docs --format=jsdoc --examples
```

**前提条件**: なし

**ドキュメント品質基準**:
- **正確性**: コードの実際の動作と一致
- **完全性**: 必要な情報がすべて含まれる
- **明確性**: 専門知識のないユーザーでも理解できる
- **簡潔性**: 冗長でなく、要点を押さえている
- **一貫性**: プロジェクト内の他のドキュメントと表記が統一
- **実用性**: 実際の使用に役立つ例が含まれる

**注意事項**:
- 既存のドキュメントがある場合は、それを尊重しつつ不足部分を補完します
- コードは変更されません(ドキュメントのみ追加・更新)
- 自動生成であることを示すコメントは付けられません

---

### /pr-review

**説明**: GitHub Pull Requestの内容をレビューし、改善提案を提示する

**構文**:
```
/pr-review <PR番号> [オプション]
```

**引数**:
- `<PR番号>`: レビューするPRのID(例: 123)
- `--checkout`: PRのブランチをチェックアウトしてローカルで確認
- `--test`: テストを実行して結果を確認
- `--comment`: レビュー結果をPRにコメントとして投稿

**動作フロー**:
1. 前提条件を確認:
   - GitHub CLIが認証済み(`gh auth status`)
   - リポジトリが存在する
   - リモートリポジトリが設定されている
2. `gh pr view <PR番号>`でPRの詳細を取得
3. `gh pr diff <PR番号>`でPRの差分を確認
4. コードレビューを実施:
   - **コードの品質**: 可読性、命名規則、コメント、複雑度
   - **機能性**: 実装の妥当性、エッジケース、エラーハンドリング
   - **テスト**: テストの充足性、テストケースの適切性、カバレッジ
   - **セキュリティ**: セキュリティ上の問題、入力値検証、機密情報漏洩リスク
   - **パフォーマンス**: パフォーマンス問題、不要な処理、リソース使用
5. レビューコメントを作成(優先度付き)
6. 総合評価を提示(Approve/Request Changes/Comment)

**使用例**:
```
/pr-review 42
/pr-review 42 --checkout --test
/pr-review 42 --comment
```

**前提条件**:
- GitHub CLIが認証済み
- リポジトリがGitHubに存在する

**レビュー結果のフォーマット**:
```markdown
## PR概要
- タイトル: [PRのタイトル]
- 変更ファイル数: [数]
- 追加行数: [数]
- 削除行数: [数]

## 良い点
- [良い点1]
- [良い点2]

## 指摘事項

### 必須(Must Fix)
- [ファイル名:行番号] [指摘内容]

### 推奨(Should Fix)
- [ファイル名:行番号] [指摘内容]

### 任意(Nice to Have)
- [ファイル名:行番号] [指摘内容]

## 総合評価
[Approve/Request Changes/Comment]

## 理由
[評価の理由]
```

**注意事項**:
- レビューは建設的で具体的な提案を含みます
- 良い点も併せて伝えられます
- 優先度が明示されるため、対応の判断がしやすくなります

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

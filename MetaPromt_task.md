# BWCシステムプロンプト参照ガイド

/command_nameの形式でシステムプロンプトの冒頭に記載し、@agent_nameをシステムプロンプトに入れてください。1コマンド+複数エージェントの組み合わせが可能です。システムプロンプトを作成する際は下記に記載のコマンドやサブエージェントを使うようにしてください

## プロンプト構造テンプレート

### 基本構造
```
/command_name @agent_name1 @agent_name2 @agent_name3

[タスクの目的と概要]

## エージェント役割分担
- @agent_name1: [担当する作業内容]
- @agent_name2: [担当する作業内容]
- @agent_name3: [担当する作業内容]

## 入力要件
- パラメータ1: [説明]
- パラメータ2: [説明]

## 出力形式
- [期待される結果の形式]
- [ファイル出力がある場合の指定]

## 実行条件
- [前提条件]
- [依存関係]

## エラーハンドリング
- [想定されるエラーケース]
- [対応方法]
```

### 単一エージェント使用例
```
/optimize @python-expert

Pythonコードのパフォーマンス最適化を実行

## エージェント役割分担
- @python-expert: コード解析、最適化実装、パフォーマンステスト

## 入力要件
- target_file: 最適化対象のPythonファイルパス
- performance_target: 目標改善率（例：30%高速化）

## 出力形式
- 最適化されたコードファイル
- パフォーマンス改善レポート（before/after比較）

## 実行条件
- Python 3.8以上
- プロファイリングツール利用可能

## エラーハンドリング
- 構文エラー時: 元ファイルを保持し修正提案
- 依存関係不足時: 必要パッケージリスト提示
```

### 複数エージェント使用例
```
/create-pr @frontend-developer @ui-ux-designer @test-writer-fixer

ユーザーダッシュボード機能の包括的実装とテスト付きPR作成

## エージェント役割分担
- @frontend-developer: Reactコンポーネント実装、状態管理、API統合
- @ui-ux-designer: ユーザーインターフェース設計、アクセシビリティ対応
- @test-writer-fixer: ユニットテスト、統合テスト、E2Eテスト実装

## 入力要件
- feature_spec: ダッシュボード機能仕様書
- design_mockup: デザインモックアップファイル
- api_endpoints: 使用するAPIエンドポイント一覧
- user_personas: 対象ユーザーペルソナ

## 出力形式
- Reactコンポーネントファイル（TypeScript）
- UIデザインコンポーネント（Storybook）
- 包括的テストスイート（Jest + Testing Library）
- アクセシビリティ監査レポート
- プルリクエスト（完全なレビューチェックリスト付き）

## 実行条件
- React 18.0以上、TypeScript 5.0以上
- Storybook 7.0以上
- 既存デザインシステムとの整合性
- WCAG 2.1 AA準拠

## エラーハンドリング
- デザイン競合時: エージェント間でデザイン合意形成
- テスト失敗時: 実装とテストの相互調整
- 統合エラー時: エージェント協力による問題解決
```

### 追加使用例

#### 高度なフロントエンド開発（複数エージェント連携）
```
/create-pr @frontend-developer @accessibility-specialist @performance-engineer

ユニバーサルデザイン対応の高性能Reactコンポーネント開発

## エージェント役割分担
- @frontend-developer: Reactコンポーネント実装、状態管理、API統合
- @accessibility-specialist: WCAG準拠、スクリーンリーダー対応、キーボードナビゲーション
- @performance-engineer: レンダリング最適化、バンドルサイズ削減、ロード時間改善

## 入力要件
- component_name: 作成するコンポーネント名
- feature_spec: 機能仕様書
- accessibility_requirements: アクセシビリティ要件
- performance_budget: パフォーマンス予算

## 出力形式
- 高性能Reactコンポーネント（memoization適用）
- アクセシビリティ完全対応テストスイート
- パフォーマンス監査レポート
- プルリクエスト（品質保証済み）

## 実行条件
- React 18以上、TypeScript 5.0以上
- アクセシビリティテストツール導入済み
- パフォーマンス測定環境構築済み

## エラーハンドリング
- アクセシビリティ違反時: エージェント連携による修正
- パフォーマンス要件未達時: 最適化手法の共同検討
```

#### 包括的データベース最適化（複数エージェント連携）
```
/optimize @database-optimizer @security-auditor @performance-engineer

エンタープライズレベルのデータベース最適化とセキュリティ強化

## エージェント役割分担
- @database-optimizer: SQLクエリ最適化、インデックス設計、スキーマ改善
- @security-auditor: SQLインジェクション対策、データ暗号化、アクセス制御
- @performance-engineer: 負荷分散、キャッシュ戦略、監視設定

## 入力要件
- database_system: データベースシステム（PostgreSQL、MySQL等）
- current_performance_metrics: 現在のパフォーマンス指標
- security_compliance_requirements: セキュリティコンプライアンス要件
- scale_requirements: スケール要件

## 出力形式
- 最適化されたデータベーススキーマ
- セキュリティ強化設定
- パフォーマンス監視ダッシュボード
- 包括的最適化レポート

## 実行条件
- データベース管理者権限
- セキュリティポリシー準拠
- 負荷テスト環境利用可能

## エラーハンドリング
- セキュリティ競合時: エージェント協議による最適解決策
- パフォーマンス制約時: 複数角度からの改善提案
```

## ベストプラクティス

### 組み合わせ指針
1. **機能的補完**: コマンドの目的とエージェントの専門性が合致
2. **ワークフロー効率**: 1つのタスクで完結する設計
3. **専門性活用**: 各エージェントの特性を最大限活用
4. **エージェント連携**: 複数エージェント使用時は明確な役割分担を設定

### エージェント選択指針
#### 単一エージェント使用
- 特定領域に特化したタスク
- 単純明快な作業範囲
- エージェント間連携が不要な場合

#### 複数エージェント使用
- 複数専門領域にまたがる複雑なタスク
- 品質の多角的検証が必要な場合
- クロスファンクショナルな開発が必要な場合

### 品質指標
- **実行時間**: タスク完了までの予想時間を明記
- **成功率**: 期待される成功率の目安を設定
- **リソース要件**: 必要なメモリやCPU使用量を記載
- **連携効率**: 複数エージェント使用時の協調効果を測定

### 注意事項
- パラメータは具体的で測定可能な内容にする
- エラーハンドリングは実際のユースケースに基づく
- 出力形式は再利用可能な形で設計する
- 複数エージェント使用時は役割重複を避ける
- エージェント間のコミュニケーション方法を明確化する

## インストール済みエージェント ✓

以下のエージェントが現在プロジェクトにインストールされています：

- **@@deployment-engineer** ✓ - CI/CDパイプライン、Dockerコンテナ、クラウドデプロイメントの設定
- **@@python-expert** ✓ - Pythonコードの高度な機能やパフォーマンス最適化

## 利用可能なエージェント（117個）

### 🔗 blockchain-web3

- **@@blockchain-developer** - スマートコントラクト、DeFiプロトコル、Web3アプリケーションの開発
- **@@hyperledger-fabric-developer** - Hyperledger Fabric v2.5 LTS & v3.x でのエンタープライズブロックチェーンソリューション開発

### 💼 business-finance

- **@business-analyst** - メトリクス分析、レポート作成、KPI追跡
- **@legal-advisor** - プライバシーポリシー、利用規約、免責事項、法的通知の作成
- **@payment-integration** - Stripe、PayPal、決済プロセッサの統合
- **@quant-analyst** - 金融モデル構築、取引戦略のバックテスト、市場データ分析

### 💰 crypto-trading

- **@arbitrage-bot** - 取引所間・DeFiプロトコル間での暗号通貨アービトラージ機会の特定・実行
- **@crypto-analyst** - 暗号通貨市場分析、オンチェーン分析、センチメント分析
- **@crypto-risk-manager** - 暗号通貨取引・DeFiポジションのリスク管理システム実装
- **@crypto-trader** - 暗号通貨取引システム構築、取引戦略実装、取引所API統合
- **@defi-strategist** - DeFiイールド戦略設計、流動性提供、プロトコル相互作用

### 🤖 data-ai

- **@ai-engineer** - LLMアプリケーション、RAGシステム、プロンプトパイプライン構築
- **@context-manager** - 複数エージェント・長期実行タスク間でのコンテキスト管理
- **@data-engineer** - ETLパイプライン、データウェアハウス、ストリーミングアーキテクチャ構築
- **@data-scientist** - SQLクエリ、BigQuery操作、データ洞察の専門家
- **@hackathon-ai-strategist** - ハッカソン戦略、AIソリューションアイデア創出、プロジェクト評価
- **@llms-maintainer** - AIクローラナビゲーション用llms.txtロードマップファイル生成・維持
- **@ml-engineer** - MLパイプライン、モデルサービング、特徴量エンジニアリング実装
- **@mlops-engineer** - MLパイプライン、実験追跡、モデルレジストリ構築
- **@prompt-engineer** - LLM・AIシステム用プロンプト最適化
- **@search-specialist** - ウェブから情報発見・統合する検索専門家
- **@task-decomposition-expert** - 複雑なユーザー目標を実行可能タスクに分解

### 🎨 design-experience

- **@accessibility-specialist** - WCAG 2.1 AA/AAA標準を満たすWebアプリケーション確保
- **@ui-ux-designer** - モダンデザイン原則、アクセシビリティ標準、デザインシステムによるUI/UX設計

### 🏗️ development-architecture

- **@backend-architect** - RESTful API、マイクロサービス境界、データベーススキーマ設計
- **@directus-developer** - Directusアプリケーション構築・カスタマイズ
- **@drupal-developer** - Drupalアプリケーション構築・カスタマイズ
- **@frontend-developer** - Next.js、React、shadcn/ui、Tailwind CSSアプリケーション構築
- **@graphql-architect** - GraphQLスキーマ、リゾルバ、フェデレーション設計
- **@ios-developer** - Swift/SwiftUIネイティブiOSアプリケーション開発
- **@laravel-vue-developer** - Laravel+Vue3フルスタックアプリケーション構築
- **@mobile-developer** - React Native・Flutterアプリ開発
- **@nextjs-app-router-developer** - Next.js App Routerモダンアプリケーション構築
- **@react-performance-optimization** - Reactアプリケーションパフォーマンス最適化専門家
- **@wordpress-developer** - WordPressソリューション構築

### ☁️ infrastructure-operations

- **@cloud-architect** - AWS/Azure/GCPインフラ設計、Terraform IaC、クラウドコスト最適化
- **@database-admin** - データベース運用、バックアップ、レプリケーション、監視管理
- **@database-optimization** - データベースパフォーマンス最適化専門家
- **@database-optimizer** - SQLクエリ最適化、効率的インデックス設計、データベース移行処理
- **@deployment-engineer** ✓ - CI/CDパイプライン、Dockerコンテナ、クラウドデプロイメント設定
- **@devops-troubleshooter** - 本番環境問題デバッグ、ログ分析、デプロイメント失敗修正
- **@network-engineer** - ネットワーク接続デバッグ、ロードバランサ設定、トラフィックパターン分析
- **@terraform-specialist** - Terraformモジュール作成、Infrastructure as Code管理

### 💻 language-specialists

- **@c-developer** - システムプログラミング・組み込み開発のためのCプログラミング専門家
- **@cpp-engineer** - モダンC++機能によるイディオマティックなC++コード作成
- **@golang-expert** - ゴルーチン、チャネル、インターフェースによるイディオマティックなGoコード作成
- **@java-developer** - ストリーム、並行性、JVM最適化によるモダンJavaマスター
- **@javascript-developer** - モダンES6+、非同期パターン、Node.jsのJavaScript専門家
- **@php-developer** - デザインパターン、SOLID原則、モダンベストプラクティスによるイディオマティックなPHPコード作成
- **@python-expert** ✓ - デコレータ、ジェネレータ、async/awaitなど高度な機能によるイディオマティックなPythonコード作成
- **@rails-expert** - モダンパターン・ベストプラクティスによるスケーラブルなRailsアプリケーション構築
- **@ruby-expert** - ベストプラクティス・デザインパターンに従ったイディオマティックなRubyコード作成
- **@rust-expert** - 所有権、ライフタイム、型安全性によるイディオマティックなRustコード作成
- **@sql-expert** - 複雑なSQLクエリ作成・データベースパフォーマンス最適化
- **@typescript-expert** - 高度な型システム機能、ジェネリクス、ユーティリティ型による型安全なTypeScript作成

### 🛡️ quality-security

- **@api-security-audit** - REST APIセキュリティ監査・脆弱性特定
- **@architect-review** - アーキテクチャ一貫性・パターンのコード変更レビュー
- **@code-reviewer** - 品質、セキュリティ、保守性のための専門コードレビュー
- **@command-expert** - 自動化・ツール用CLIコマンド作成
- **@debugger** - エラー、テスト失敗、予期しない動作のデバッグ専門家
- **@dx-optimizer** - 開発者体験専門家：ツール、セットアップ、ワークフロー改善
- **@error-detective** - ログのエラーパターン検索・根本原因特定
- **@incident-responder** - 緊急性・精度による本番インシデント処理
- **@mcp-security-auditor** - MCP実装脆弱性レビュー、認証システム設計、コンプライアンス確保専門家
- **@mcp-server-architect** - MCPサーバー設計・実装：トランスポート層、ツール/リソース/プロンプト定義
- **@mcp-testing-engineer** - MCPサーバーテスト、デバッグ、品質確保
- **@performance-engineer** - アプリケーションプロファイル、ボトルネック最適化、キャッシュ戦略実装
- **@review-agent** - ナレッジ管理システム品質保証専門エージェント
- **@security-auditor** - 脆弱性コードレビュー、セキュアな認証実装、OWASP準拠確保
- **@test-automator** - ユニット、統合、e2eテストによる包括的テストスイート作成

### 📈 sales-marketing

- **@content-marketer** - ブログ投稿、ソーシャルメディアコンテンツ、メールニュースレター作成
- **@customer-support** - サポートチケット、FAQ対応、顧客メール処理
- **@risk-manager** - ポートフォリオ保護・リスク測定専門リスクマネージャー
- **@sales-automator** - コールドメール、フォローアップ、提案テンプレート作成
- **@social-media-clip-creator** - ソーシャルメディアプラットフォーム最適化動画クリップ作成
- **@social-media-copywriter** - ポッドキャストプロモーション専門ソーシャルメディアコピーライター

### 🔬 specialized-domains

多数の専門分野エージェントを含む（学術研究、API文書化、音声品質制御、包括的研究、データ分析、ゲーム開発、レガシー近代化、市場調査分析、OCR処理、ポッドキャスト関連、研究オーケストレーション、SEO最適化など）

## 利用可能なコマンド（37個）

### 🚀 ci-deployment

- **/release** - 変更ログ、バージョン、文書更新による新リリース準備
- **/run-ci** - CIチェック実行・全テスト合格まで エラー修正

### 🔍 code-analysis-testing

- **/check** - コミットなしでプロジェクトチェック・エラー修正
- **/clean** - コードベース全体でリント・フォーマット問題修正
- **/code_analysis** - 品質メトリクス・推奨事項による包括的コード分析
- **/optimize** - コードパフォーマンス分析・3つの具体的最適化改善提案
- **/repro-issue** - 失敗テストケース作成による特定問題再現
- **/tdd** - Red-Green-Refactorプロセス・ブランチ管理によるテスト駆動開発ワークフロー

### 📚 context-loading-priming

- **/context-prime** - README.md読み込み・関連プロジェクトファイル探索によるプロジェクトコンテキスト読み込み
- **/initref** - markdownファイル作成・CLAUDE.md更新による参照文書構築
- **/prime** - キードキュメントファイル読み込み・プロジェクト構造探索によるプロジェクトコンテキスト読み込み
- **/rsi** - AI支援開発プロセス最適化のためのプロジェクトコマンド・文書読み込み

### 📝 documentation-changelogs

- **/add-to-changelog** - Keep a Changelogフォーマットに従ったCHANGELOG.md新エントリ追加
- **/create-docs** - GitHub課題分析・実装計画付き技術仕様作成
- **/docs** - SQLモデル用YAML文書更新・生成
- **/explain-issue-fix** - 課題内タスク実装方法詳細説明
- **/update-docs** - 仕様、ステータス、ベストプラクティス含む実装文書更新

### 🔧 miscellaneous

- **/five** - 問題系統的調査のための5つのなぜ根本原因分析技法適用
- **/mermaid** - SQL/データベースファイルからMermaid使用エンティティ関係図作成
- **/use-stepper** - 問題解決・プロジェクト開発用構造化ステッパーアプローチ使用

### 📋 project-task-management

- **/create-command** - 既存パターン・組織構造に従った新コマンド作成
- **/create-jtbd** - ユーザーニーズ重視の製品機能用Jobs to be Done (JTBD)文書作成
- **/create-prd** - 製品機能用Product Requirements Document (PRP)作成
- **/create-prp** - 研究・コンテキスト収集による包括的Product Requirement Prompt (PRP)作成
- **/todo** - 追加、完了、削除、リスト操作によるtodos.mdファイルプロジェクトtodo管理

### 🔄 version-control-git

- **/bug-fix** - 課題作成、ブランチ管理、PR提出含むバグ修正系統的ワークフロー
- **/commit** - 従来コミットメッセージ・絵文字による整形されたgitコミット作成
- **/commit-fast** - 最初の提案コミットメッセージ使用gitコミット自動作成・実行
- **/create-pr** - 新ブランチ作成、変更コミット、自動コミット分割によるプルリクエスト提出
- **/create-pull-request** - 適切なテンプレート・規約によるGitHub CLI使用プルリクエスト作成ガイド
- **/create-worktrees** - オープンPR用gitワークツリー管理・新ブランチワークツリー作成
- **/fix-github-issue** - 包括的テスト・検証によるGitHub課題分析・修正
- **/fix-issue** - 指定識別子・説明による特定問題修正
- **/fix-pr** - 現在ブランチPR未解決コメント取得・修正
- **/husky** - CIチェック実行・問題修正によるリポジトリ動作状態検証
- **/pr-review** - 複数視点（PM、開発者、QA、セキュリティ）からの包括的PRレビュー
- **/update-branch-name** - 変更分析に基づく現在gitブランチ名更新

---

**合計**: 117エージェント + 37コマンド


## 注記

- ✓マークはインストール済みエージェントを示します
- 各エージェントには専用ツールセットと具体的な使用場面が定義されています
- PRO ACTIVELY使用が推奨されるエージェントは適切なタイミングで自動的に活用されます
# CLAUDE.md

このファイルは、このリポジトリのコードを扱う際にClaude Code (claude.ai/code) にガイダンスを提供します。

## プロジェクト概要

TradingAgentsは、実際の金融取引会社の構造を模倣したマルチエージェントLLM金融取引フレームワークです。複数の専門化されたAIエージェントが協調して市場分析を行い、取引決定を下すシステムです。

## 開発環境のセットアップとコマンド

### インストール
```bash
# 仮想環境作成（推奨Python 3.13）
conda create -n tradingagents python=3.13
conda activate tradingagents

# 依存関係インストール
pip install -r requirements.txt

# 開発モードでパッケージインストール
pip install -e .
```

### 実行コマンド
```bash
# CLIでの実行（推奨）
python -m cli.main

# 直接実行
python main.py

# コンソールスクリプト
tradingagents
```

### 必要な環境変数
```bash
export FINNHUB_API_KEY=$YOUR_FINNHUB_API_KEY  # 必須：金融データ取得
export OPENAI_API_KEY=$YOUR_OPENAI_API_KEY    # LLMプロバイダー
export GOOGLE_API_KEY=$YOUR_GOOGLE_API_KEY    # Google Gemini使用時
export ANTHROPIC_API_KEY=$YOUR_ANTHROPIC_API_KEY  # Claude使用時
```

## アーキテクチャ概要

### コアコンポーネント

1. **エージェントチーム** (`tradingagents/agents/`)
   - `analysts/`: テクニカル、ファンダメンタル、ニュース、ソーシャル分析
   - `researchers/`: 強気・弱気研究、研究管理
   - `trader/`: 取引計画立案
   - `risk_mgmt/`: 積極的・保守的・中立的リスク評価
   - `managers/`: 最終取引決定

2. **データフロー** (`tradingagents/dataflows/`)
   - FinnHub、Yahoo Finance、Reddit、Google Newsからのデータ取得
   - 統計処理とリアルタイムデータ統合

3. **グラフシステム** (`tradingagents/graph/`)
   - `trading_graph.py`: メインオーケストレーター
   - LangGraphベースのワークフロー管理
   - エージェント間の情報伝播と条件分岐

### 設定システム

- `tradingagents/default_config.py`で設定を管理
- 主要設定：LLMプロバイダー、議論ラウンド数、オンラインツール使用等
- 設定は`TradingAgentsGraph`初期化時にオーバーライド可能

### データフロー

1. **分析段階**: 複数のアナリストエージェントが市場データを分析
2. **研究段階**: 強気・弱気研究者が議論し、研究マネージャーが判断
3. **取引段階**: トレーダーエージェントが取引計画を立案
4. **リスク評価**: 3つの異なる観点からリスク議論
5. **最終決定**: リスクマネージャーが取引承認・拒否を決定

### 重要な技術仕様

- **マルチLLMサポート**: OpenAI、Anthropic、Google Geminiに対応
- **段階的LLM使用**: 深い思考用（o1-mini）と高速思考用（gpt-4o-mini）を使い分け
- **メモリ管理**: Redis、ChromaDBでのベクトルストレージ
- **CLI**: Rich、Typerベースの対話型インターフェース

## コードを変更する際の注意点

- エージェント追加時は対応する設定を`default_config.py`に追加
- 新しいデータソース追加は`dataflows/`に実装
- LangGraphノード追加時は`trading_graph.py`のワークフローを更新
- APIキー等の機密情報は環境変数で管理
- python-dotenvによる.envファイル読み込みに対応済み

## 技術スタック・ベストプラクティス

### LangChain/LangGraphの最適化

**エラーハンドリング強化**:
```python
# API失敗時の指数バックオフ実装
async def execute_with_retry(self, task, max_retries=3):
    for attempt in range(max_retries):
        try:
            return await self.execute(task)
        except (RateLimitError, APIConnectionError) as e:
            if attempt == max_retries - 1:
                raise
            await asyncio.sleep(2 ** attempt)  # 指数バックオフ
```

**メモリ管理**:
- ChromaDBクライアントは永続化設定を使用
- バッチサイズを制限してメモリ使用量を制御
- エージェント別に関連コンテキストのみフィルタリング

**エージェント通信最適化**:
- スーパーバイザーの冗長な再生成を避ける
- メタデータ（信頼度、タイムスタンプ、ソースエージェント）を含む構造化メッセージ
- エージェントタイプ別のコンテキスト関連度フィルタリング

### 金融データ処理の最適化

**APIレート制限対応**:
```python
# プロバイダー別レート制限設定
rate_limits = {
    'yahoo_finance': 100,  # calls/min
    'finnhub': 60,         # calls/min  
    'reddit': 60           # calls/min
}

# 429エラー時の自動リトライ機能
# Retry-Afterヘッダーを尊重
```

**データ品質管理**:
- 価格データの妥当性チェック（負の価格、異常な変動率）
- 50%以上の価格変動時のアラート機能
- データソース別のバリデーションルール

**キャッシュ戦略**:
```python
# データタイプ別キャッシュTTL
cache_ttls = {
    'daily_prices': 86400,     # 1日
    'hourly_prices': 3600,     # 1時間  
    'news': 1800,              # 30分
    'fundamentals': 604800     # 1週間
}
```

### Python開発の最適化

**非同期処理**:
- データ取得タスクの並行実行（market_data, news_data, social_data, fundamentals_data）
- `asyncio.gather()`でエラーハンドリング付き並行処理
- 失敗したデータソースでも他のソースで処理継続

**構造化ログ**:
```python
# 取引決定のログ
logger.info("trading_decision", 
    ticker=ticker, date=date, decision=decision, 
    confidence=confidence, reasoning=reasoning)

# パフォーマンス追跡
logger.info("performance_tracking",
    predicted_return=predicted, actual_return=actual)
```

**設定管理**:
- Pydanticを使用した型安全な設定クラス
- 環境別設定ファイル（.env）サポート  
- API制限、キャッシュTTL、リスク管理パラメータの一元管理

### AI/LLMアプリケーションの最適化

**トークン管理**:
```python
# コンテキスト優先度に基づくトークン最適化
priority_order = ['fundamentals', 'market', 'news', 'social']
# 制限内で最も重要な情報を保持
```

**コスト最適化**:
- タスク複雑度に基づくモデル選択
- レスポンスキャッシュによる重複API呼び出し削減
- プロンプト最適化によるトークン使用量削減

**プロンプトエンジニアリング**:
- エージェントタイプ別の最適化されたプロンプト
- 信頼度スコア要求の組み込み
- トークン制限の明示的指定

### 実装優先度

**高優先度（即座に実装推奨）**:
1. APIレート制限対応とエラーハンドリング
2. 構造化ログの導入
3. データ品質バリデーション

**中優先度（次期リリース）**:
1. 非同期処理への移行
2. 改善されたキャッシュシステム  
3. トークン最適化

**低優先度（長期的改善）**:
1. 高度なプロンプトエンジニアリング
2. パフォーマンス監視システム
3. マルチエージェント通信の更なる最適化

## プロジェクト制限事項

- 現在テストスイートは存在しない
- リント、フォーマット設定は未設定
- これは研究目的のフレームワークであり、実際の金融アドバイス用ではない
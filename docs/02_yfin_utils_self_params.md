# チケット#02: YFinanceUtilsクラスのselfパラメータ修正

## 概要
`YFinanceUtils`クラスのメソッドで`self`パラメータが欠如しているため発生している型エラーを修正します。

## 影響範囲
- ファイル: `tradingagents/dataflows/yfin_utils.py`
- エラー数: 10件以上
- 優先度: **高（データ取得機能の基盤）**

## 詳細なエラー内容

### 主要な問題
デコレータ`@decorate_all_methods(init_ticker)`により、メソッドの第一引数`self`が正しく処理されていない。

### 典型的なエラーパターン
```
error: Method must have at least one argument
error: Argument 1 to "get_stock_data" has incompatible type "YFinanceUtils"; expected "str"
```

## 修正方針
最低限の修正で型安全性を確保：
1. 各メソッドに`self`パラメータを明示的に追加
2. デコレータとの互換性を維持

## 具体的な修正内容

### 修正対象メソッド一覧
```python
# 修正前の例
def get_stock_data(
    symbol: Annotated[str, "ticker symbol"],
    start_date: Annotated[str, "start date in YYYY-MM-DD format"],
    end_date: Annotated[str, "end date in YYYY-MM-DD format"]
) -> Dict[str, Any]:

# 修正後
def get_stock_data(
    self,
    symbol: Annotated[str, "ticker symbol"],
    start_date: Annotated[str, "start date in YYYY-MM-DD format"], 
    end_date: Annotated[str, "end date in YYYY-MM-DD format"]
) -> Dict[str, Any]:
```

### 修正が必要なメソッド
- [ ] `get_stock_data`
- [ ] `get_historical_data`
- [ ] `get_company_info`
- [ ] `get_company_news`
- [ ] `get_financial_data`
- [ ] `get_technical_indicators`
- [ ] `get_options_data`
- [ ] `get_earnings_data`
- [ ] `get_dividend_data`
- [ ] `calculate_volatility`

## 作業手順
- [ ] `tradingagents/dataflows/yfin_utils.py`ファイルの確認
- [ ] 各メソッドの第一引数に`self`を追加
- [ ] デコレータとの互換性確認
- [ ] 修正後の型チェック実行
- [ ] 動作確認（データ取得テスト）

## 検証方法
```bash
# 型チェック
mypy tradingagents/dataflows/yfin_utils.py

# 動作確認
python -c "
from tradingagents.dataflows.yfin_utils import YFinanceUtils
utils = YFinanceUtils()
data = utils.get_stock_data('AAPL', '2024-01-01', '2024-01-31')
print('Success')
"
```

## 想定される修正時間
**15分**（10個のメソッドシグネチャ修正）

## 依存関係
なし（他のチケットと独立）

## 注意事項
- デコレータ`@decorate_all_methods(init_ticker)`の動作を変更しない
- 既存の関数呼び出しに影響しないよう、引数の順序と型は維持
- `yfinance`ライブラリとの互換性を保持

## 完了条件
- [ ] 10件以上の`self`パラメータ関連エラーが解消される
- [ ] YFinanceUtilsクラスのインスタンス化と メソッド呼び出しが正常に動作する
- [ ] 既存の金融データ取得機能に影響がない
# チケット#03: 設定管理の型安全性修正

## 概要
設定管理ファイルでの辞書アクセス時の型エラーを修正し、基本的な型安全性を確保します。

## 影響範囲
- ファイル: `tradingagents/dataflows/config.py`
- エラー数: 1件
- 優先度: **中（設定読み込みの安全性）**

## 詳細なエラー内容

### 対象ファイル: tradingagents/dataflows/config.py
```
Line 23: error: Incompatible types in assignment (expression has type "object | Any", variable has type "str | None")
```

### 問題のコード
```python
DATA_DIR = _config["data_dir"]
```

## 修正方針
最低限の修正で型安全性を確保：
1. 辞書アクセスを型安全な方法に変更
2. `None`チェックを追加

## 具体的な修正内容

### 修正箇所: 設定値の読み込み
```python
# 修正前
DATA_DIR = _config["data_dir"]

# 修正後（案1: 基本的な型チェック）
DATA_DIR = _config.get("data_dir") if isinstance(_config.get("data_dir"), str) else None

# 修正後（案2: よりシンプル）
DATA_DIR = _config.get("data_dir")  # 型注釈をstr | Noneに変更
```

## 作業手順
- [ ] `tradingagents/dataflows/config.py`ファイルの内容確認
- [ ] 現在の`_config`の型と使用方法を確認
- [ ] 最適な修正方法を選択（案1または案2）
- [ ] 修正実装
- [ ] 型チェック実行
- [ ] 設定読み込み動作確認

## 検証方法
```bash
# 型チェック
mypy tradingagents/dataflows/config.py

# 動作確認
python -c "
from tradingagents.dataflows.config import DATA_DIR
print(f'DATA_DIR: {DATA_DIR}')
"
```

## 想定される修正時間
**5分**（1箇所の簡単な型修正）

## 依存関係
なし（他のチケットと独立）

## 注意事項
- 設定ファイルの読み込みロジックは変更しない
- `DATA_DIR`を使用している他のモジュールへの影響を最小化
- デフォルト値の設定も考慮

## 代替案
型エラーを無視する場合：
```python
DATA_DIR = _config["data_dir"]  # type: ignore
```

## 完了条件
- [ ] 1件の型エラーが解消される
- [ ] 設定値の読み込みが正常に動作する
- [ ] 他のモジュールから`DATA_DIR`の使用に影響がない
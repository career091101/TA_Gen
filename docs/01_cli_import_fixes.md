# チケット#01: CLIモジュールの基本インポートエラー修正

## 概要
CLIモジュール内で`console`オブジェクトが未定義のため発生している型エラーを修正します。

## 影響範囲
- ファイル: `cli/utils.py`
- エラー数: 7件
- 優先度: **高（基本機能に影響）**

## 詳細なエラー内容

### 対象ファイル: cli/utils.py
```
Line 28:  error: Name "console" is not defined
Line 61:  error: Name "console" is not defined  
Line 87:  error: Name "console" is not defined
Line 119: error: Name "console" is not defined
Line 175: error: Name "console" is not defined
Line 237: error: Name "console" is not defined
Line 270: error: Name "console" is not defined
```

## 修正方針
最低限の修正で型安全性を確保：
1. `rich.console.Console`のインポートを追加
2. `console`インスタンスを作成

## 具体的な修正内容

### 修正箇所1: インポート文の追加
```python
# 修正前（ファイル冒頭）
from rich.console import Console  # この行を追加

# 修正後
from rich.console import Console

console = Console()
```

## 作業手順
- [ ] `cli/utils.py`ファイルの内容確認
- [ ] 適切な場所に`Console`インポートと`console`インスタンス作成を追加
- [ ] 修正後の型チェック実行
- [ ] 動作確認（CLIコマンド実行テスト）

## 検証方法
```bash
# 型チェック
mypy cli/utils.py

# 動作確認
python -m cli.main --help
```

## 想定される修正時間
**5分**（単純なインポート追加のみ）

## 依存関係
なし（他のチケットと独立）

## 注意事項
- `rich`ライブラリは既にrequirements.txtに含まれているため、新規依存関係の追加は不要
- 既存の`console.print()`呼び出しの動作に影響しないよう注意

## 完了条件
- [ ] 7件の型エラーが全て解消される
- [ ] CLIコマンドが正常に実行される
- [ ] 既存機能に影響がない
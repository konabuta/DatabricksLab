# 2. データ加工の基本操作 (Spark利用しないパターン)
## 2.1 PandasによるCSVインポート

```python
import pandas as pd
df = pd.read_csv("")
```
## 2.2 集計、フィルタリング、列追加、グラフ表示  
基礎統計
```python
df.describe()
```
集計
フィルタリング
列追加
```python
df["newcol] = "This is new col"
```

## 2.3 CSV書き込み
```python
df.write()
```
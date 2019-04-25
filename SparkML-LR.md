# Spark ML ロジスティック回帰

## Notebook インポート
使用する下記の Notebook をインポートします。
- インポート手順 : Managing Notebooks - Import a notebook  
https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive


## 説明変数のベクトル化
Spark MLでは、モデル学習で使用するデータの説明変数をベクトルに変換する必要があります。

```python
assembler = VectorAssembler(inputCols=featureCols,outputCol="features")
```

## データの分割
学習データ、テストデータにデータ分割します。

```python
train, test = dataset.select("Quality","features").randomSplit([0.85, 0.15], seed=1)
```

## モデル学習
学習データに対して、ロジスティック回帰を適用して、モデル学習をします。

```python
from pyspark.ml.classification import LogisticRegression
# ロジスティック回帰のモデル学習
model = LogisticRegression(labelCol="Quality", featuresCol="features", regParam=0.1,elasticNetParam=0.3).fit(train)
```

## テストデータにモデル適用
モデルの精度を評価するために、テストデータにモデルを適用します。
```python
predictions = model.transform(test)
```

## モデル評価
モデル精度を表す"ROC面積"を出力します。

```python
from pyspark.ml.evaluation import BinaryClassificationEvaluator

areaUnderROC = BinaryClassificationEvaluator(labelCol="Quality").evaluate(predictions)

print("areaUnderROC: ", areaUnderROC)
```


# [PySpark & Spark SQL 基本操作](./PySpark_Operation.md)  

## 1. 利用するデータ
UCI Machine Learning Repository
https://archive.ics.uci.edu/ml/datasets/adult

Adult データセットという国勢調査データを使用します。48842人の情報と年間所得の情報が含まれます。このデータから、所得が$ 50K /年を超えるかどうかを予測します。

**属性情報**

- age: continuous
- workclass: Private,Self-emp-not-inc, Self-emp-inc, Federal-gov, Local-gov, State-gov, Without-pay, Never-worked
- fnlwgt: continuous
- education: Bachelors, Some-college, 11th, HS-grad, Prof-school, Assoc-acdm, Assoc-voc...
- education-num: continuous
- marital-status: Married-civ-spouse, Divorced, Never-married, Separated, Widowed, Married-spouse-absent...
- occupation: Tech-support, Craft-repair, Other-service, Sales, Exec-managerial, Prof-specialty, Handlers-cleaners...
- relationship: Wife, Own-child, Husband, Not-in-family, Other-relative, Unmarried
- race: White, Asian-Pac-Islander, Amer-Indian-Eskimo, Other, Black
- sex: Female, Male
- capital-gain: continuous
- capital-loss: continuous
- hours-per-week: continuous
- native-country: United-States, Cambodia, England, Puerto-Rico, Canada, Germany...

Target/Label: - <=50K, >50K
## 2. Spark DataFrame によるCSVインポート 

```python
# CSVデータの読み込み
adult = sqlContext.read.format('csv').options(header='true', inferSchema='true').load('adult.csv')
```

```python
# スキーマ情報
adult.printSchema()
```

## 3. DataFrame 基本操作  

```python
# デフォルト表示
display(adult)
# 10件のみ
display(adult.show(5))
```

```python
# 基礎統計量
adult.describe()
```

```python
# カウント
adult.count()
```

```python
# フィルタリング
adult.filter(adult["age"] > 17.0)
```

```python
# 新規計算列
adult = adult.withColumn("education_day", adult.education_num * 365)
```

```python
# 列の削除
adult = adult.drop("education_day")
```


## 4. SQL Table 登録

```python
# Tmp Tableとして登録
adult.createOrReplaceTempView('adultTB')
 ```

 ```sql
 %sql
 select * from adultTB
 ```

## 3.4 Spark SQL 基本操作

```sql
%sql
SELECT * FROM adultTB
```

```sql
%sql
SELECT DISTINCT machineID FROM telemetry ORDER BY machineID
```

```python
# Spark Table to DataFrame
tableDF = spark.sql("select distinct age from adultTB order by age")
display(tableDF)
```


## 5. CSV書き込み  

```python
adult.write.option("header", "true").save("/mnt/output.csv",format="csv")
 ```

## 6. Tips
```python
# Spark Dataframe to Pandas Dataframe
adult_pd = adult.toPandas()
```




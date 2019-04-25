#  Azure SQL Database

## JDBC接続情報の設定
Azure SQL Databaseのサーバ情報を設定します。

```python
jdbcHostname = "<ホスト名>"
jdbcDatabase = "<データベース名>"
username = "<ユーザ名>"
password = "<パスワード>"
jdbcPort = 1433
jdbcUrl = "jdbc:sqlserver://{0}:{1};database={2};user={3};password={4}".format(jdbcHostname, jdbcPort, jdbcDatabase, username, password)
 ```
## データの取得
Azure SQL Database からデータを取得します。

```python
df = spark.read.jdbc(url=jdbcUrl, table="employees", column="emp_no", lowerBound=1, upperBound=100000, numPartitions=100)
display(df)
```

## データの書き込み
Azure SQL Database にデータを書き込みます。

```python
# mode : "append", "overwrite", "ignore", "error"
df.write.jdbc(table="dbo.df_out", url=jdbc_url_db, mode= "append")
```



## 参考
[Connecting to SQL Databases using JDBC - Python Example](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html#python-example)

[JDBC To Other Databases](https://spark.apache.org/docs/latest/sql-data-sources-jdbc.html)
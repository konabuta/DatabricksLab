# 既存 Azure Blob Storage 設定
Workshopで準備しているBlobストレージを DBFS "/mnt/lab" にマウントします。  
## 1. Workshop用 Blobのマウント
1. Azure Databricks のNotebookをオープン
2. 下記コマンドを実行

```python
import os

source = "<Workshopにて情報提供>"
mount_point = "/mnt/lab"
extra_configs = {"<Workshopにて情報提供>"}

try:
  if len(os.listdir('/dbfs/mnt/lab/')) > 0:
    print("Already mounted.")
  else:
    dbutils.fs.mount(
      source = source,
      mount_point = mount_point,
      extra_configs = extra_configs)
    print("Mounted: %s at %s" % (source, mount_point))
except:
  dbutils.fs.mount(
    source = source,
    mount_point = mount_point,
    extra_configs = extra_configs)
  print("Mounted: %s at %s" % (source, mount_point))
```

## 2. Pathの確認 (DBFS, OS)

```python
# DBFSからみたファイルパス
dbutils.fs.ls("/mnt/<mount-name>")
```

```shell
# OSからみたファイルパス
%sh 
ls -la /dbfs/mnt/<mount-name>
```
## 参考
Azure Blob Storage
https://docs.databricks.com/spark/latest/data-sources/azure/azure-storage.html#access-azure-blob-storage-directly
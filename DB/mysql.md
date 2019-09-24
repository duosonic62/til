# MySql

## h2との話
```/*! なんかの処理 */```で囲むと、mysqlでは実行されるけど、h2では実行されないみたいなことができる。  
```/*!32302 なんかの処理*/```みたいな事書くと、3.23.02以上のバージョンでのみ実行される。  
[mysql -9.6コメント構文-](https://dev.mysql.com/doc/refman/5.6/ja/comments.html)

## User・Roleの作成と付与
ユーザ作成
```sql
CREATE USER 'user_name'@'%' IDENTIFIED BY 'password';
```

ロールの作成
```sql
CREATE ROLE 'role_name'@'%';
```

ロールへ権限の付与
```sql
GRANT SELECT, INSERT, UPDATE ON database.* TO 'role_name';
```

ユーザにロールを付与
```sql
GRANT 'role_name'@'%' TO 'user_name'@'%';
```

### 権限とかの確認
割り当ての確認
```sql
SELECT * FROM mysql.role_edges;
```

権限の確認
```sql
SELECT * FROM information_schema.user_privileges;   -- グローバルレベル権限のリスト
SELECT * FROM information_schema.schema_privileges; -- データベースレベル権限のリスト
SELECT * FROM information_schema.table_privileges;  -- テーブルレベル権限のリスト
SELECT * FROM information_schema.column_privileges; -- カラムレベル権限のリスト
SELECT * FROM information_schema.user_privileges WHERE GRANTEE='\'user_name\'@\'%\''; -- 絞り込み
```

## Export

### csv
csvにテーブル情報を出力する場合は、以下のように設定する。
```sql
SELECT * FROM table
  INTO OUTFILE '/output/output.csv'
  FIELDS TERMINATED BY ','
  OPTIONALLY ENCLOSED BY '"';
```

### Dumpfile
```sql
mysqldump -h rdsのホスト -u ユーザ名 -p DB名 > ./出力ファイル
```

### RDSでdumpfileで復元
[参考](https://qiita.com/hikey/items/ab5fca33849d3e3929cb)

* dumpファイルの生成
```sql
mysqldump -uuser_name -p -hhost_name --quick --single-transaction database_name　| sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' > dump.sql
```

* リストア
RDSのパラメータストアでlog_bin_trust_finction_creatorsを1に設定

```sql
mysql -uuser_name -p -hhost_name database_name < dump.sql
```



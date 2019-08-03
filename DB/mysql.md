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
# Flyway with gradle
```gradle

plugins {
  id "org.flywaydb.flyway" version "5.2.3"
}

dependencies {
  /*flyway*/
  implementation "org.flywaydb:flyway-core:${flywayVersion}"
} 

/* DBマイグレーションツール */
flyway {
  url = "url"
  user = "user"
  password = "password"
}

```

## ディレクトリの構成
```
src/main/
    └ resources
        └ db
            └ migration
                ├ afterMigrate.sql          // マイグレーション完了後に実行
                ├ V1.0.0__create_table.sql  
                ├ V1.0.1__add_・・・.sql
                ├ V1.0.2__change_・・・.sql

```

## Spring起動時に動かしたくない
```./gradlew flywayXXX```だけで動かしたい場合は、application.ymlを変更

```yml
spring:
    flyway:
        enabled: false
```



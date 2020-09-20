# FireStore
フルマネージド・サーバレスのNoSqlデータベース
勝手にスケールしてくれる

## モード
Datastore mode - 従来のdatastore互換用
Native mode - とりあえずこれ使っときゃOK?

## アクセス
サーバ -> ライブラリ or Rest or Grpc
クライアント -> Web or iOS or Android

Firebase Auth Security Ruleでアクセス

## 特徴
強整合性
全てのドキュメントフィールドにインデックスがつく
アトミック
IDは分散 -> UUIDとか良さそう

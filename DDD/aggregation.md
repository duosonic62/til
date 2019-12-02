# 集約
トランザクションの整合性の範囲内のオブジェクトをまとめたオブジェクト

## 不変条件
* 常にトランザクションの整合性を保っている必要があるビジネスルール(ロジック)のこと
* トランザクションと集約が一対一の関係になるように設計するのが好ましい
* 逆にトランザクションに不要な要素はその集約に必要な要素であるか検討すること
* 出来るだけUIで行えるリクエストは一つの集約に対する操作に絞ること。

## 整合性の境界
* 整合性の境界とは、その境界内部の操作が一つの不変条件に従っていること
* トランザクションの整合性に必ず従っている整合性の境界が集約の範囲と言えるっぽい

## 小さな集約
他の要素と整合性を保つ必要があるもののみに留めてしまうこと。
* なるべくエンティティを減らす(DBであれば走査するテーブルが減り、高速化できるはず)
* 値オブジェクトを使うこと(DBであれば集約のテーブルにつっこめる)
* エンティティを保持するのではなく、エンティティを識別できる識別子の値オブジェクトを保持する(依存関係の解決はサービスに任せる)

## トランザクション整合性と結果整合性
* 整合性を保つのは処理を実行したユーザ自身 -> トランザクション
* 他のユーザやシステム -> 結果整合性

プルリクで承認依頼を出すときは他のユーザが状態を更新する(承認・拒否)するので結果整合性？
マージするときは自身の実行した処理自体なのでトランザクション？

## 例外
1. ユーザインターフェースの利便性  
    -> 複数のチケットを一気に作りたいとか。
1. 
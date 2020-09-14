
## DDDにおけるオブジェクト指向
下記の二つは区別すること
* ドメインモデル
* データモデル

良いモデル
  * 問題解決ができる
  * 抽象化できている

抽象化 -> 問題解決ができるように現実から必要な側面のみ抽出したモデル

コード <-> モデルの乖離  
ER図にルールを書いていく -> ルールをドメイン層に委譲していく

アプリケーション層はwhat  
ドメイン層にhowを書いていく

集約 -> トランザクションで保ちたい整合性を担保したい範囲

## ドメインモデリング
概念を正しく + 過不足なく 
-> 不要な柔軟性 + 高すぎる自由度
-> 仕様の曖昧さと考慮漏れ

### ユースケース
システムの振る舞いを決定する。確認できるのは下記の2つ。
* 実装する仕様
* 設計の過不足


1. 主シナリオ
2. 分岐による複シナリオ
3. 前後の条件

### CRC
実装に近い形でモデリングできる。

## 神クラス
* 機能分割的なアプローチ
  * 技術的な分割で神クラスになりにくい
  * 外部の要求が存在しない
* 要求分割的なアプローチ
  * SRPを満たしにくい
  * 外部の要求が存在する

例えば要求分割的なアプローチは商品発売日 -> 予約という要求から生まれたもの  
外部の要求で持つべき責務が変わっていくため、SRPを満たしにくい
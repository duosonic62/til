# タスクベースインターフェース
CRUDスタイルではなく、ユーザが完了したいタスクをベースに考える方法

## クライアントとの対話
このアーキテクチャを使用した際の一般的なクライアント - サーバ間の対話

1. 描画するDTOを取得する
1. DTOを描画
1. __ユーザがタスクを実行する__　 -> コマンドの生成
1. __タスクをメッセージ(コマンド？)としてサーバに送信する__
1. サーバが操作の結果を返す

メッセージとは、「販売完了」、「購入命令の承認」や「ローン申請の提出」などのユーザが完了したいタスクのこと。

## コマンド
アプリケーションに実行したいタスクを伝える方法  
コマンドは以下の要素を含む。

* 処理に必要なデータ
* 処理の名前（命令形）

例) 商品の購入
処理に必要なデータ  
顧客ID, 購入したい商品のID ・・・  
処理の名前  
purchaseProducts

```kotlin
class purchaseProducts {
    val customerId: String
    val productIds: List<String>
}

```

### 命名のポイント
「ChangeAddress」「CreateUser」または「DeleteClass」のようななじみ が深い名前のコマンドをすぐ作成しがちだが、避けるべきである。
ユーザの作成(CreateUser)がしたいのかユーザの登録がしたいのか(RegisterUser)、ユーザの認証(AuthorizeUser)がしたいのか、行いたいタスクによって命名を分けるべきである。

## タスクベースユーザインターフェース
ユーザがどのようにソフトウェアを使いたいのかを把握すること、プロセスを通して使用方法を導くことができるようにするユーザインタフェース。

例) イベントリアイテムを非アクティブ化  
* 従来のインターフェース
    - Dtoを表示して、ステータスを非アクティブ、非アクティブ理由をコメントに追記して保存
* タスクベースのインターフェース
    - イベントリアイテムに非アクティブ化するためのリンクを設置
    - ユーザが非アクティブ理由をかくコメントを入力するためのポップアップを表示
    - 非アクティブ化を実行(サーバにコマンドを発行)


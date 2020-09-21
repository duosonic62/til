# IAM
「誰が」「何を」「どのリソースに」操作を行えるか定義する

## Who
* Googleアカウント
* Googleグループサービスアカウント
* G Suite
* Cloud Identity

## What
下記の基本ロールがあるが、細かく設定できるので、セキュリティを担保する場合はカスタムすると良さそう。

### オーナー
read, write, 権限の付与

### 編集者
read, write

### 閲覧者
read

### 請求管理者
支払い管理
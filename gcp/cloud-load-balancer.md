# Cloud Load Balancer

## マネージドインスタンスグループ
インスタンステンプレートを使い単一エンティティとして制御できるグループ。
ローリング更新で新しいテンプレートを指定するだけでグループ内の全インスタンスを簡単に更新できる。  
自動スケーリングも可能。
ゾーンとリージョンで管理が可能だが、複数ゾーンでできるリージョンがおすすめ。  

### ルール
マネージドインスタンスグループ作成時にはルールを指定できる。

* シングルゾーン、マルチゾーンの選択
* 負荷分散に使用するポート
* 使用するテンプレート
* 自動スケーリングの設定

### オートスケーリング
負荷によってインスタンスが増減する。  
ポリシーで基準にできるのはCPU使用率、負荷分散能力、モニタリング指標、Cloud Pub/Subなどのキューベースのワークロード。  

### ヘルスチェック
プロトコル、ポート、ヘルス条件を定義すると、GCPが正常性を診断する。

## HTTP(S) ロードバランサー
OSIモデルのレイヤ７で機能。  
インスタンスに対するHTTPリクエストをグローバルに分散する。  
単一の エニーキャストIPアドレスで公開されるため DNS設定が容易。 
IPv4とIPv6をサポート 

* HTTPリクエストの負荷はポート80または8080で分散
* HTTPSリクエストの負荷はポート443で分散

URLマップを構成して一部のURLを 特定のインスタンスにルーティングし 他のURLを別のインスタンスに ルーティングできる。  
通常は一番ユーザに近いインスタンスに、負荷が大きければ次に近いインスタンスにいく。
負荷分散はラウンドロビン。タイムアウトはデフォルト30秒。

セッションアフィニティを有効にすると、同じクライアントからのリクエストが 常に同じVMインスタンスに送信できる。
分散モードはCPU使用率または１秒あたりのリクエスト数をもとにバックエンドを使い切ったかの判断方法を提供する。

容量の設定は分散モード設定と連動する追加の制御機能で、  
たとえば通常は最大80％のCPU使用率でインスタンスを動作させる場合,
* 分散モードを80％のCPU使用率、 容量を100％に設定
インスタンスのCPU使用率を半減させる場合
* 分散モードを80％のCPU使用率のままにして 容量を50％に設定

### HTTPS ロードバランサー
証明書が ターゲットHTTPSプロキシに インストールされている必要がある。  
HTTPSを使用するには１つ以上のSSL証明書を作成して、ロードバランサのターゲットプロキシで使用可能にする必要がある。 
ターゲットプロキシには最大10個のSSL証明書を設定可能で、SSL証明書ごとにSSL証明書リソースを作成しそこにSSL証明書情報を格納する。 
SSL証明書リソースは負荷分散プロキシでのみ使用できる。

## SSLプロキシロードバランサー
SSLプロキシは暗号化された非HTTPトラフィック用のグローバルロードバランサー  
インスタンスが複数のリージョンにある場合、ロードバランサは容量のある最も近いリージョンにトラフィックを送信する。  
SSLプロキシ負荷分散はIPv4とIPv6の両方のアドレスをサポートし、プロキシとバックエンド間のトラフィックでは SSLまたはTCPを使用する。  
一般的にはSSLが良い。
勝手にセキュリティバッチとか当ててくれる。

## TCPプロキシロードバランサー
TCPプロキシは暗号化されていない非HTTPトラフィック用のグローバル負荷分散。  
SSLプロキシ負荷分散はIPv4とIPv6の両方のアドレスをサポートし、プロキシとバックエンド間のトラフィックでは SSLまたはTCPを使用する。 
一般的にはSSLが良い。
勝手にセキュリティバッチとか当ててくれる。

## Network ロードバランサー
ネットワーク負荷分散は非プロキシのリージョン負荷分散サービスで、全てのトラフィックはプロキシではなく、ロードバランサを通過する。  
このサービスは転送ルールを使用してアドレス、ポート、プロトコルタイプなどの受信IPプロトコルデータを基にシステムの負荷を分散するため、他ロードバランサが対応していないポートに分散できる。  
バックエンドはインスタンスグループまたはターゲットプールリソース(転送ルールから受信トラフィックを受信するインスタンスグループを定義したもの)を指定できる。  


## 内部ロードバランサー
CP/UDPトラフィック用のプライベートなリージョン負荷分散サービス。  
サービスにアクセスできるのは 同じリージョンにあるVMインスタンスの内部IPアドレスを介した場合のみ。  
レイテンシの改善が期待できる。  
グローバルロードバランサー -> 内部ロードバランサ -> アプリサーバのような構築が効果的。  
 
# Cloud Interconnect
データ送信量は 
Dedicated Interconnect > Partner Interconnect(プロバイダ次第) > Cloud VPN
SLAあり

## Dedicated Interconnect
Dedicated InterconnectではオンプレミスとGoogleのネットワーク間を直接物理的に接続する。  
大量のデータをネットワーク間で転送できる。  

## Partner Interconnect
サポート対象のプロパイだを通してGCPと接続する。  
Cloud Routerとオンプレミスルーターの間でBGPセッションを確立すればサービスプロバイダのネットワーク経由でトラフィック転送を開始できる。

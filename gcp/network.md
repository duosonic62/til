# ネットワーク

## VPC
どのリージョンのサブネットがもて、同じサブネットに複数のゾーンを跨ぐことも可能(多分リージョンは無理)
カスタムvpc内のサブネットサイズを変更してもIPアドレスは変更されない。

他のプロジェクトにアクセスする場合はVPCピアリングを使う

1プロジェクトあたり5ネットワークがデフォルトで割り振られる。

IPレンジの拡張は可能だが縮小は不可
複数のネットワークインターフェースや、VPCネットワークピアリング、オンプレミスネットワークとのVPNや他の接続を構成する際はCIDRの競合が起きやすくなるので、サブネットを拡張しすぎない事。

上り(リクエスト）は無料、下りは同じゾーンで内部IPを利用すると無料。

### デフォルト
* 各リージョンに1つのサブネットが割り当てられている(CIDRブロックは重複しない)
* ファイウォールルールであらゆる場所からのICMP、RDP、SSH 上りトラフィックを許可する。
* ネットワーク内のトラフィックはプロトコル・ポートを問わずに許可

### Auto Mode
* デフォルトのネットワーク
* 自動でサブネットが作成される(サブネットマスクは /20 -> /16まで変更可能)

### Custom Mode
* サブネットが作成されない
* RFC1918アドレス空間を利用できる

10.128.0.0/20
10.132.0.0/20
```
gcloud compute networks create managementnet --project=qwiklabs-gcp-00-f6b76860b557 --subnet-mode=custom --bgp-routing-mode=regional

gcloud compute networks subnets create managementsubnet-us --project=qwiklabs-gcp-00-f6b76860b557 --range=10.130.0.0/20 --network=managementnet --region=us-central1

gcloud compute --project=qwiklabs-gcp-00-f6b76860b557 firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=tcp:22,tcp:3389,icmp --source-ranges=0.0.0.0/0
gcloud beta compute --project=qwiklabs-gcp-00-f6b76860b557 instances create managementnet-us-vm --zone=us-central1-c --machine-type=f1-micro --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=201893138824-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=managementnet-us-vm --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any

```

## ファイアウォール
インスタンスのメタデータタグを使用してルール定義が行える。例えばwebとタグ付しておいて443をアクセス可能にしておくなど。。。

## CloudLoad Balancer

## 8.8.8.8
DNSサービス

## CDN 
エッジキャッシュサーバ

## Cloud
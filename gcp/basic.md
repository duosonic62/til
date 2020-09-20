# GCP

## API
API の選択肢
### Server
Compute Engine - ec2
Kubernetes Engine - container (ECS?)

### Serverless
Cloud Functions - lambda? faas (ちょっとした関数)
App Engine - ~fargate~  paas (デプロイ簡単)
Cloud Run - fargate? コンテナを動かす　-> Knative情ならどこでも動く

## Project 
リソースの集合体  
IAMで設定可能な単位
上位のフォルダ(または組織ノード)がある場合、上位で設定したIAMが下位のフォルダまたはプロジェクトに適用される。  

下位のフォルダまたはプロジェクトで上位で設定した権限を削除することはできないが、それより緩い権限を付与することは可能
例:)
上位フォルダ -> リード権限
下位フォルダ -> (リード権限) + ライト権限
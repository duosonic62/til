# Github Actions 
## submoduleについて
プライベートのサブモジュールを使用している場合、下記のようにcheckoutを使用する。
```yml
- name: submodule
  uses: actions/checkout@v1
  with:
    submodules: true
    token: ${{ secrets.PERSONAL_TOKEN }}
```
GithubActionsでは、別のリポジトリを操作するには用意されたGITHUB_TOKENでは権限が足りない。そのため、secretsにトークンを登録しておく必要がある。

また、このトークンはhttpsでしか使えないので、サブモジュール自体もhttpsで登録しておく必要がある。
.gitmodulesで確認できる。

# プライベートリポジトリにあるGihubActionsを使う
組織内でGithubActionsを使いまわしたいけど、外部に公開したくない！と言う要件は結構あるかと思います。
現在、GitHubActionsではプライベートリポジトリに作成したアクションは他のリポジトリから参照できません。。。
https://help.github.com/ja/actions/automating-your-workflow-with-github-actions/configuring-a-workflow
> プライベートリポジトリで定義されているアクションを使用するには、ワークフローファイルとそのアクションが同じリポジトリになければなりません。 同じOrganizationにある場合でも、他のリポジトリで定義されているアクションは、ワークフローで使用できません。

なんとかしてプライベートのリポジトリにあるアクションを使いまわしたい。。。

## サブモジュールとしてアクションを取り込む
なんとかならないものかと頭を捻らせていると、同じリポジトリ内にあれば使えると言うことを思い出しました。
> プライベートリポジトリで定義されているアクションを使用するには、ワークフローファイルとそのアクションが同じリポジトリになければなりません。

ならば、アクションが入ってるリポジトリをサブモジュールとして取り込めば、ワークフローファイルと同じリポジトリにアクションファイルが置けますね！

早速やってみます。。。
あ、アクションの入っているリポジトリと、そのアクションを使いたいプロジェクトを作成したリポジトリはすでに作成してあることを前提に説明させていただきます！
今回の説明では以下のように呼ばせてもらいます！
* プロジェクトリポジトリ -> アクションを使用するリポジトリ
* アクションリポジトリ -> アクションが登録してあるリポジトリ

## パーソナルアクセストークンの取得
GitHub Actionsでプライベートのリポジトリの情報を取得するには、そのリポジトリのアクセス権限を持ったアクセストークンが必要です。以下の資料を参考にしてトークンを取得しましょう。  
(この記事ではPERSONAL_ACCESSTOKENと言う名前でトークンを設定したことを前提に説明していきます。)  

[コマンドライン用の個人アクセストークンを作成する](https://help.github.com/ja/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line)

次のサブモジュール設定でアクセストークンの文字列を使用しますので、コピーをとっておきましょう。

## アクションを使いたいプロジェクトにサブモジュールの設定
それではプロジェクトリポジトリにサブモジュールとしてアクションリポジトリを設定していきます。  
GitHub Actionsでサブモジュールの取得にパーソナルアクセストークンを使用するために、以下のような手順が必要となります。  
また、パーソナルアクセストークンはsshではなく、httpsでしか使用できないので注意が必要です。

* 取得したパーソナルアクセストークンを~/.netrcに設定してhttpsでローカルからアクションリポジトリにアクセスできるようにする　[参考: GitHubでユーザ名・パスワード省略（push, pull）](https://qiita.com/azusanakano/items/8dc1d7e384b00239d4d9)  
    ~/.netrcに以下を追記
    ```
    machine github.com
    login [{GitHubのユーザ名}]
    password [{所得したパーソナルアクセストークンの文字列}]
    ```
* プロジェクトリポジトリにサブモジュールとしてアクションリポジトリを追加 [参考: Git submodule の基礎](https://qiita.com/sotarok/items/0d525e568a6088f6f6bb)  
    プロジェクトリポジトリに移動して下記のコマンドでサブモジュールを追加(アクションリポジトリのURLは必ず __https__ で！)
    ```
    git submodule add https://{アクションリポジトリのURL} .github/actions
    ```
    上記のコマンドでは、.githubディレクトリ内のactionsディレクトリにサブモジュールを設定していますが、他の場所でも大丈夫です。

* 変更をコミット

これでアクションをサブモジュールとして追加できました！  
が、GitHub Actionsでサブモジュールを使うにはもう少し設定が必要です。。。

## プロジェクトからアクションを使ってみる
GitHub Actionsでサブモジュールを使うには、パーソナルアクセストークンを使うことが必要です。  
ワークフローにパーソナルアクセストークンを直書きしてもサブモジュールを取得できますが、セキュリティの観点から、プロジェクトリポジトリのsecretに登録する方法を説明します。

### パーソナルアクセストークンをプロジェクトリポジトリのsecretに登録
プロジェクトリポジトリのsecret情報としてパーソナルアクセストークンを以下の手順で登録していきます。  

1. プロジェクトを開く
1. タブでSettingsを選択
1. メニューバーでSecretsを選択
1. Add a new secretを選択
1. NameにPERSONAL_ACCESSTOKENを、Valueに先ほど取得したパーソナルアクセストークンの文字列を入力
1. Add secret

これでやっとプロジェクトリポジトリでアクションを追加する準備が全て完了しました！

### workflowの作成
ワークフローファイルでのサブモジュールにしたアクションの使い方は、同じリポジトリ内にアクションがある時と同様にプロジェクトのルートからのパスを指定してあげます。また、サブモジュールをチェックアウトする記述も必要です。  

```yml:/.gihub/workflow/pull_request.yml
name: Using Private Repositroy Sample

on:
  pull_request:
    branches:
    - master
jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - name: checkout with submodule
      uses: actions/checkout@v1
      with:
        submodules: true
        token: ${{ secrets.PERSONAL_TOKEN }}

    - name: hoge
      uses: ./.github/actions/hoge

    - name: huga
      uses: ./.github/actions/huga

```

サブモジュールをチェックアウトする記述は以下の部分ですね。サブモジュールを取得するにはcheckoutのアクションを使用してwithの所にtokenを渡す必要があります。  
tokenには先ほどsecretsに登録したパーソナルアクセストークンを指定してあげます。

```yml
- name: checkout with submodule
  uses: actions/checkout@v1
  with:
    submodules: true
    token: ${{ secrets.PERSONAL_TOKEN }}
```

## 実行結果
上記の例ではプルリクエストを作成時にアクションを実行してくれます。
実際にプルリクエストを作ってみると・・・！できました！！！

## まとめ
いかがでしたでしょうか。トークンの扱いが少し面倒ですが、アクションのコードが一元管理できるのは結構いいのかなと個人的には思います。  
~~まぁ、GitHubがプライベートリポジトリに作成したアクションを使用できるようにしてくれれば、最高なんですけどね~~


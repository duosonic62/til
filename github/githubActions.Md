# Github Actions 
## submoduleについて
privateのサブモジュールを使用している場合、下記のようにcheckoutを使用する。
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


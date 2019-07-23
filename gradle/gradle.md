# Gralde

## 環境変数を使いたい
```gradle
def env =　System.getenv()['ENV_NAME']
```
これで取得できる。該当の環境変数が設定されていない場合はnullが設定されるので

```gradle
if (env == null) {
    env = "default value"
  }
```
初期化を行った方が良いかも。以下みたいにかきたい・・・

```gradle
def env = System.getenv()['ENV_NAME'] ? "default value"
```
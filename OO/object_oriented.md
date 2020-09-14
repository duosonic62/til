関数分割をうまく扱いたい
* 凝集度
* 結合度

## 凝集度
DRY <-> 論理的凝集はトレードオフ感がある。  

### 論理的凝集
フラグによる処理の切り替え。実行側が内部構造の理解が必要  
似てるユースケースをまとめない。  
* モジュールにフラグが大きくなったら
* 時間的凝集に機能的なことを書きがち

### 時間的
関係ないけど、同時にやりたいことを同じ関数にする。
  -> 問題があるわけではない
  -> mainとかはそう
  -> useCaseとかそうだよね
  -> 内部で手続き的な処理をするのはNG
  -> 機能的関数を呼び出すだけにしよう

### 機能的

## 結合度
スタンプ + データ + メッセージ -> やってよし  
制御結合はやってしまいがちだが、論理的凝集度を意識すると大丈夫なことが多い。

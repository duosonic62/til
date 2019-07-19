# Coroutine Basic
https://qiita.com/pljp/items/768f26bc09913f60dd07

## coroutineとThread
> コルーチンは 中断可能な計算 のインスタンスとして考えることができます。ある時点で中断し、後で別のスレッドで実行を再開することができます。
コルーチンはお互いを呼び出す（そしてデータを前後に渡す）ことで、協力的なマルチタスキングのための機構を形成することができます。

別のスレッドで動かせる(せてしまう)

## suspend関数
実行を中断して呼び出しが完了すると再開する関数

## SupervisorJob
[kotlinx.coroutines - SupervisorJob](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-supervisor-job.html)
# Dispatcherを自作する
## ExecutorService.asCoroutineDispatcher()を使う
下のように完全に自作することもできるが
```kotlin
ThreadPoolExecutor(
        10,
        10,
        10L,
        TimeUnit.SECONDS,
        SynchronousQueue()
    ).asCoroutineDispatcher()
```

普通に使う分にはExecutorsのファクトリメソッドで良さそう
```kotlin
Executors.newCachedThreadPool()
        .asCoroutineDispatcher()
```

object使うとシングルトン簡単に作れるから、用途に合わせたDispatcher作るの簡単そう
```kotlin
object DbDispatcher {
    val IO = Executors.newCachedThreadPool()
        .asCoroutineDispatcher()
}
```

使うとき(launchじゃなくてもasyncとかのビルダーでも良い)
```kotlin
CoroutineScope(DbDispatcher.IO).launch { 
    ・・・
}
```
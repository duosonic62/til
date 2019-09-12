# 命名規則
## チェックしてエラーをスローしたい
例えば値チェックして負数ならエラーを投げたいときは `thorwIf条件`というようにすると良さそう。

```kotlin
fun throwIfNegativeNumber(num: int) {
    if (num < 0) throw NegativeNumberException()
}

```
# ファクトリー
複雑なインスタンス化をカプセル化することが目的。
また、インスタンス生成時のバリデーションを一限で管理できるため、後からコツコツバリデーションをかけなくて済む。
クライアント側の手間も減るのも◯

## 集約ルートに作る
集約自体や集約ルートが保持、または関連のある(その集約の識別子を保持している)集約を生成するファクトリメソッド。
これだと変なCircleIdでCircleNotificationを作成されるのを防げる

```kotlin
class Circle {
    fun scheduleCircleNotification(noticeDateTime: LocalDateTime, message: Message) {
        val notice = CircleNotification.of(this.circleId, noticeDateTime, message)
        nreturn notice
    }
}

```


## サービスに作る
複数のコンテキストにまたがる場合はこっちのが良さそう。
例ではCircleとMember

```kotlin
class CircleService (private val circleRepository: CircleRepository) {
    fun circleMembers(circleId: CircleId) {
        val circle = circleRepository.find(cicleId)
        val members: List<CircleMember> = memberRepository.find(cicle.memberIds).map {
            member -> CircleMember.of(member)
        }
        retrun members
    }
}
```
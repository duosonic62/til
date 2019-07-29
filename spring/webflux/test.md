# WebFluxでのテスト

## コントローラ
@WebFluxTestをコントローラのテストに付与する事によってDIを読み込んでくれる。
Configureも読み込まれているらしく、JsonのconfigureHttpMessageCodecsとか、GlobalExceptionHandlerも取得してきてくれる。

WebMVCより楽では・・・？

```kotlin
@WebFluxTest(SampleController::class)
class SampleControllerTest {
　  @Autowired
    private lateinit var webTestClient: WebTestClient

    @MockBean
    private lateinit var sampleService: SampleService

    @Nested
    inner class SampleMethodTest {
        @Test
        fun positive() {
            webTestClient.get()
            .uri("/v1/sample")
            .header("Content-Type", "json")
            .exchange()
            .expectStatus().isOk
            .expectBody().json(
                """
                {
                    ・・・
                }
                """
            )
        }
    }
}
```

また、ヘッダーはheadersではうまく設定できないので以下みたいな拡張関数用意しておくと楽。
```kotlin
private fun WebTestClient.RequestHeadersSpec<*>.addMochHeader(): WebTestClient.RequestHeadersSpec<*> {
    header("Content-Type", "application/json")
    header("Authorization", "toke...")
    return this
  }
```
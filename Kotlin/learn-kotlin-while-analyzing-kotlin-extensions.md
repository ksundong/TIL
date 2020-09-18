# Kotlin extensions를 분석하면서 공부하는 Kotlin

```Kotlin
fun LocalDateTime.format() = this.format(englishDateFormatter)

private val daysLookup = (1..31).associate { it.toLong() to getOrdinal(it) }

private val englishDateFormatter = DateTimeFormatterBuilder()
        .appendPattern("yyyy-MM-dd")
        .appendLiteral(" ")
        .appendText(ChronoField.DAY_OF_MONTH, daysLookup)
        .appendLiteral("")
        .appendPattern("yyyy")
        .toFormatter(Locale.ENGLISH)

private fun getOrdinal(n: Int) = when {
    n in 11..13 -> "${n}th"
    n % 10 == 1 -> "${n}st"
    n % 10 == 2 -> "${n}nd"
    n % 10 == 3 -> "${n}rd"
    else -> "${n}th"
}

fun String.toSlug() = toLowerCase()
        .replace("\n", " ")
        .replace("[^a-z\\d\\s]".toRegex(), " ")
        .split(" ")
        .joinToString("-")
        .replace("-+".toRegex(), "-")
```

[소스코드 출처](https://spring.io/guides/tutorials/spring-boot-kotlin/)

## Kotlin extensions란

코틀린은 클래스를 상속하거나 Decorator와 같은 디자인 패턴을 사용하지 않고도 새로운 기능을 제공하도록 클래스를 확장하는 기능을 제공합니다. 이것은 `extensions`라는 특별한 선언을 통해 이루어집니다.

예를 들어, 당신이 수정할 수 없는 서드 파티 라이브러리의 클래스에 대한 새로운 함수를 작성할 수 있습니다. 이러한 함수는 원래 클래스의 메서드였던 것 처럼 일반적인 방식으로 호출할 수 있습니다. 이 메커니즘은 `extension functions`라고 합니다.

이미 존재하는 클래스들에 새로운 프로퍼티를 정의하는 `extension properties`도 존재합니다.

자바에서는 abstract 메서드와 함께 util 클래스를 사용하는 방식을 Kotlin에서는 이러한 형태로 대체해서 사용합니다.

즉, 위의 코드는 `LocalDateTime` 타입에 `format()` 메서드를 추가합니다.

## References

- [Kotlin Reference Document/extensions](https://kotlinlang.org/docs/reference/extensions.html)
- [Spring Boot Kotlin Guide Document](https://spring.io/guides/tutorials/spring-boot-kotlin/)

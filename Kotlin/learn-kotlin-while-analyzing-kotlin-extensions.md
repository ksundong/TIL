# Kotlin extensions를 분석하면서 공부하는 Kotlin

```Kotlin
// 1
fun LocalDateTime.format() = this.format(englishDateFormatter)

// 2
private val daysLookup = (1..31).associate { it.toLong() to getOrdinal(it) }

// 3
private val englishDateFormatter = DateTimeFormatterBuilder()
        .appendPattern("yyyy-MM-dd")
        .appendLiteral(" ")
        .appendText(ChronoField.DAY_OF_MONTH, daysLookup)
        .appendLiteral("")
        .appendPattern("yyyy")
        .toFormatter(Locale.ENGLISH)

// 4
private fun getOrdinal(n: Int) = when {
    n in 11..13 -> "${n}th"
    n % 10 == 1 -> "${n}st"
    n % 10 == 2 -> "${n}nd"
    n % 10 == 3 -> "${n}rd"
    else -> "${n}th"
}

// 5
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

## 1번 분석

```Kotlin
fun LocalDateTime.format() = this.format(englishDateFormatter)
```

이 코드에서 `this`는 무엇을 가리킬까요? 앞에 선언된 `LocalDateTime`을 가리킵니다.  
즉, 이 코드는 `LocalDateTime`의 `format(DateTimeFormatter)` 메서드로 연결됩니다.

## 2번 분석

```Kotlin
private val daysLookup = (1..31).associate { it.toLong() to getOrdinal(it) }
```

코틀린에서 `..`은 범위를 표현합니다. `(1..31)` 은 `1 <= i <= 31` 과 동일한 의미입니다.

`associate`는 `Map<K, V>`를 반환하며 인자로 `transform` 함수를 받아 주어진 시퀀스에 적용시킨 결과를 'Key-Value' 쌍으로 `Map`에 담아 반환합니다.

`sequence -> transform -> result(Map)`

`Map`이기 때문에, 나중에 들어온 `Key`의 값이 저장됩니다.

반환된 `Map`은 원래 시퀀스의 반복순서를 유지합니다.

`it`은 `(1..31)` 에서 제공되는 것으로 `Int` 타입입니다. 이를 `to` 문법을 활용해서 `Pair` 타입 튜플을 만들어냅니다. `to` 왼쪽이 `Key`가 되고 `to` 오른쪽이 `Value`가 되는 식입니다.

따라서, `<Long, String>` 형태가 반환됨을 짐작할 수 있습니다.

## 3번 분석

```Kotlin
private val englishDateFormatter = DateTimeFormatterBuilder()
        .appendPattern("yyyy-MM-dd")
        .appendLiteral(" ")
        .appendText(ChronoField.DAY_OF_MONTH, daysLookup)
        .appendLiteral("")
        .appendPattern("yyyy")
        .toFormatter(Locale.ENGLISH)
```

Builder 패턴의 활용입니다. 별로 설명할 내용이 없네요.

`appendText(ChronoField, Map<Long, String>` 메서드의 경우 ChronoField에 해당하는 값을 Key로 String을 찾는 메서드입니다.

## 4번 분석

```Kotlin
private fun getOrdinal(n: Int) = when {
    n in 11..13 -> "${n}th"
    n % 10 == 1 -> "${n}st"
    n % 10 == 2 -> "${n}nd"
    n % 10 == 3 -> "${n}rd"
    else -> "${n}th"
}
```

`switch`문을 대치하는 `when`문의 사용이 두드러집니다.

자바의 `switch`문과 다르게 직관적으로 작성할 수 있다는 장점이 있습니다.

먼저 11부터 13까지는 th를 붙이는 조건이 우선합니다. 그리고 1, 2, 3으로 끝나는 경우 각각 st, nd, rd를 붙여줍니다. 그 외 나머지는 th를 붙여줍니다.

## 5번 분석

```Kotlin
fun String.toSlug() = toLowerCase()
        .replace("\n", " ")
        .replace("[^a-z\\d\\s]".toRegex(), " ")
        .split(" ")
        .joinToString("-")
        .replace("-+".toRegex(), "-")
```

Slug를 만들어주는 메서드입니다. Slug란 어떤 URL을 사람의 가독성 및 간결성을 위해 깔끔하게 정리한 상태로 만드는 것입니다.

먼저 모든 문자는 소문자로 만듭니다. 그리고 줄바꿈 문자를 빈 칸으로 치환합니다.

그 다음 소문자와, 숫자, 빈칸을 제외한 나머지를 전부 빈 칸으로 치환합니다.

빈 칸으로 나눈 다음 `-`로 문자열을 다시 연결시켜줍니다.

`-`가 연속적으로 나오는 문자열을 `-`로 바꿔줍니다.

## References

- [Kotlin Reference Document/extensions](https://kotlinlang.org/docs/reference/extensions.html)
- [Kotlin Reference Document/ranges](https://kotlinlang.org/docs/reference/ranges.html)
- [Kotlin Reference Document/associate](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.sequences/associate.html)
- [Kotlin Reference Document/to](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/to.html)
- [Kotlin Reference Document/when](https://kotlinlang.org/docs/reference/control-flow.html#when-expression)
- [Spring Boot Kotlin Guide Document](https://spring.io/guides/tutorials/spring-boot-kotlin/)
- [Wikipedia/Clean URL](https://en.wikipedia.org/wiki/Clean_URL)

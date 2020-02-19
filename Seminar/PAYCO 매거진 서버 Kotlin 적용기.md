# PAYCO 매거진 서버 Kotlin 적용기

전현구 님, 유재은 님

1. Introduction
2. Motivation
3. Kotlin
4. Migration
5. Refactoring
6. Report
7. Conclusion

## 1. Introduction

Server side Kotlin
Java => Kotlin Migration 에서 시행착오 줄이기

Spring에서 Kotlin으로 개발시 차이점

PAYCO Magazine(SpringBoot 2.0, FreeMarker, MySQL, MyBatis, NginX, Java -> Kotlin, Gradle)

## 2. Motivation

Kotlin for Android

점차 유명서비스에서 Kotlin을 지원하는 추세

Spring에서도 5부터 지원

1. **Null Safety**
2. **Extension Functions**
3. **Java Interoperability**
4. **Data Class**
5. High Order Functions
6. Type Interface
7. Corutines

Test Code 부터 부분적용하고 확대해나가는 것을 권장함

## 3. Kotlin

```kotlin
// Kotiln
data class Article(
  var aaa
  var bbb
  var ccc
  ...
)
```

field라는 개념은 있으나, getter와 setter로만 접근가능

data class로 equals(), hashcode(), toString(), copy(), componentsN() 지원

toStirng() => override할 필요 없이 필드의 값을 보여줌

**Null Safety**

Java Boxing된 타입, Primitive 타입(Null x)

Kotlin ?가 붙으면 null 허용

컴파일 과정에서 NullPointerException 캐치 가능

Java로 디컴파일 가능

## 4. Migration

Java -> 변경해도 문제 없는 부분 적용 -> 비즈니스 로직부분 적용 -> 리팩토링

IntelliJ의 자동 변환 기능 이용

문제점

1. Lombok
   자동 변환은 Lombok을 커버해주지 않음
   No Lombok(property를 Kotlin data class에서 지원하기 때문)

의존성 주입 동일하게 사용 불가

1. Nullable 필드로 변경 및 null로 초기화(x)
2. lateinit 사용 `private lateinit var aaa`

생성자 주입: java 코드와 동일

`val` final과 필드선언 동시에

@RequestParam의 필수 파라미터를 나타낼 때, required 속성을 사용하지 않음(Kotlin의 속성을 이용: ?를 떼준다.)

static 키워드가 없음: companion object를 사용하여 정적 필드와 메소드를 정의
const 키워드(variable), @JvmStatic을 이용(method)하여 Java에서 참조가능

## 5. Refactoring

Kotlin으로 동작하는 것을 확인 완료

자동으로 변환된 코드를 Kotlin의 특성을 살려서 리팩토링

1. if else -> when
   switch-case 문과 유사
   if else가 반복되는 상황을 대체 가능
   intelliJ가 if-else를 when으로 바꿔줌
2. Kotlin Extension function(model)
   `model["title"] = article.title // model.addAttribute("title", article.title)`

## 6. Report

실제 돌아가는데 문제가 없나?

성능상 문제가 없나?

Build Time: 큰 차이 없음

API response time: 큰 차이 없음(성능)

Code Lines: 6% 감소

Characters: 10% 감소

변하지 않는 부분 import, Annotations

변하는 부분

- 의존성 주입 부 코드 라인 감소
- Null Point Exception Handling 소스 코드 부분 제거(if-else, try-cath) (Null Safety)
- if-else -> when구문

Statement(구문)[13%] & Cyclomatic Complexity(순환 복잡도) 감소[12%]

Statement: assign statement 숫자가 감소

순환 복잡도: 코드분기 처리시 경로 수에 따라 복잡도가 상승

중첩 분기수가 줄어든 것이 큼, Null Safety(try-catch, Null Check)

경험: 새로운 코드 작성시 효율성이 좋아짐

- 간결한 문법
- 함수형 프로그래밍 고려한 collection 등
- 가독성 향상, 유지 보수하는데 도움이 됨

## 7. Conclusion

Kotlin 사용법을 어느정도 알아야 함
spring-kotlin 사용법을 알아야 함.
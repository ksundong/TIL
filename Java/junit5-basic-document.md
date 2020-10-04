# JUnit 5 사용하기

## JUnit 5는 어떻게 동작하는가?

JUnit5는 그 이전 버전들하고는 다른 방식으로 동작합니다.

JUnit4까지는 단일 모듈로 동작했다면 JUnit5는 여러 개의 sub-modules로 동작합니다.

> JUnit5 = JUnit Platform + JUnit Jupiter + JUnit vintage

- JUnit Platform은 testing framework를 실행시킬 수 있는 기반이 되어줍니다.
- JUnit Jupiter는 JUnit5 방식으로 테스트 코드를 작성하고, 실행할 수 있도록 해줍니다.
- JUnit Vintage는 JUnit3, 4 버전의 테스트 코드를 실행할 수 있도록 해줍니다.

JUnit은 모든 IDE에서 사용할 수 있으며, 지원합니다. 또한 JUnit의 종속성은 Java Build Tools(Ant, Maven, Gradle) 모두에서 지원합니다.

**JUnit5는 JAVA 8 버전 이상에서 제공됩니다.** 그러나 이전 버전에 컴파일 된 코드는 테스트 할 수 있습니다.

## JUnit Hello World

Gradle 의존성 `build.gradle` 에 해당 코드로 변경합니다.

```groovy
dependencies {
  ...
  testCompile group: 'org.junit.jupiter', name: 'junit-jupiter-api', version: '5.5.2'
}
```

혹은 Spring Boot 2.2 버전 이상을 사용하면 자동으로 5버전을 사용하게 됩니다.

IntelliJ에서는 테스트를 만드는 단축키를 제공하는데 맥 기준 ⌘⇧T/ 윈도우 기준 Ctrl+Shift+T 입니다.

Testing Library는 JUnit5를 선택해주고, 하위 메소드가 있다면 선택적으로 test method 또한 만들 수 있습니다.

`@Test` Annotation으로 JUnit Runner에 이 메소드는 테스트 메소드임을 알려줄 수 있고, Test를 Run하면 JUnit이 알아서 main method가 없어도 Test를 실행시켜줍니다.

그리고 JUnit5 부터는 class나 method가 public이 아니어도 됩니다. 이전에는 둘 다 필요했습니다.

Java의 Reflection을 사용하면 굳이 public이 아니어도 되는 private하거나 default한 method에 접근 할 수 있게됩니다.

`@BeforeAll`

여러 테스트가 모두 실행이 될 때, 모든 테스트를 실행하기 전 딱 한 번만 호출되는 메소드를 선언할 때 사용하는 Annotation입니다. 이 Annotaion을 사용하는 method는 반드시 static해야합니다. private여도 안됩니다. return 타입이 있어도 안됩니다. 반드시 `static void` 해야합니다.

JUnit4는 `@BeforeClass` 로 사용합니다.

`@AfterAll`

`BeforeAll` 과 마찬가지로 모든 테스트를 실행한 후 딱 한 번만 호출됩니다. 이 Annotation도 역시 `static void` 해야합니다.

JUnit4는 `@AfterClass` 로 사용합니다.

`@BeforeEach`

위 Annotation이 붙은 메소드는 모든 테스트를 실행 할 때, 각각의 테스트 메소드를 수행하기 이전에 동작합니다. static일 필요는 없습니다.

JUnit4는 `@Before` 로 사용합니다.

`@AfterEach`

`BeforeEach` 와 마찬가지로 각각의 테스트 메소드를 수행한 이후에 동작합니다.

JUnit4는 `@After` 로 사용합니다.

### 실행 예

```java
import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.*;

class Test {
    @Test
    void test1() {
        System.out.println("Test1");
    }

    @Test
    void test2() {
        System.out.println("Test2");
    }
  
    @BeforeAll
    static void beforeAll() {
        System.out.println("Before All");
    }
  
    @AfterAll
    static void afterAll() {
        System.out.println("After All");
    }
  
    @BeforeEach
    void beforeEach() {
        System.out.println("Before Each");
    }
  
    @AfterEach
    void afterEach() {
        System.out.println("After Each");
    }
}
```

### 예상 동작

```text
Before All

Before Each
Test1
After Each

Before Each
Test2
After Each

After All
```

`@Disabled`

`@Test` Annotation이 적용되어 있는 메소드에 이 Annotation을 입력하면, 전체 테스트 수행시에 해당 메소드는 테스트 하지 않습니다. 당연히 좋지 않은 방법입니다.(기본적으로 단위테스트는 실패하는 테스트는 그냥 내버려 두고, 테스트가 잘못되었다면 테스트를 수정해야 하고, 구현이 잘못되어 실패한다면 구현을 고쳐야합니다.)
부득이 사용할 수 밖에 없는 경우에만 사용해야합니다.

JUnit4는 `@Ignored` 로 사용합니다.

물론 JUnit4로 작성된 코드는 vintage로 사용하면 됩니다.

[출처](https://junit.org/junit5/docs/current/user-guide/#writing-tests)

## 아주 간단한 테스트 예시

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {
  
    private final Calculator calculator = new Calculator();
  
    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }
}
```

`assertEquals(expected, actual)` 은 expected와 actual이 실제로 같은지 비교하여 같으면 Test를 통과하고, 다르면 Test를 실패합니다.

## Annotations

| Annotation               | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `@Test`                  | Annotation이 선언된 메소드가 test 메소드임을 선언합니다. JUnit4의 `@Test` 에 대응합니다. 이 Annotation은 어떤 속성도 선언하지 않습니다. |
| `@ParameterizedTest`     | Annotation이 선언된 메소드가 Parameterized Test임을 나타냅니다. |
| `@RepeatedTest`          | Annotation이 선언된 메소드가 Repeated Test임을 나타냅니다.   |
| `@TestFactory`           | Annotation이 선언된 메소드가 Dynamic Test를 위한 Test Factory임을 나타냅니다. |
| `@TestTemplate`          | Annotation이 선언된 메소드가 등록된 provider가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계된 test case의 template임을 나타냅니다. |
| `@TestMehotdOrder`       | Annotaion이 선언된 테스트 클래스의 테스트 메소드의 실행 순서를 구성하는데 사용됩니다.<br />JUnit4의 `@FixMethodOrder` 와 유사합니다. |
| `@TestInstance`          | Annotation이 선언된 테스트 클래스의 테스트 인스턴스의 생명주기를 관리합니다. |
| `@DisplayName`           | 테스크 클래스 또는 테스트 메소드의 커스텀된 표시 명칭을 정의합니다. |
| `@DisplayNameGeneration` | 테스트 클래스의 커스텀 표시 명칭 생성자를 정의합니다.        |
| `@BeforeEach`            | Annotation이 선언된 메소드가 현재 클래스의 각 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory` 실행 이전에 실행되어야 함을 표시합니다. JUnit4의 `@Before` 에 대응합니다. |
| `@AfterEach`             | Annotaion이 선언된 메소드가 현재 클래스의 각 `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory` 실행 이후에 실행되어야 함을 표시합니다. JUnit4의 `@After`에 대응합니다. |
| `@BeforeAll`             | Annotaion이 선언된 메소드가 현재 클래스의 모든 `@Test`, `@RepeatedTest`, `@TestFactory` 이전에 실행되어야 함을 표시합니다. 추가적으로 `@BeforeEach` 보다 먼저 실행됩니다. JUnit4의 `@BeforeClass` 에 대응합니다. 반드시 static해야 합니다. |
| `@AfterAll`              | Annotaion이 선언된 메소드가 현재 클래스의 모든 `@Test`, `@RepeatedTest`, `@TestFactory` 이후에 실행되어야 함을 표시합니다. 추가적으로 `@AfterEach` 보다 나중에 실행됩니다. JUnit4의 `@AfterClass` 에 대응합니다. 반드시 static해야 합니다. |
| `@Nested`                | 어노테이션이 선언된 클래스가 static 하지 않은 중첩된 테스트 클래스임을 나타냅니다. `@BeforeAll` 과  `@AfterAll` 메소드는 "클래스 별" 테스트 인스턴스 라이프사이클을 사용하지 않는 한 `@Nested` 선언된 테스트 클래스에 직접적으로 사용할 수 없습니다. |
| `@Tag`                   | 클래스 또는 메소드 레벨의 filtering 테스트의 태그를 선언하는데 사용됩니다. TestNG의 Test Group 또는 JUnit4의 Categories와 유사합니다. 이러한 Annotation은 클래스 레벨에서는 상속되지만, 메소드 레벨에서는 상속되지 않습니다. |
| `@Disabled`              | 테스트 클래스 또는 테스트 메소드를 비활성화 하는데 사용됩니다. JUnit4의 `@Ignore` 에 대응됩니다. |
| `@Timeout`               | 실행시간이 주어진 시간을 초과할 경우 실패하는 test, test factory, test template, lifecycle 메소드 테스트에 사용됩니다. |
| `@ExtendWith`            | 선언적으로 확장을 등록할 때 사용됩니다.                      |
| `@RegisterExtension`     | 필드를 통해 확장을 프로그래밍 방식으로 등록하는데 사용됩니다. |
| `@TempDir`               | `org.junit.jupiter.api.io` 패키지에 위치한 생명주기 메소드 또는 테스트 메소드에서 필드 인젝션 또는 파라미터 인젝션 임시 디렉토리를 제공하는데 사용됩니다. |

## Assertions

```java
import static java.time.Duration.ofMillis;
import static java.time.Duration.ofMinutes;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTimeout;
import static org.junit.jupiter.api.Assertions.assertTimeoutPreemptively;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.concurrent.CountDownLatch;

import example.domain.Person;
import example.util.Calculator;

import org.junit.jupiter.api.Test;

class AssertionsDemo {

    private final Calculator calculator = new Calculator();

    private final Person person = new Person("Jane", "Doe");

    @Test
    void standardAssertions() {
        assertEquals(2, calculator.add(1, 1));
        assertEquals(4, calculator.multiply(2, 2),
                "The optional failure message is now the last parameter");
        assertTrue('a' < 'b', () -> "Assertion messages can be lazily evaluated -- "
                + "to avoid constructing complex messages unnecessarily.");
    }

    @Test
    void groupedAssertions() {
        // In a grouped assertion all assertions are executed, and all
        // failures will be reported together.
        assertAll("person",
            () -> assertEquals("Jane", person.getFirstName()),
            () -> assertEquals("Doe", person.getLastName())
        );
    }

    @Test
    void dependentAssertions() {
        // Within a code block, if an assertion fails the
        // subsequent code in the same block will be skipped.
        assertAll("properties",
            () -> {
                String firstName = person.getFirstName();
                assertNotNull(firstName);

                // Executed only if the previous assertion is valid.
                assertAll("first name",
                    () -> assertTrue(firstName.startsWith("J")),
                    () -> assertTrue(firstName.endsWith("e"))
                );
            },
            () -> {
                // Grouped assertion, so processed independently
                // of results of first name assertions.
                String lastName = person.getLastName();
                assertNotNull(lastName);

                // Executed only if the previous assertion is valid.
                assertAll("last name",
                    () -> assertTrue(lastName.startsWith("D")),
                    () -> assertTrue(lastName.endsWith("e"))
                );
            }
        );
    }

    @Test
    void exceptionTesting() {
        Exception exception = assertThrows(ArithmeticException.class, () ->
            calculator.divide(1, 0));
        assertEquals("/ by zero", exception.getMessage());
    }

    @Test
    void timeoutNotExceeded() {
        // The following assertion succeeds.
        assertTimeout(ofMinutes(2), () -> {
            // Perform task that takes less than 2 minutes.
        });
    }

    @Test
    void timeoutNotExceededWithResult() {
        // The following assertion succeeds, and returns the supplied object.
        String actualResult = assertTimeout(ofMinutes(2), () -> {
            return "a result";
        });
        assertEquals("a result", actualResult);
    }

    @Test
    void timeoutNotExceededWithMethod() {
        // The following assertion invokes a method reference and returns an object.
        String actualGreeting = assertTimeout(ofMinutes(2), AssertionsDemo::greeting);
        assertEquals("Hello, World!", actualGreeting);
    }

    @Test
    void timeoutExceeded() {
        // The following assertion fails with an error message similar to:
        // execution exceeded timeout of 10 ms by 91 ms
        assertTimeout(ofMillis(10), () -> {
            // Simulate task that takes more than 10 ms.
            Thread.sleep(100);
        });
    }

    @Test
    void timeoutExceededWithPreemptiveTermination() {
        // The following assertion fails with an error message similar to:
        // execution timed out after 10 ms
        assertTimeoutPreemptively(ofMillis(10), () -> {
            // Simulate task that takes more than 10 ms.
            new CountDownLatch(1).await();
        });
    }

    private static String greeting() {
        return "Hello, World!";
    }

}
```

`fail([message], [Throwable cause])`: 테스트를 실패하고, 메시지를 입력했다면 메시지를 보여줍니다. 개발이 완료되지 않았을 경우 사용하면 좋습니다.

`assertTrue(condition, [message])`: condition이 true이면 성공하고, false이면 실패합니다. `BooleanSupplier` 를 사용하여 람다식을 사용하는 테스트도 작성할 수 있습니다.

`assertFalse(condition, [message])`: assertTrue와 동작원리는 동일합니다.

`assertNull(actual, [message])`: actual이 Null이면 성공하고, null이 아니면 실패합니다.

`asssertNotNull(actual, [message])`: assertNull과 동작원리는 동일합니다.

`assertEquals(expected, actual, [message])`: 대상 결과가 일치하는지 여부를 판단합니다. message는 필수는 아닙니다, 하지만, 실패시 정보를 표시해주는 편이 좋기 때문에 message를 입력해주는 것이 좋습니다.
부동 소수점은 세번째 인자가 delta가 될 수도 있으며, delta값 이내라면 테스트는 성공합니다.

직접 생성한 클래스는 equals 메소드를 오버라이딩 해서 비교할 수도 있습니다.

`assertArrayEquals(expected, actual, [message])`: 대상 배열이 일치하는지 여부를 판단합니다.
내부적으로 equals를 사용하여 비교하기 때문에, 직접 생성한 클래스는 equals 메소드를 오버라이딩 해서 비교할 수도 있습니다.

`assertIterableEquals(expected, actual, [message])`: 대상 클래스가 Iterable의 구현체인 경우 비교합니다.
Collection을 구현하면 Iterable의 구현체이므로 Collection을 비교하려면 이 메소드를 사용하면 됩니다.

expected와 actual이 null이면 테스트는 성공합니다.

`assertLinesMatch(List<String> expectedLines, List<String> actualLines, [message])`:
assertLinesMatch는 다음과 같은 알고리즘을 따릅니다.

1. expectedLine이 actualLine과 같은지 비교합니다.
2. 정규식으로 비교합니다(`matches` 사용).
3. `>>` 라는 fast-forward marker를 만나면 건너뜁니다. (적어도 4자리가 넘어야 합니다. `>>>>` 혹은 `>> any string >>` 과 같은 형태여야 합니다.) `>> 21 >>` 을 사용하면 21라인을 건너뛰게 됩니다.

log나 prompt 출력을 비교할 때 유용하게 사용할 수 있습니다.

`assertNotEquals(unexpected, actual, [message])`: 대상 결과가 불일치하는지 여부를 판단합니다.

`assertSame(expected, actual, [message])`: 대상 객체가 동일한 객체를 참조하는지 확인합니다.

`assertNotSame(unexpected, actual, [message])`: 대상 객체가 동일하지 않은 객체를 참조하는지 확인합니다.

`assertAll(executables)`: executables에 실행할 테스트를 그룹화 해서 실행합니다. 내부의 테스트가 블랙리스트 예외(OutOfMemoryError와 같은)를 던지지 않는 이상 실행합니다. 람다식을 넣어줄 수도 있습니다.

heading이라는 String을 넣어줄 수 있는데 이를 넣어주면, 예외가 발생했을 경우 지정한 메시지가 header가 됩니다.

`assertThrows(exceptionClass, executable, [message])`: executable 실행 중 발생한 예외를 Throwable로 return해줍니다. 또한 해당 예외가 발생하지 않거나, 다른 예외가 발생할 경우 실패합니다.

`assertDoesNotThrow(executable, [message])`: executable 실행중 에러가 발생하면 실패합니다.

`assertTimeout(timeout, executable, [message])`: timeout을 지정해줄 수 있습니다. 이 때, Duration형으로 지정해야 합니다. ofSeconds나 Duration의 생성자를 사용해주면 됩니다.

해당 시간 이내에 성공하면 성공, 그 외의 경우 실패합니다. 단, 실패하더라도 중단되지 않습니다.

`assertTimeoutPreemptively` 를 사용하면, 시간 초과시 중단됩니다.

또한, 예외를 받아서 리턴할 수도 있습니다.

## Assumption

특정 조건을 만족시키는 경우 다음 코드를 수행합니다.

실패시 테스트를 중단합니다. assume은 fail이 되더라도 error를 발생시키지 않습니다.

---

[공식문서](https://junit.org/junit5/docs/current/user-guide/) 를 읽어주세요. 🥺

저도 잘 모르기 때문에 같이 공부해보는 것도 좋을 것 같습니다!

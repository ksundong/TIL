# 11주차: 열거형

## 학습할 것

- [enum 정의하는 방법](#enum-정의하는-방법)
- [enum이 제공하는 메소드(values()와 valueOf())](#enum이-제공하는-메소드values와-valueof)
- [java.lang.Enum](#javalangenum)
- [EnumSet](#enumset)

## enum 정의하는 방법

`enum` 타입은 변수가 미리 정의 된 상수들의 집합이 되도록 하는 특별한 데이터 타입입니다. 변수는 사전에 정의 된 값 중 하나와 같아야 합니다.

일반적인 열거형의 예로는 나침반의 방향과 같은 (NORTH, SOUTH, EAST, WEST)나 요일이 있습니다.

일반적으로 `enum`의 필드 이름은 상수이기 때문에 대문자로 작성하는 것이 컨벤션입니다.

자바에서는 `enum` 타입을 선언할 때, `enum` 키워드를 사용해서 정의합니다. 예를 들어 요일을 나타내는 `Day`라는 열거형을 만들어보겠습니다.

```java
public enum Day {
  SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
```

고정된 상수들의 집합을 표현할 때면 언제나 열거형을 사용하는 것이 좋습니다. 이는 태양계의 행성과 같은 자연적인 열거형들과 메뉴의 선택지, 커맨드 라인 플래그 등과 같은 컴파일 타임에 알 수 있는 모든값들의 집합을 포함합니다.

### enum의 활용

enum을 정의했다면, 활용할 수 있어야 합니다. 아래의 예제 코드는 어떻게 enum을 다른 코드에서 사용할 수 있는지 설명합니다.

```java
public class EnumTest {
  Day day;

  public EnumTest(Day day) {
    this.day = day;
  }

  public void tellItLikeItIs() {
    switch (day) {
      case MONDAY:
        System.out.println("월요일은 힘들어");
        break;
      case FRIDAY:
        System.out.println("금요일은 좋아");
        break;
      case SATTURDAY: case SUNDAY:
        System.out.println("주말이 최고야!");
        break;
      default:
        System.out.println("주중엔 그냥 그래");
        break;
    }
  }

  public static void main(String[] args) {
    EnumTest firstDay = new EnumTest(Day.MONDAY);
    firstDay.tellItLikeItis();
    EnumTest thirdDay = new EnumTest(Day.WEDNESDAY);
    thirdDay.tellItLikeItis();
    EnumTest fifthDay = new EnumTest(Day.FRIDAY);
    fifthDay.tellItLikeItis();
    EnumTest sixthDay = new EnumTest(Day.SATURDAY);
    sixthDay.tellItLikeItis();
    EnumTest seventhDay = new EnumTest(Day.SUNDAY);
    seventhDay.tellItLikeItis();
  }
}
```

결과

```text
월요일은 힘들어
주중엔 그냥 그래
금요일은 좋아
주말이 최고야!
주말이 최고야!
```

사실 열거형은 클래스 정의와 같습니다. 따라서 열거형 클래스 바디에는 메서드와 기타 필드들이 포함될 수 있습니다. 그리고 컴파일러가 자동으로 몇 가지 특수 메서드를 추가해줍니다.

그리고 모든 열거형들은 암묵적으로 `java.lang.Enum` 클래스를 상속합니다. 따라서 자바 언어 스펙 상 다중 상속을 지원하지 않아 한 클래스가 다른 클래스를 상속하고 있다면, 또 다른 클래스는 상속할 수 없으므로, 열거형은 다른 클래스를 상속할 수 없습니다.

자바에서는 필드 또는 메서드보다는 먼저 상수를 정의해야 합니다. 그리고 이런 필드와 메서드가 있는 경우 열거형 상수 목록은 세미콜론으로 끝나야 합니다.

enum의 생성자는 package-private 또는 private 접근만 가능합니다. 이는 자동으로 enum body 시작 부분에 정의된 상수를 생성합니다. 또, 열거형의 생성자는 직접 호출할 수 없습니다.

또한 enum에는 여러 메서드를 정의할 수 있으므로, 이를 활용해서 객체지향의 특성을 더더욱 살리는 코드를 작성할 수도 있습니다.

### enum 사용시 주의할 점

enum에는 `ordinal()` 이라는 메서드가 제공됩니다. 이는 현재 상수가 Enum에서 차지하고 있는 위치를 나타냅니다.

이는 소스 코드에서의 순서가 변경되면, enum의 ordinal 값 또한 변경된다는 것을 의미합니다. 따라서 해당 조건으로 처리하는 코드를 만들었다면 문제가 발생할 수 있으므로, 좋지 않습니다.

이는 javadoc을 확인해보면 더 확실히 알 수 있습니다. 이는 `EnumSet` 또는 `EnumMap`과 같은 열거형을 기반으로하는 데이터 타입에서 사용되도록 설계되었음을 알 수 있습니다.

> Most programmers will have no use for this method. It is designed for use by sophisticated enum-based data structures, such as EnumSet and EnumMap.

<https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Enum.html#ordinal()>

또한 이를 방지하기 위해서는 누구나 납득할 수 있는 논리적인 순서로 배치해야합니다.

## enum이 제공하는 메소드(values()와 valueOf())

- `values()`: **선언된 순서**대로 열거형의 모든 값을 포함하는 배열을 반환하는 `static` 메서드  
   일반적으로 `for-each` 구문과 함께 사용되어 열거형의 값을 반복합니다.  
   아래의 예제는 태양계의 행성들을 표현하는 `Planet` 클래스들을 반복하는 코드입니다.

  ```java
  for (Planet p : Planet.values()) {
    System.out.printf("이 행성 %s의 무게는 %f%n 입니다.", p, p.surfaceWeight(mass));
  }
  ```

  이 예에서는 `Planet`이 태양계의 행성을 나타내는 열거형이라고 가정했습니다. 행성들은 일정한 질량 및 반경이라는 속성으로 정의되어있습니다.  
   각 열거형 상수는 정의 될 때, 질량과 반경을 매개 변수 값으로 받습니다. 이러한 값은 상수가 생성될 때, 생성자에 전달됩니다.

## java.lang.Enum

## EnumSet

## 참고

[오라클 자바 튜토리얼(열거형)](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)

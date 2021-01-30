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

- `valueOf()`: 두 가지 종류의 `valueOf` 메서드가 있습니다. 하나는 `Enum` 클래스에서도 사용 가능한 static 메서드이고 이는 인자로 `Class` 타입과 `String`으로 표현된 상수명을 받아 해당 상수를 반환합니다. 또 다른 하나는 정의된 열거형 타입에서만 사용가능하며 인자로 `String`으로 표현된 상수명을 받아 해당 상수를 반환합니다.  
   존재하지 않는 상수를 입력한 경우 `IllegalArgumentException`이 발생하게 됩니다.

## java.lang.Enum

`java.lang.Enum`은 모든 Java 열거형의 기본 타입입니다. `Enum` 클래스는 추상클래스여서 `Enum` 클래스의 객체를 만들 수는 없습니다.

`Enum` 클래스는 기본적으로 제공하는 메서드들이 있습니다. 대부분 `Object` 클래스에서 정의된 메서드를 재정의하며, 이런 메서드들 중 일부는 `final`로 선언되어 다시 재정의 할 수 없게끔 만들어두었습니다.

### 기본 제공 메서드들

- `final String name()` 메서드는 상수의 이름을 반환합니다.
- `final int ordinal()` 메서드는 위에서도 설명했듯 상수의 열거형에서의 순서를 반환합니다.
- `String toString()` 메서드는 열거형 상수의 문자열 표현을 반환합니다. 재정의 가능합니다.
- `final boolean equals(Object obj)` 메서드는 매개변수로 전달된 객체가 열거형 상수와 같다면 `true`를 반환하고, 그렇지 않다면 `false`를 반환합니다.
- `final int hashCode()` 메서드는 이 열거형 상수에 대한 해시 코드를 반환합니다. 실제 구현은 `super.hashCode()`를 호춣합니다.
- `final int compareTo(E obj)` 메서드는 열거형의 순서를 비교합니다. 순서가 낮은 경우 음수, 같은 경우 0, 높은 경우 양수를 반환합니다.
- `final Class <E> getDeclaringClass()` 메서드는 열거형 상수의 타입에 해당하는 `Class` 객체를 반환합니다.
- `final Object clone()` 메서드는 열거형이 복제되지 않도록 보장하며, 단일 상태임을 보장해줍니다. 사용시 `CloneNotSupportedException`이 발생합니다. 열거형 상수를 만들기 위해 컴파일러 내부적으로 사용된다고 합니다.
- `final void finalize()` 메서드는 enum 클래스가 `finalize` 메서드를 가질 수 없음을 보장해줍니다.

### 참고

[Geeks for Geeks(Enum 클래스)](https://www.geeksforgeeks.org/java-lang-enum-class-java/)

## EnumSet

`EnumSet`은 `enum`과 함께 동작하는 특별한 `Set`입니다.  
`EnumSet`은 `Set` 인터페이스를 구현하면서 `AbstractSet`를 상속하지만, 대부분의 메서드를 재정의해서 사용합니다.

`EnumSet`을 사용할 때 유의해야할 점이 있습니다.

- `EnumSet`에는 열거형 값만 저장할 수 있습니다. 그리고 모든 값은 같은 열거형에 속해야합니다.
- `EnumSet`에는 `null`을 추가할 수 없습니다. 만약 추가하려고 한다면 `NullPointerException`이 발생하게 됩니다.
- `EnumSet`은 thread-safe하지 않습니다. 만약 동기화가 필요하다면, 외부에서 동기화 처리를 해주어야 합니다.
- 요소들은 열거형에 정의된 순서에 따라 저장됩니다.
- `EnumSet`은 복사시에 실패에 안전한 반복자를 사용하므로 컬렉션이 반복되는 도중에 변경되어도 `ConcurrentModificationException`이 발생하지 않습니다.

### EnumSet을 왜 쓸까?

EnumSet은 왜 쓸까요? 저자의 경험상 `EnumSet`은 `enum` 값을 저장할 때, 다른 `Set` 구현체보다 먼저 고려되어야 한다고 합니다.

그럼 Java에서 `EnumSet`은 어떻게 구현되어 있을까요? 한 번 뜯어봅시다.

```java
public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
  Enum<?>[] universe = getUniverse(elementType);
  if (universe == null)
    throw new ClassCastException(elementType + " not an enum");

  if (universe.length <= 64)
    return new RegularEnumSet<>(elementType, universe);
  else
    return new JumboEnumSet<>(elementType, universe);
}
```

`noneOf()`라는 메서드는 많은 부분에서 사용되고 있습니다. 핵심 연산이라고 볼 수 있겠습니다. 또 특이한 점은 `RegularEnumSet`과 `JumboEnumSet`이 `universe`의 `length` 속성으로 구분이 된다는 점입니다. 그것도 64라는 특징적인 숫자로요.

`RegularEnumSet`은 `long`의 `bit`를 기준으로 `long` 하나로 충분히 표현가능한 경우 사용됩니다. `JumboEnumSet`은 그 이상의 경우에 사용됩니다. 이렇게 사용하는 이유는 각 비트가 현재 열거형에서의 위치를 반영하기 때문에 해당 값이 존재하는 지 아닌지 파악하는 것이 매우 빠르기 때문입니다.

주의할 점은 저장할 데이터의 수가 아닌 열거형에 정의된 상수의 개수만을 고려한다는 점입니다.

따라서 이 구현 때문에 `EnumSet`의 모든 메서드는 산술 비트 연산을 사용해서 구현됩니다. 이는 각 세부 구현체에 가면 자세하게 작성되어 있습니다. 또한 산술 비트 연산의 연산 속도는 매우 빠르기 때문에 모든 작업들이 일정한 시간 안에 실행됩니다.

이는 `HashSet` 같은 다른 `Set` 구현체보다 빠르다고 볼 수 있습니다. 올바른 데이터를 찾기 위해 `hashCode`를 찾을 필요조차 없습니다.

또한 비트 연산 자체의 특성으로 인해서 굉장히 압축적이고 효율적입니다. 이 때문에 메모리를 적게 차지합니다. 이 또한 장점이라고 볼 수 있습니다.

### EnumSet의 연산

EnumSet은 인스턴스를 생성하는 메서드를 제외하고는 대부분은 다른 `Set`과 동일하게 동작합니다.

예제로 색상과 관련된 `Enum`을 정의해보겠습니다.

```java
public enum Color {
  RED, YELLOW, GREEN, BLUE, BLACK, WHITE
}
```

#### 생성 메서드

`allOf()`와 `noneOf()`가 바로 `EnumSet`을 생성하는 간단한 메서드입니다.

`allOf()`는 해당 `Enum`의 모든 요소들을 포함하는 `EnumSet`을 만듭니다.

```java
EnumSet.allOf(Color.class);
```

`noneOf()`는 `allOf()`와 반대의 동작을 합니다. 비어있는 `EnumSet`을 만듭니다.

```java
EnumSet.noneOf(Color.class);
```

그리고 `EnumSet`이 `Enum`의 하위 집합 형태로 만들어져야 할 경우, 오버로딩된 `of()` 메서드들을 이용할 수 있습니다.

이는 고정된 수를 정의하는 5개의 메서드와 varargs로 정의하는 메서드로 나뉘어집니다.

이렇게 한 이유는 사실 varargs가 내부적으로 배열을 생성하기 때문에 느려진다고 자바독에서 설명하고 있습니다.

또 다른 방법으로는 `range()` 메서드를 사용하는 것입니다. 이는 해당 상수들의 범위의 enum이 모두 등록됩니다. 이 순서는 Enum에 정의된 순서를 따릅니다.

그리고 `complementOf()`라는 메서드도 있습니다. 이 메서드는 해당 EnumSet의 여집합을 구하는 메서드입니다.

그리고 `copyOf()`라는 메서드는 다른 EnumSet을 복사해오는 메서드입니다. 위에서 설명했듯 안전합니다. 내부적으로 `clone()`메서드를 호출한다고 합니다. 이는 컬렉션이 `enum`을 포함하고 있다면 똑같이 사용할 수 있습니다.

#### 기타 메서드

다른 메서드는 다른 `Set` 구현체들과 동일하게 동작하며, 사용상에 차이는 없습니다.

`add`, `contains`, `forEach`, `remove` 등을 사용할 수 있습니다.

### 참고

[밸덩 EnumSet](https://www.baeldung.com/java-enumset)

## 참고

[오라클 자바 튜토리얼(열거형)](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)


---

## 라이브스터디

- `values()`는 컴파일 시점에 삽입된다.

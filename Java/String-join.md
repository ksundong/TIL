# String Join 사용법

Java8 부터는 문자열을 합칠 수 있는 여러가지 방식을 지원합니다.

## StringJoiner

`java.util` package에 속하는 유틸리티 클래스입니다.

```java
public void addElements() {
  StringJoiner joiner = new StringJoiner(",", PREFIX, SUFFIX);
  joiner.add("String1")
      .add("String2);
  System.out.println(joiner.toString);
}
```

","는 delimiter 즉 구분자입니다.

위와 같은 방식으로 작성된 코드는 `(PREFIX)String1,String2(SUFFIX)` 이러한 형태를 리턴하게 됩니다.  
enhanced for loop를 활용하게되면, Iterable한 Collection에 담겨있는 객체들을 손쉽게 붙일 수 있게됩니다.

`toString` 메소드를 호출하면, joiner에 담겨있던 값들을 합쳐서 String으로 반환하게 됩니다.

`setEmptyValue` 메소드를 사용해서 기본 반환 문자열을 사용할 수 있습니다.(원래는 empty String입니다.)  
Joiner 내부에 요소가 없을 때에만 반환되게 됩니다.

`merge` 메소드를 사용해서 두 Joiner를 병합할 수 있습니다.

```java
@Test
public void whenMergingJoiners_thenReturnMerged() {
    StringJoiner rgbJoiner = new StringJoiner(
      ",", PREFIX, SUFFIX);
    StringJoiner cmybJoiner = new StringJoiner(
      "-", PREFIX, SUFFIX);

    rgbJoiner.add("Red")
      .add("Green")
      .add("Blue");
    cmybJoiner.add("Cyan")
      .add("Magenta")
      .add("Yellow")
      .add("Black");

    rgbJoiner.merge(cmybJoiner);

    assertEquals(
      rgbJoiner.toString(),
      "[Red,Green,Blue,Cyan-Magenta-Yellow-Black]");
}
```

여기서 주목할 점은 구분자는 기존에 있던 그대로 유지된다는 것 입니다.

Java8에서 알 수 있듯이, StringJoiner는 Stream API에서도 사용할 수 있습니다.

바로 `Collectors.joining()`이 그 주인공입니다.

### Collectors.joining()

```java
@Test
public void whenUsedWithinCollectors_thenJoined() {
    List<String> rgbList = Arrays.asList("Red", "Green", "Blue");
    String commaSeparatedRGB = rgbList.stream()
      .map(color::toString)
      .collect(Collectors.joining(","));

    assertEquals(commaSeparatedRGB, "Red,Green,Blue");
}
```

`Collectors.joining()`은 내부적으로 StringJoiner를 사용하여 join 작업을 수행합니다.

물론 StringJoiner는 매우 primitive하고, 기본적인 기능밖에 제공하지 않습니다. 이는 `Collectors`에 사용하기 위한 것으로 추정됩니다.  
더 많고 다양한 기능을 원한다면 라이브러리를 고려하는 것이 좋겠습니다.

## String join

이 역시도 Java8에서 등장한 기능입니다. 사용법도 간단합니다.

```java
String joinedString = String.join("구분자", string배열, 혹은 collection);
```

이 메소드의 가장 큰 장점은 구분자를 신경쓰지 않아도 된다는 점입니다.

## Reference

[String Joiner Baeldung](https://www.baeldung.com/java-string-joiner)

[String Concatenation Baeldung](https://www.baeldung.com/java-strings-concatenation)

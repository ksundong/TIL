# map과 flatMap의 차이

`map()`과 `flatMap()`은 함수형 언어에서 유래한 메소드입니다. Java8에서 추가된 `Stream`과 `Optional`, `CompletableFuture`에서 사용할 수 있는 API입니다.

`map()`과 `flatMap()`은 리턴하는 타입은 같지만, 조금 다릅니다.

## Optional에서 `map()`과 `flatMap()`의 차이

`map()`은 인자로 참조형의 객체를 받습니다. 반환하는 것은 `Optional`로 Wrapping된 객체를 반환합니다. 따라서, 내부적으로 중첩된 구조가 됩니다.

```java
Optional<String> s = Optional.of("test");
assertEquals(Optional.of("TEST"), s.map(String::toUpperCase));
```

`flatMap()`은 인자로 `Optional`로 Wrapping된 객체를 받습니다. 반환할 때는 `map()`과 달리 따로 Wrapping을 하지 않습니다. 이 것을 사용하는 경우는 이미 `Optional`로 Wrapping이 되어 추가적인 Wrapping 작업이 불필요할 경우 사용하게 됩니다.

```java
assertEquals(Optional.of("STRING"), Optional
  .of("string")
  .flatMap(s -> Optional.of("STRING")));
```

## Stream에서 `map()`과 `flatMap()`의 차이

두 메소드는 `Optional`에서와 비슷하게 동작합니다.

`map()`은 `Stream` 인스턴스로 Wrapping하고, `flatMap()`은 중첩된 `Stream`구조를 피하기 위해 사용합니다.

```java
List<String> myList = Stream.of("a", "b")
  .map(String::toUpperCase)
  .collect(Collectors.toList());
assertEquals(asList("A", "B"), myList);
```

```java
List<List<String>> list = Arrays.asList(
  Arrays.asList("a"),
  Arrays.asList("b"));
System.out.println(list);

// 위와같은 구조의 중첩 Collection의 경우 flatMap을 사용하여 flat한 구조로 만들어줄 수 있습니다.

System.out.println(list)
  .stream()
  .flatMap(Collection::stream)
  .collect(Collectors.toList()));
```

즉 Nested Collection을 Flatten Collection으로 만들어낼 때, flatMap을 사용하면 좀 더 간단하게 할 수 있음을 알 수 있습니다.

## References

[Baeldung The Difference with map and flatMap](https://www.baeldung.com/java-difference-map-and-flatmap)

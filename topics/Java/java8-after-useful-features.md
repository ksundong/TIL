# Java8 이후 추가된 유용한 기능들

1. `var` 지역 변수 타입 추론(Java9)
2. `Stream API`에 추가된 유용한 기능: `takeWhile()`, `dropWhile()`  
   `takeWhile()` 은 해당 요소를 만나면 멈추고 이전 요소들을 반환하고, `dropWhile()`은 이후 요소를 반환하는 것으로 보입니다.  
   정렬을 통해서 'deterministic'한 결과를 얻으라는 팁도 있고, 병렬 스트림에서 사용시 성능이 저하되니 독립 스트림에서 사용하는 것을 권장합니다. (Java9)
3. `switch표현식` (switch 표현식은 값을 할당할 때 주로 사용하고, switch문은 로직을 수행할 때 주로 사용) (Java12)
4. `records` Kotlin의 `data class`와 유사한 기능인 것으로 보입니다. 내용은 `getter`, `setter`, `equals()`, `hashCode()`, `toString()` 재정의가 필요 없고, 불변이며, 필드명으로 접근할 수 있습니다.(Accessor methods)  
   다른 클래스가 상속은 할 수 없고, record가 interface를 구현할 수는 있습니다. (Java14)
5. `text block` `"""` 로 사용하는 텍스트 블록이 추가되었습니다.(Java13에 소개되었고 Java15에 추가되었음)

추후 관련 코드를 추가할 생각입니다.

## References

- [Oracle Blog](https://blogs.oracle.com/javamagazine/modern-java-toys-that-boost-productivity-from-type-inference-to-text-blocks#anchor_5)

# 동등성과 동일성

Java에서 Object를 비교하는 방법은 두 가지 방법이 있습니다.

- 서로 다른 객체의 값이 같은지 동등성을 비교하는 것
- 객체 둘이 같은 객체인지 동일성을 비교하는 것

`eqauls()` 메소드를 오버라이딩 하지 않으면 기본적으로 Object는 동일성 비교를 합니다.

`==`은 동일성 비교 연산자, `equals()`는 동등성 비교 연산자입니다.

## 주의할 점

동등성을 비교할 때, 이 객체가 서로 같은지를 명확히 비교할만한 조건이 있는 경우.  
그 값을 제외하고 부가적인 값을 비교하지 않도록 하는 편이 오류를 줄일 수 있습니다.

## 테스트 코드 작성시 참고사항

AssertJ의 `.isEqualToComparingFieldByField()` 를 사용하면 `equals()` 오버라이딩 없이 비교가 가능합니다.

그 밖에도 여러가지 옵션들이 있습니다. (해당하는 필드만 비교, 해당하는 필드만 제외, null 무시 등)

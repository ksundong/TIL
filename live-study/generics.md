# 14주차: 제네릭

## 학습할 것

- [재네릭 사용법](#재네릭-사용법)
- [제네릭 주요 개념(바운디드 타입, 와일드 카드)](#제네릭-주요-개념바운디드-타입-와일드-카드)
- [제네릭 메소드 만들기](#제네릭-메소드-만들기)
- [Erasure](#erasure)

## 제네릭 타입

제네릭 타입이란 제네릭 클래스 또는 인터페이스를 매개 변수처럼 사용하는 것입니다.

제네릭 타입이 필요한 이유를 알기 위해 `Box`라는 간단한 클래스를 예를들어 설명해보겠습니다.

이 `Box`라는 클래스는 `set`, `get`이라는 메서드를 제공하며, 그 기능은 그냥 Box에 무엇인가 담고 꺼내는 것을 생각하면 됩니다.

```java
public class Box {

  private Object object;

  public void set(Object object) {
    this.object = object;
  }

  public Object get() {
    return object;
  }
}
```

이 메서드는 `Object`인 무언가를 받습니다. 우리가 무엇을 넣기를 원하던간에 기본 타입만 아니면 어떤 타입이든 받고 꺼낼 수 있습니다. 컴파일 타임에서는 여기에 들어올 수 있는 것이 무엇인지 특정하기 힘들며, 어떤 코드에서는 `Integer`형의 값을 넣어줬지만, 다른 코드에서는 `String`형의 값을 꺼내려고 할 때, 문제가 발생할 수도 있습니다.

그럼 제네릭이 적용된 `Box` 클래스를 봅시다.

제네릭 클래스는 먼저 다음과 같은 형태로 정의할 수 있습니다.

```
class name<T1, T2, ..., Tn> { /* ... */ }
```

타입 파라미터 영역은 `class` 다음의 꺽쇠 괄호(`<>`)로 구분됩니다. 이는 타입 파라미터를 정의하는 역할을 합니다.

```java
public class Box<T> {
  private T t;

  public void set(T t) {
    this.t = t;
  }

  public T get() {
    return t;
  }
}
```

`Box`를 제네릭을 적용하면 위와같은 코드가 됩니다. 제네릭의 장점은 첫번째로, 특정한 타입을 컴파일 타임에 정의해줄 수 있습니다. (타입 안정성) 두번째로, 컴파일타임에 이를 알 수 있기 때문에, 개발자의 실수가 없어지게 됩니다.

### 타입 파라미터의 네이밍 규칙

관례적으로, 타입 파라미터의 이름은 단일 대문자로 작성합니다. 이것은 다른 명명규칙과 다르며, 이는 일반적인 명명규칙과 동일할 경우, 클래스와 인터페이스의 정의와 헷갈릴 여지가 있기 때문입니다.

대표적인 타입 파라미터는 다음과 같습니다.

- E - Element(보통 컬렉션 프레임워크에서 많이 사용)
- K - Key
- N - Number
- T - Type
- V - Value
- S, U, V etc - 2nd, 3rd, 4th types

위의 네이밍은 자바 SE 버전에서 많이 볼 수 있습니다.

### 참고

- [오라클 자바 튜토리얼(제네릭 타입)](https://docs.oracle.com/javase/tutorial/java/generics/types.html)

## 재네릭 사용법

## 제네릭 주요 개념(바운디드 타입, 와일드 카드)

## 제네릭 메소드 만들기

## Erasure
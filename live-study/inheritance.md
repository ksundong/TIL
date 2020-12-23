# 6주차: 상속

## 학습할 것

- [자바 상속의 특징](#자바-상속의-특징)
- [super 키워드](#super-키워드)
- [메소드 오버라이딩](#메소드-오버라이딩)
- [다이나믹 메소드 디스패치(Dynamic Method Dispatch)](#다이나믹-메소드-디스패치dynamic-method-dispatch)
- [추상 클래스](#추상-클래스)
- [final 키워드](#final-키워드)
- [Object 클래스](#object-클래스)

## 자바 상속의 특징

자바의 상속은 다른 클래스에서 `derived`(유래, 파생)될 수 있습니다. 이것을 상속을 한다고 하며, 해당 클래스의 필드와 메서드를 `inheriting`(상속)할 수 있습니다.

이렇게 다른 클래스에서 `derived`된 클래스를 보통 서브클래스(확장된 클래스, 자식 클래스, derived 클래스)라고 부릅니다. 그리고 그 대상이 된 클래스를 슈퍼클래스(base 클래스 또는 부모 클래스)라고 부릅니다.  
`Object` 클래스를 제외한 모든 클래스는 하나의 슈퍼클래스를 가집니다. 이를 단일 상속이라고 표현합니다. 즉, 자바는 **단일 상속**만을 지원합니다.  
명시적으로 슈퍼클래스를 지정해주지 않는다면, 모든 클래스의 암시적인 슈퍼클래스는 `Object` 클래스가 됩니다.

클래스들은 궁극적으로 최상위 클래스 `Object`에서 파생된 클래스에서 계속해서 파생될 수 있습니다.(상속의 횟수에 제한을 두지 않습니다.) 따라서 모든 클래스는 `Object`의 자손이라고 볼 수 있게됩니다.

상속의 개념자체는 간단하지만, 강력하게 사용할 수 있습니다. 예를 들어, 새 클래스를 만들고 싶지만 원하는 코드가 포함된 클래스가 이미 있는 경우 이 클래스를 확장해서 사용하기를 원할 수 있습니다.  
이렇게 한다면, 이 코드들을 직접 작성하거나, 디버깅 하지 않더라도 재사용할 수 있습니다. (객체지향에서 중요하게 보는 것 중 하나가 바로 코드의 재사용성이죠.)

서브클래스는 슈퍼클래스의 모든 멤버(필드, 메서드, 중첩 클래스)를 상속합니다. 생성자는 멤버가 아니므로, 상속되지 않습니다. 그러나 슈퍼클래스의 생성자는 서브클래스에서 실행될 수 있습니다. `super()`가 그 예입니다.

### 요약

1. Java는 단일 상속만 지원합니다. 다중 상속은 지원하지 않습니다.
2. Java는 상속 횟수에 제한을 두지 않습니다.
3. Java는 `Object` 클래스를 제외한 클래스는 모두 부모 클래스가 있어야 하며, 명시적으로 지정해주지 않은 경우 `Object` 클래스를 상속하게 됩니다.

### 참고

[오라클 자바 튜토리얼(상속)](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

## super 키워드

`super` 키워드는 서브클래스에서 슈퍼클래스를 가리키기 위해 사용하는 키워드입니다.

`super` 키워드는 슈퍼클래스의 멤버에 접근하기 위해서 사용할 수 있습니다. 메서드 오버라이딩을 할 때, 슈퍼클래스의 메서드를 호출해서 사용할 수도 있습니다. super를 사용해서 필드에 접근을 할 수도 있으나 권장되지 않습니다.
`super` 키워드를 통해서 서브클래스는 부모 클래스의 생성자를 호출할 수도 있습니다.

### 슈퍼클래스 멤버에 접근

위의 설명을 코드로 보여드리겠습니다.

아래의 코드는 슈퍼클래스가 될 클래스인 `Superclass` 입니다. 이 클래스는 `printMethod`라는 메서드를 가지고 있습니다.

```java
public class Superclass {

  public void printMethod() {
    System.out.println("Printed in Superclass.");
  }
}
```

아래의 코드는 `Superclass`의 서브클래스가 될 클래스 `Subclass`입니다. 이 클래스는 `printMethod()`를 오버라이딩 하고 있습니다.

```java
public class Subclass extends Superclass {

  @Override
  public void printMethod() {
    super.printMethod(); // super 키워드로 슈퍼클래스의 멤버 메서드에 접근할 수 있습니다.
    System.out.println("Printed in Subclass.");
  }

  public static void main(String[] args) {
    Subclass s = new Subclass();
    s.printMethod();
  }
}
```

기본적으로 상속을 하면 슈퍼클래스의 멤버 중 `private`, `default`가 아닌 멤버들을 참조할 수 있습니다. 만약 슈퍼클래스의 메서드와 동일한 시그니처를 가진 메서드를 선언한다면 이를 오버라이딩이라고 하며 재정의한다고 표현합니다. 이렇게 하면, 메서드의 동작을 변경할 수 있습니다. (매개변수와 리턴타입은 변경 불가)

주로 오버라이딩 할 때는 컴파일러에서 컴파일 타임에 체크할 수 있도록 하기 위해 `@Override` 애노테이션을 붙여주는 것이 좋습니다. 오타를 쳐서 실수를 하는 경우를 방지해줍니다.

그리고 이렇게 오버라이딩 된 메서드는 슈퍼클래스의 메서드를 가리기 때문에 `super`라는 키워드로 호출을 해야합니다.

위의 코드를 컴파일하고 실행하면 다음과 같이 출력됩니다.

```text
Printed in Superclass.
Printed in Subclass.
```

객체지향의 원칙중에서 LSP(Liskov Substitution Principle)이 있는데, 오버라이딩을 하면서 이 원칙이 깨지도록 하면 안됩니다. (이 원칙은 상위 타입의 객체를 하위 타입의 객체로 치환하더라도 상위 타입을 사용하는 다른 코드는 이전과 동일하게 동작해야 한다는 원칙입니다.)

### 서브클래스 생성자에서 접근

`super` 키워드는 서브클래스 생성자에서 슈퍼클래스의 생성자를 호출하는데도 사용할 수 있습니다.

```java
public MountainBike (int startHeight, int startCadence, int startSpeed, int startGear) {
  super(startCadence, startSpeed, startGear);
  seatHeight = startHeight;
}
```

위 코드를 보면 MountainBike의 생성자는 슈퍼클래스의 생성자를 호출해서 값을 초기화 함을 알 수 있습니다.

슈퍼클래스의 생성자를 호출하는 것은 서브클래스 생성자의 맨 첫번째 줄에 입력해야합니다. 슈퍼클래스의 생성자를 호출하는 문법은

```java
super();
```

또는

```java
super(parameter list);
```

입니다. `super()`를 호출하면 슈퍼클래스의 인자없는 생성자가 호출됩니다. `super(parameter list)`를 호출하면, 해당 파라미터 리스트에 매핑되는 생성자가 호출됩니다.

#### 알아두기

만약 슈퍼클래스 생성자가 서브클래스 생성자에서 명시적으로 수행되지 않았다면, 자바 컴파일러가 자동으로 슈퍼클래스의 인자없는 생성자를 넣어줍니다. 만약 슈퍼클래스가 인자없는 생성자를 가지고 있지 않다면 컴파일 에러가 발생합니다. `Object`는 해당 생성자가 있습니다. 만약 `Object`가 유일한 슈퍼클래스라면 아무런 문제도 없을 것입니다.

만약, 서브클래스의 생성자가 슈퍼클래스의 생성자를 실행한다면, `Object`의 생성자까지 호출되는 생성자의 체인이 있을 것이라고 생각할 겁니다. 이것은 복잡한 상속관계를 다룰 때 꼭 인지할 필요가 있습니다.

### 참고

[오라클 자바 튜토리얼(super 키워드)](https://docs.oracle.com/javase/tutorial/java/IandI/super.html)

## 메소드 오버라이딩

### 인스턴스 메소드

슈퍼클래스에 있는 메서드의 시그니처와 리턴타입이 동일한 서브클래스의 메서드는 슈퍼클래스의 메서드를 재정의 할 수 있습니다.

메소드 오버라이딩은 "충분히 가까운(?)" 클래스를 상속하고 동작을 수정하는 것을 의미합니다. 메소드 오버라이딩을 할 때에는 오버라이딩 하려는 메소드의 이름, 파라미터의 수와 타입(리스트), 리턴 타입이 같아야 합니다. 오버라이딩 된 메서드는 원래 정의되어있던 리턴타입의 하위타입을 반환할 수도 있습니다. 이를 `covariant return type`이라고 합니다.

찾아보니 C#은 `covariant return type`을 지원하지 않는다고 합니다.

#### Covariance

공변으로 번역할 수 있는 Covariance는 쉽게 말해서 슈퍼타입이 정의되었을 때, 서브타입을 허용한다고 약속하는 것입니다. 이는 리스코프 치환 원칙과도 관계가 있다고 합니다.

즉 상위 타입으로 정의된 요소에 하위 타입을 사용해도 괜찮다는 것입니다.

#### Covariant Return Type

이 공변 반환 타입은 우리가 메서드를 오버라이딩 할 때, 오버라이딩 한 메서드의 리턴타입의 서브타입을 반환하는 것을 허용한다는 것입니다.

예제코드로 설명해드리겠습니다. 간단한 `Producer` 클래스와 `produce()` 메서드가 있습니다. 기본적으로 `String`을 매개변수로 받고, 서브클래스의 유연성을 위해 `Object` 타입으로 `String`이 반환됩니다.

```java
public class Producer {
  public Object produce(String input) {
    Object result = input.toLowerCase();
    return result;
  }
}
```

`Object`가 리턴타입이므로, 우리는 좀 더 구체적인 타입을 서브클래스에서 지정할 수 있습니다. 이를 공변 반환 타입이라고 합니다.

```java
public class IntegerProducer extends Producer {
  @Override
  public Integer produce(String input) {
    return Integer.parseInt(input);
  }
}
```

#### 이를 검증하기 위한 테스트 코드 작성

공변 반환 타입의 기본적인 아이디어는 리스코프 치환 원칙을 지원하기 위함입니다.

당연하게도 리턴타입은 슈퍼클래스의 메서드의 리턴타입과 호환되는 타입만 지정가능합니다.(혹시 몰라서 해봤는데 안되더라고요.)

먼저 우리는 간단히 `Producer` 인스턴스를 생성하는 코드를 `IntegerProducer`로 교체할겁니다. 결과는 `Object` 타입으로 반환된다고 했으므로 호환이 됩니다. `Integer` 타입을 리턴한다고 해도 말이죠. 물론 위의 코드는 예제 코드이기 때문에 엄밀히 말하면 리스코프 치환 원칙의 행동이 호환되어야 함을 지키지는 않습니다.

아래의 테스트 코드를 보면, Object 타입으로 반환하지만 실제 리턴되는 인스턴스의 타입이 다르고 그 결과도 상이함을 알 수 있습니다. 하지만, 어쨌든 동작합니다.

만약, 좀 더 명시적인 타입의 반환을 원한다면, 명시적으로 `IntegerProducer`로 교체하면 됩니다.

```java
import static org.assertj.core.api.Assertions.assertThat;

import org.junit.jupiter.api.Test;

class ProducerTest {

  @Test
  void whenInputIsArbitrary_thenProducerProducesString() {
    String arbitraryInput = "just a random text";
    Producer producer = new Producer();

    Object objectOutput = producer.produce(arbitraryInput);

    assertThat(objectOutput).isEqualTo(arbitraryInput).isInstanceOf(String.class);
  }

  @Test
  void whenInputIsSupported_thenProducerCreatesInteger() {
    String integerAsString = "42";
    IntegerProducer producer = new IntegerProducer();

    Object result = producer.produce(integerAsString);

    assertThat(result).isInstanceOf(Integer.class).isEqualTo(Integer.parseInt(integerAsString));
  }

  @Test
  void whenInputIsSupported_thenIntegerProducerCreatesIntegerWithoutCasting() {
    String integerAsString = "42";
    IntegerProducer producer = new IntegerProducer();

    Integer result = producer.produce(integerAsString);

    assertThat(result).isEqualTo(Integer.parseInt(integerAsString));
  }
}
```

이 부분은 자바를 1년 넘게 알고 있었는데, 새롭게 배운 부분이라 신기했습니다.

언젠가 써먹을 일이 있었으면 좋겠습니다.

### `static` 메소드

### 인터페이스 메소드

### 제어자

### 요약

### 참고

[오라클 자바 튜토리얼(메소드 오버라이딩)](https://docs.oracle.com/javase/tutorial/java/IandI/override.html)
[밸덩 java covariant return type](https://www.baeldung.com/java-covariant-return-type)

## 다이나믹 메소드 디스패치(Dynamic Method Dispatch)

## 추상 클래스

## final 키워드

## Object 클래스

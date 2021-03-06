# 8주차 과제: 인터페이스

## 학습할 것

- [8주차 과제: 인터페이스](#8주차-과제-인터페이스)
- [인터페이스 정의하는 방법](#인터페이스-정의하는-방법)
- [인터페이스 구현하는 방법](#인터페이스-구현하는-방법)
- [인터페이스 레퍼런스를 통해 구현체를 사용하는 방법](#인터페이스-레퍼런스를-통해-구현체를-사용하는-방법)
- [인터페이스 상속](#인터페이스-상속)
- [인터페이스의 기본 메소드(Default Method), 자바 8](#인터페이스의-기본-메소드default-method-자바-8)
- [인터페이스의 static 메소드, 자바 8](#인터페이스의-static-메소드-자바-8)
- [인터페이스의 private 메소드, 자바 9](#인터페이스의-private-메소드-자바-9)

## 인터페이스 정의하는 방법

일반적으로 소프트웨어 개발에서 "약속"은 프로그래머들의 소프트웨어간의 소통에 있어서 중요합니다. 각각의 소프트웨어는 다른 소프트웨어의 구체적인 지식이 없이도 코드를 작성할 수 있어야합니다. 인터페이스는 이를 가능하게 해주는 자바의 방법 중 하나입니다.

인터페이스는 우리가 흔히 말하는 UI, API 등에 들어가는 Interface와 동일한 의미입니다. 인터페이스를 사용하는 이유는, **구체적인 내용에 대한 지식**이 없이도 **어떻게 동작할지에 대한 약속을 지킨다고 생각하고 사용할 수 있다**는 점에 있습니다.

### 자바의 인터페이스

자바에서 인터페이스는 클래스와 유사한 형태를 띄는 참조타입이고, 가지고 있을 수 있는 멤버가 한정되어 있습니다. (상수, 메서드 시그니처, default 메서드, static 메서드, 중첩 타입) 인터페이스는 인스턴스화 할 수 없으며, 클래스에서 구현하거나 다른 인터페이스에서 인터페이스를 상속할 수 있습니다.

인터페이스를 정의하는 것은 클래스 정의와 비슷합니다.

접근 제어자, `interface` 키워드, 인터페이스 이름, 쉼표로 구분된 상위 인터페이스의 목록(Optional), 인터페이스 본문으로 구성됩니다.

```java
public interface Car {

  // 상수 정의(가급적이면 사용하는 것을 권장 X, Effective Java 참조)

  // 메서드 시그니처 정의
  boolean drive();

  // 기타 default 메서드, static 메서드, 중첩 타입 정의
}
```

인터페이스를 `public`으로 지정하지 않으면, 동일한 패키지에 정의 된 클래스에서만 인터페이스에 액세스할 수 있습니다.

인터페이스는 여러 인터페이스를 상속할 수 있습니다. 따라서 선언할 때, `extends` 키워드 뒤에 여러 개의 상위 인터페이스가 올 수 있습니다.

메서드 시그니처(추상 메서드 `abstract`키워드는 입력하지 않음)는 중괄호를 입력하지 않고 세미콜론으로 끝냅니다.

그리고, 인터페이스 안의 모든 메서드는 암묵적으로 `public` 접근제어자가 지정되어 있기 때문에 입력하지 않아도 됩니다.

상수는 `public static final`이 암묵적으로 입력되어있지만, 지난 방송에서의 내용과 같이 일종의 안티패턴이므로 사용을 지양해야 합니다.

인터페이스를 사용하기 위해서는 인터페이스를 `implements`하는 클래스를 작성해야 합니다.

### 참고

- [오라클 자바 튜토리얼(인터페이스)](https://docs.oracle.com/javase/tutorial/java/IandI/createinterface.html)

## 인터페이스 구현하는 방법

인터페이스를 구현하는 방법은 다음과 같습니다.

```java
public class ToyCar implements Car {

  @Override // 컴파일러에서 재정의 관련 문제를 예방하기 위해서 꼭 붙입니다.
  public boolean drive() {
    System.out.println("장난감 자동차가 움직입니다.");
    return true;
  }

  public void play() {
    if (this.drive()) {
      System.out.println("장난감 자동차가 잘 움직였네요.");
    }
  }

  // 추가로 정의된 멤버, 메서드 구현
  // 인터페이스에 정의되지 않은 것은 인터페이스를 통해서는 확인할 수 없습니다.
}
```

우리가 만든 이 인터페이스는 API로도 사용될 수 있습니다. 일반적으로 우리 내부의 인터페이스를 제외한 외부에서 제공된 인터페이스를 API라고 생각하면 됩니다.  
우리가 API들을 사용할 때, 내부 동작에는 신경쓰지 않고 어떻게 동작할지에 대한 약속만 신뢰하는 것과 동일하다고 보면 됩니다.

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법

인터페이스는 인스턴스화가 불가능합니다. 그렇다면 어떻게 사용해야 할까요?

인터페이스를 구현한 클래스를 인스턴스화해서 사용할 수 있습니다.

```java
public static void main(String[] args) {
  Car car = new ToyCar();

  car.drive(); // 이건 됩니다.
  car.play(); // 이건 안됩니다.
}
```

위 코드는 컴파일 되지 않습니다. 왜일까요? `play`라는 메서드는 `ToyCar`에 분명히 정의되어 있습니다. 하지만 `Car`에서는 해당 메서드에 대해서 모르고 있는 상태입니다.

따라서, 해당 코드는 `play` 메서드를 호출하는 부분을 제거해야 컴파일 됩니다.

```java
public static void main(String[] args) {
  Car car = new ToyCar();

  car.drive(); // 이건 됩니다.
}
```

이렇게 된다면 장점은 바로, `ToyCar`를 다른 것으로 대체해도 인터페이스의 약속만 지킨다면 코드의 동작에 문제가 없다고 추측할 수 있다는 것입니다.  
다시말해 변경에 유연해집니다.

이것이 바로 추상화가 가져다 주는 변경에 유연해지는 코드입니다.

실제 인스턴스는 `ToyCar`의 인스턴스기 때문에 다음과 같이 사용이 가능합니다.

```java
public static void main(String[] args) {
  Car car = new ToyCar();

  car.drive(); // 이건 됩니다.
  ((ToyCar) car).play(); // 이건 됩니다.
}
```

## 인터페이스 상속

우리가 `DoIt` 이라는 인터페이스를 정의했다고 가정합시다.

```java
public interface DoIt {
  void doSomething(int i, double x);
  int doSomethingElse(String s);
}
```

하지만 나중에 우리가 새로운 메서드를 추가한다고 가정해봅시다. 그리고 인터페이스를 구현하는 구현체 전반에 걸쳐서 그 메서드를 구현하는 것이 필요할지도 생각해봅시다.  
그래야만 한다면, 인터페이스에 메서드를 추가하는 것이 올바른 선택이라고 볼 수 있습니다.

하지만, 그런 경우가 아니라면 어떨까요?

되도록이면 인터페이스는 변경에 신중해야 합니다. 처음 정의할 때 부터 모든 가능성을 염두에 두고 인터페이스를 지정해야합니다.  
굳이 인터페이스에 메서드를 추가해서 잘 동작하던 클래스를 컴파일 에러가 발생하지 않게하는 것이 효과적이고, 또한 필요없는 구현을 할 필요도 없습니다.  
이를 SOLID 원칙 중 ISP(인터페이스 분리 원칙)이라고도 합니다.

인터페이스 상속은 인터페이스를 분리하느냐, 아니면 상속하느냐 중에서 상속을 선택한 것입니다.

예제코드를 보면서 인터페이스 상속에 대해 알아봅시다.

```java
public interface DoItPlus extends DoIt {
  boolean didItWork(int i, double x, String s);
}
```

이제 이 인터페이스는 DoIt에 정의된 사항을 상속하며, 확장까지 할 수 있습니다.

또한 기존 인터페이스는 그대로 유지하면서 구현체 개발자가 선택을 할 수 있도록 했습니다.

혹은 별도의 인스턴스를 정의해서, 구현체가 여러 개의 인터페이스를 구현하도록 해도 됩니다.

### 인터페이스 상속과 클래스 상속

인터페이스 상속은 인터페이스에서만 수행가능하며 인터페이스는 여러 개의 인터페이스를 상속할 수 있습니다.

다중 상속의 문제 또한 개발자는 신경써야 하지만, 언어적으로는 큰 문제가 없습니다. 메서드 시그니처가 같다면 가려지는 것이 아닌 모두 구현한다고 받아들이면 됩니다.

반면, 클래스 상속은 클래스, 추상클래스에서 수행가능하며 둘 다 한 개의 클래스, 추상 클래스를 상속할 수 있습니다.

자바에서는 클래스 다중 상속은 지원하지 않습니다.

## 인터페이스의 기본 메소드(Default Method), 자바 8

default 메서드가 추가된 이유는 다음과 같습니다. 우리의 인터페이스는 최대한 수정이 되지 않도록 설계하는게 좋지만, 언제나 변화는 있기 마련입니다.

인터페이스에 메서드를 추가하면 기존에 인터페이스를 구현하던 클래스는 결국 컴파일 되지 않게됩니다. 그렇다면, 외부에 공개된 API의 경우 인터페이스 사용자의 불편이 많아질 것입니다.

하지만 default 메서드가 추가됨으로 인해 인터페이스를 유연하게 변경하면서, 인터페이스의 장점까지도 챙길 수 있게 되었습니다.

예제 코드를 통해 이해해봅시다.

```java
public interface Movable {
  void move();
}
```

```java
public class SimpleMove implements Movable {

  @Override
  public void move() {
    System.out.println("Simple Move");
  }
}
```

`Movable` 을 구현하는 `SimpleMove` 클래스를 정의했습니다. 이는 일반적인 인터페이스의 사용에 해당합니다.

그런데, Movable의 사양이 변경되었습니다. 멈출수도 있어야 합니다. 기존이라면 SimpleMove까지 같이 고쳐주어야 합니다. (물론, 우리 눈에 보이지 않는 구현체들도 모두 신경써줘야 하고, 인터페이스를 사용하는 쪽의 코드는 전부 깨질것입니다.)

아래와 같이 default 메서드를 적용해주면 구현체 전반에 걸쳐서 아래의 메서드가 구현됩니다. 따라서 컴파일 에러가 발생하지 않게됩니다.

```java
public interface Movable {
  void move();

  default void stop() {
    System.out.println("Stop");
  }
}
```

default 메서드가 포함된 인터페이스를 상속할 때 다음의 항목들을 수행할 수 있습니다.

- default 메서드는 상속을 하는 인터페이스는 default 메서드에 대해서 다시 알려주지 않아도 됩니다.
- default 메서드를 abstract 메서드로 다시 선언할 수 있습니다.
- default 메서드를 다시 재정의 할 수 있습니다.

```java
public interface SuperMovable extends Movable {

  @Override
  default void stop() {
    System.out.println("Super Stop");
  }
}
```

```java
public interface SuperMovable extends Movable {

  @Override
  void stop();
}
```

default 메서드가 등장함으로 다중 상속을 흉내낼 수 있게 되었습니다.

여러 메서드가 같은 시그니처를 가지면 충돌을 하게되고, 이를 해결하는 규칙이 있습니다.

- 클래스나, 슈퍼클래스에 정의된 메서드가 default 메서드보다 우선합니다. 이외의 상황에서는 default 메서드가 선택됩니다.
- default 메서드의 시그니처가 같고, 상속관계로도 충돌을 해결할 수 없는 경우, default 메서드를 사용하는 클래스에서 오버라이드해서 어떤 쪽의 디폴트 메서드를 호출할지 명시적으로 결정해주어야 합니다.

### 참고

- [오라클 자바 튜토리얼(인터페이스 default 메서드)](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)

## 인터페이스의 static 메소드, 자바 8

인터페이스의 인스턴스 생성과는 관계없이 인터페이스 타입에서 호출이 가능한 정적 메서드

접근 제어자는 `public`으로 설정되어 있습니다.

static 메소드를 사용할 때에는 인터페이스 타입을 통해서 사용합니다.

## 인터페이스의 private 메소드, 자바 9

자바 9에서는 인터페이스에서도 캡슐화를 지원합니다. 정확히는 `private` 메서드와 `private static` 메서드를 지원하기 시작했습니다.

`private` 메서드의 사용은 인터페이스 내부에서 코드의 재사용성을 향상시킵니다. 예를 들어서 두 개의 default 메서드가 같은 코드를 공유해야하는 경우 중복이 발생하게 되는데 이를 방지할 수 있습니다.

private 메서드를 사용하기 위한 4가지 규칙

- private 인터페이스 메서드는 추상 메서드가 될 수 없습니다.
- private 메서드는 인터페이스 내부에서만 사용될 수 있습니다.
- private static 메서드는 다른 static, non-static 인터페이스 메서드에서 사용될 수 있습니다.
- private non-static 메서드는 private static 메서드 내부에서 사용될 수 없습니다.

```java
public interface CustomInterface {

  void method1();

  default void method2() {
    method4(); // 디폴트 메서드 안의 private 메서드
    method5(); // non-static 메서드 안의 static 메서드
    System.out.println("default method 2");
  }

  static void method3() {
    method5(); // static 메서드 안의 private static 메서드
    System.out.println("static method");
  }

  private void method4() {
    System.out.println("private method 4");
  }

  private static void method5() {
    System.out.println("private staic method");
  }
}

public class CustomClass implements CustomInterface {

  @Override
  public void method1() {
    System.out.println("abstract method");
  }

  public static void main(String[] args) {
    CustomInterface instance = new CustomClass();
    instance.method1();
    instance.method2();
    CustomInterface.method3();
  }
}

```

결과

```text
abstract method
private method 4
private staic method
default method 2
private staic method
static method
```

### private 인터페이스 메서드 예제

```java
import java.util.function.IntPredicate;
import java.util.stream.IntStream;

public interface CustomCalculator {

  default int addEvenNumbers(int... nums) {
    return add(n -> n % 2 == 0, nums);
  }

  default int addOddNumbers(int... nums) {
    return add(n -> n % 2 != 0, nums);
  }

  private int add(IntPredicate predicate, int... nums) {
    return IntStream.of(nums)
        .filter(predicate)
        .sum();
  }
}

public class Main implements CustomCalculator {

  public static void main(String[] args) {
    CustomCalculator demo = new Main();

    int[] nums = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    int sumOfEvens = demo.addEvenNumbers(nums);
    System.out.println(sumOfEvens);

    int sumOfOdds = demo.addOddNumbers(nums);
    System.out.println(sumOfOdds);
  }
}
```

```text
20
25
```

참고로 Odd라는 조건은 결국 Even의 논리적인 부정이므로 `IntPredicate`의 `negate()` 메서드를 사용해도 됩니다.

이 역시 IntPredicate의 default 메서드로 구현되어있습니다. (Functional Interface지만, default 메서드는 람다식을 만드는데 문제가 되지 않음을 알 수 있습니다.)

```java
public interface CustomCalculator {

  IntPredicate isEven = n -> n % 2 == 0;

  default int addEvenNumbers(int... nums) {
    return add(isEven, nums);
  }

  default int addOddNumbers(int... nums) {
    return add(isEven.negate(), nums);
  }

  private int add(IntPredicate predicate, int... nums) {
    return IntStream.of(nums)
        .filter(predicate)
        .sum();
  }
}

```

### 참고

- [How to Do in Java(Java9)](https://howtodoinjava.com/java9/java9-private-interface-methods/)

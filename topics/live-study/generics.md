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

제네릭 타입을 호출하고 인스턴스화 하는 방법은 간단합니다.

우리가 위에서 만들었던 `Box` 클래스를 예로 들자면 다음과 같은 코드를 작성할 수 있습니다.

`Box <Integer> integerBox;`

여기서 `Integer`가 위치한 곳을 타입 인자라고 합니다.

> 타입 파라미터와 타입 인자의 차이
>
> 타입 파라미터는 파라미터화 된 타입을 생성하기 위해서 사용합니다. 우리가 제네릭 클래스를 정의할 때 사용한 것이 타입 파라미터입니다.
>
> 타입 인자는 제네릭 클래스를 사용할 때, 타입을 인자로 받는 부분을 의미합니다. 우리가 제네릭 클래스를 실제로 사용할 때 사용합니다.

`integerBox` 또한 선언한다고 실제 인스턴스가 생성되지는 않습니다. 단순히 `Integer` 값을 사용하는 `Box` 클래스임을 설명합니다.

제네릭 클래스는 다른 이름으로 parameterized type이라고도 합니다.

이 클래스를 인스턴스화 하기 위해서는 다른 인스턴스를 만들때와 마찬가지로 `new` 키워드를 사용하고, 클래스 이름과 괄호 사이에 `<Integer>`를 넣어주면 됩니다.

### 다이아몬드 연산자

Java SE 7 이후부터 컴파일러가 코드의 문맥을 판단해서 타입 인수를 추론해 줄 수 있게 되었습니다. 이는 이전에 타입 추론에 대해서 논의할 때, 나왔던 적이 있습니다.

제네릭 클래스의 생성자를 호출하는데 필요한 정보를 명시하지 않고 `<>`이렇게 바꿔줄 수 있습니다.

```java
Box<Integer> integerBox = new Box<Integer>(); // before
Box<Integer> integerBox = new Box<>(); // after
```

이로 인해서 보다 편리하게 프로그래밍이 가능해졌습니다.

왜 이를 다이아몬드 연산자라고 부르냐면 `<>`의 모양이 다이아몬드 모양과 비슷하기 때문입니다.

### 여러개의 타입 파라미터 정의하기

제네릭 클래스는 여러개의 타입 파라미터를 가질 수 있습니다. 예를 들어, `Pair`라는 인터페이스가 있고, 이를 구현하는 `OrderedPair` 구현체가 있습니다.

```java
public interface Pair<K, V> {

  public K getKey();

  public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

  private K key;
  private V value;

  public OrderedPair(K key, V value) {
    this.key = key;
    this.value = value;
  }

  @Override
  public K getKey() {
    return key;
  }

  @Override
  public V getValue() {
    return value;
  }
}
```

다음의 두 문장은 `OrderedPair` 클래스의 인스턴스를 만듭니다.

```java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String> p2 = new OrderedPair<String, String>("hello", "world");
```

`p1`의 코드가 동작하는 이유는 오토 박싱이 적용되기 때문입니다.

위에서 언급했던 다이아몬드 연산자를 적용하면, 타입 추론에 의해 다음과 같이 고칠 수 있습니다.

```java
Pair<String, Integer> p1 = new OrderedPair<>("Even", 8);
Pair<String, String> p2 = new OrderedPair<>("hello", "world");
```

제네릭 인터페이스를 정의하는 것은 제네릭 클래스를 정의하는 것과 유사합니다.

### 제네릭을 타입 파라미터로 사용할 수 있을까?

당연히 가능합니다. 제네릭을 타입 파라미터로 사용하면 다음과 같은 코드가 작성될 수 있습니다.

```java
OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<>());
```

### Raw 타입

로 타입은 타입 인수가 없는 제네릭 클래스 또는 인터페이스를 의미합니다.

```java
Box rawBox = new Box();
```

이는 `Box` 타입의 `raw type`이라고 부릅니다.

이 raw Type은 타입 인자로 `Object`가 되었다고 가정합니다.

이는 컴파일러에서 생성을 경고하고, 제네릭 메서드를 사용할 때에도 경고합니다. 이는 제네릭을 사용하는 이유인 컴파일 타임 타입 안정성을 런타임으로 연기하기 때문에 발생합니다. 따라서 우리는 로타입을 사용하는 것을 지양해야합니다.

## 제네릭 주요 개념(바운디드 타입, 와일드 카드)

### 바운디드 타입

우리가 제네릭 클래스를 사용할 때, 타입 인자로 받고싶은 타입을 특정 타입으로 제한하고 싶은 경우가 있습니다.

이럴 때 바운디드 타입을 사용해주면 됩니다.

바운디드 타입 파라미터를 선언하기 위해서는 '타입 파라미터의 이름', '`extends` 키워드', '상위 경계'를 나열하면 됩니다. 여기서 `extends`는 `implements`도 의미합니다.

```java
public class Box<T> {

  private T t;

  public void set(T t) {
    this.t = t;
  }

  public T get() {
    return t;
  }

  public <U extends Number> void inspect(U u) {
    System.out.println("T: " + t.getClass().getName());
    System.out.println("U: " + u.getClass().getName());
  }

  public static void main(String[] args) {
    Box<Integer> integerBox = new Box<>();
    integerBox.set(10);
    integerBox.inspect("some text"); // 컴파일 에러발생 (String 타입)
  }
}
```

이는 제네릭 메서드에만 사용할 수 있는 것이 아니라, 제네릭 클래스 선언에도 이용할 수 있습니다.

또한, 여러 타입을 가지도록 할 수도 있습니다. `<T extends B1 & B2 & B3>`

이렇게 경계가 여러개라면 나열된 모든 타입의 하위 타입이어야 합니다. 그리고 클래스와 인터페이스를 같이 사용하는 경우 클래스를 먼저 지정해야 합니다.

#### 참고

- [오라클 자바 튜토리얼(바운디드 타입)](https://docs.oracle.com/javase/tutorial/java/generics/bounded.html)

### 와일드카드

제네릭 코드에서 물음표(`?`)를 보신적이 있으실 겁니다. 이는 와일드카드라고 하고, 알 수 없는 타입을 나타냅니다. 와일드카드는 매개 변수, 필드 또는 지역 변수의 타입으로 다양한 상황에서 사용할 수 있습니다. 또한 반환 타입으로도 작성될 수 있지만, 이는 추천하지는 않습니다.(구체적으로 작성할 수 있다면 구체적으로 작성해야합니다.)

와일드카드는 제네릭 메서드 호출, 제네릭 인스턴스 생성 또는 슈퍼타입의 타입 인수로는 사용되지 않습니다.

#### 상한 와일드카드

상한 와일드카드를 사용하면 변수에 대한 제한을 완화해줄 수 있습니다. 예를들어 `List<Integer>`, `List<Double>`, `List<Number>` 에서 작동하는 메서드를 작성할 때, 상한 와일드카드를 이용할 수 있습니다.

상한 와일드카드를 선언하는 방법은 와일드카드 문자('?'), extends 키워드, upper bound를 입력합니다.

위에서 언급한 것을 구현하려면 `List<? extends Number>` 로 정의해주면 됩니다. 이 의미는 `Number`와 그 하위 클래스를 모두 의미하는 것입니다.

좀 더 응용해보자면 다음과 같은 코드를 작성할 수도 있습니다.

```java
public static void process(List<? extends Foo> list) {
    for (Foo elem : list) {
        // ...
    }
}
```

이는 Foo의 서브타입들이 들어올 것 이므로 이렇게 enhanced for loop를 사용해줄 수 있습니다.

#### 제한없는 와일드카드

제한없는 와일드카드는 단순히 `?` 기호만 써주면 됩니다. 이것이 필요한 두 가지 경우가 있습니다.

- `Object` 클래스에서 제공하는 기능을 사용하여 구현할 수 있는 메서드를 작성하는 경우
- 코드가 딱히 타입에 의존하지 않는 제네릭 클래스의 메서드를 사용하는 경우(예를 들어, List.clear() 같은)

#### 하한 와일드카드

상한 와일드카드와 유사하게 하한 와일드카드는 알 수 없는 타입을 특정 타입 또는 해당 타입의 슈퍼타입으로 제한합니다.

하한 와일드카드를 선언하는 방법은 와일드카드 문자('?'), super 키워드, lower bound를 입력합니다.

상한과 하한을 둘 다 지정할 수는 없습니다.

#### 참고

- [오라클 자바 튜토리얼(와일드카드)](https://docs.oracle.com/javase/tutorial/java/generics/wildcards.html)
- [와일드카드 사용 지침](https://docs.oracle.com/javase/tutorial/java/generics/wildcardGuidelines.html)

## 제네릭 메소드 만들기

제네릭 메서드는 자기 자신의 타입 파라미터를 사용하는 메서드입니다. 이는 제네릭 타입을 선언하는 것과 유사하지만, 제네릭 메서드의 타입 파라미터의 스코프는 선언된 메서드에 한정됩니다. 제네릭 클래스 생성자, static, non-static 메서드에서 사용할 수 있습니다.

제네릭 메서드의 문법은 메서드 반환 값 앞에 꺽쇠 괄호 안에 타입 파라미터 목록을 추가해주면됩니다.

```java
public class Util {
  public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
    return p1.getKey().equals(p2.getKey()) &&
           p1.getValue().equals(p2.getValue());
  }
}
```

이 메서드는 다음과 같이 사용할 수 있습니다.

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.<Integer, String>compare(p1, p2);
```

이는 타입 인자를 명시적으로 표시해주었습니다. 이는 컴파일러가 추론을 해줄 수 있기 때문에 다음과 같이 줄여쓸 수 있습니다.

```java
Pair<Integer, String> p1 = new Pair<>(1, "apple");
Pair<Integer, String> p2 = new Pair<>(2, "pear");
boolean same = Util.compare(p1, p2);
```

## Erasure

제네릭을 구현하기 위해서 자바 컴파일러가 타입 제거를 수행합니다.

- 제네릭의 타입 파라미터의 범위를 지정하지 않은 경우 `Object`로 바꿉니다. 따라서 생성된 바이트코드에는 일반적인 클래스, 인터페이스, 메서드만 포함됩니다.
- 타입 안정성을 위해 필요한 경우에 타입 캐스팅을 넣습니다.
- `extends` 된 제네릭 타입에서 다형성을 보호하기 위해서 브릿지 메서드를 생성합니다.

이를 수행하면 결과적으로 제네릭을 사용할 떄, 런타임에는 오버헤드가 발생하지 않습니다.

### 수행 과정

예제 코드를 통해서 보겠습니다.

```java
public class Node<T> {

    private T data;
    private Node<T> next;

    public Node(T data, Node<T> next) {
        this.data = data;
        this.next = next;
    }

    public T getData() { return data; }
    // ...
}
```

1. 모든 타입 파라미터를 지우고, 바인드가 되어있는 경우를 첫번째 바인드로 대체하고, 바인드가 되어있지 않으면 `Object`로 대체합니다.
    ```java
    public class Node {

        private Object data;
        private Node next;

        public Node(Object data, Node next) {
            this.data = data;
            this.next = next;
        }

        public Object getData() { return data; }
        // ...
    ```
    - 바운드가 되어 있는 경우
        ```java
        public class Node<T extends Comparable<T>> {

        private T data;
        private Node<T> next;

        public Node(T data, Node<T> next) {
            this.data = data;
            this.next = next;
        }

        public T getData() { return data; }
        // ...
        }
        ```
        컴파일러는 T가 바인딩 된 클래스인 `Comparable`로 대체해줍니다.
        ```java
        public class Node {

            private Comparable data;
            private Node next;

            public Node(Comparable data, Node next) {
                this.data = data;
                this.next = next;
            }

            public Comparable getData() { return data; }
            // ...
        }
        ```
2. 제네릭 메서드의 타입 파라미터도 제거합니다.
    ```java
    public static <T> int count(T[] anArray, T elem) {
        int cnt = 0;
        for (T e : anArray)
            if (e.equals(elem))
                ++cnt;
            return cnt;
    }
    ```
    `T`가 bound 되어있지 않으므로 이 코드는 다음과 같이 변경됩니다.
    ```java
    public static int count(Object[] anArray, Object elem) {
        int cnt = 0;
        for (Object e : anArray)
            if (e.equals(elem))
                ++cnt;
            return cnt;
    }
    ```

### 타입 삭제와 브리지 메서드

타입 삭제를 수행하면 문제가 발생할 수 있습니다. 그래서 컴파일러는 브리지 메서드를 합성해줍니다.

우리가 이에 대해 알아야 하는 이유는 스택트레이스가 남을 때, 당황스러울 수 있기 때문입니다.

타입 캐스팅이 이루어져야 하는 경우 컴파일러가 알아서 브리지 메서드를 만들어줍니다.

### 참고

- [오라클 자바 튜토리얼(타입 제거)](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html)

## 제네릭의 제한

- 기본 타입은 제네릭 타입에 사용할 수 없습니다.
- 타입 파라미터의 인스턴스를 생성할 수는 없습니다.
- 타입이 타입 파라미터인 `static` 필드는 선언할 수 없습니다.
- 매개변수 타입을 캐스팅하거나 `instanceof`를 사용할 수 없습니다.
- 매개변수 타입의 배열은 만들 수 없습니다.
- 제네릭 클래스는 `Throwable` 클래스를 직접, 간접적으로 `extends` 할 수 없습니다. 다만 T를 `throwable`에는 사용할 수 있습니다.
- 같은 로타입이 되는 파라미터 타입들이 지워지는 경우 오버로딩할 수 없습니다.

### 참고

- [오라클 자바 튜토리얼(제네릭의 제한)](https://docs.oracle.com/javase/tutorial/java/generics/restrictions.html)

# 3주차: 연산자

## 학습할 것

- [산술 연산자](#산술-연산자)
- [비트 연산자](#비트-연산자)
- [관계 연산자](#관계-연산자)
- [논리 연산자](#논리-연산자)
- [instanceof](#instanceof)
- [assignment(=) operator](#assignment-operator)
- [화살표(->) 연산자](#화살표--연산자)
- [3항 연산자](#3항-연산자)
- [연산자 우선 순위](#연산자-우선-순위)
- [switch 연산자](#switch-연산자)

먼저 연산자란 무엇인지에 대해서 먼저 알아야 합니다. 연산자란, 하나, 둘 혹은 세 개의 피연산자에 대해서 특정 연산을 수행한 후 **결과를 반환**하는 기호입니다.

간단히 말해서, '연산을 수행하는 기호'입니다.

피연산자는 연산의 대상이라고 할 수 있습니다. 리터럴이 될 수도 있고, 변수가 될 수도 있으며, 또 다른 식이 될 수도 있습니다.

식은 연산자와 피연산자를 조합한 것이고, 이를 수행하는 것을 평가한다고 합니다.

위의 내용을 요약하자면, 연산은 식을 평가하고 식의 평가 결과를 반환하는 것입니다.

평가의 방향은 연산자마다 다르며, 알고있어야 합니다.

## 산술 연산자

산술 연산자는 우리가 흔히 말하는 더하기, 빼기, 곱하기, 나누기를 수행하는 연산자입니다. 나누기만 조금 유의하면 되는데, 몫을 구하는 연산자와, 나머지를 구하는 연산자가 따로 존재합니다.

| 연산자 | 설명                                       |
| :----: | ------------------------------------------ |
|   +    | 더하기 연산자(문자열 연결에도 사용됩니다.) |
|   -    | 빼기 연산자                                |
|   \*   | 곱하기 연산자                              |
|   /    | 나누기 연산자(정수형은 몫 연산자)          |
|   %    | 나머지(모듈로) 연산자                      |

```java
int result = 1 + 2;
System.out.println("1 + 2 = " + result); // 3

int originalResult = result;
result = originalResult - 1;
System.out.println(originalResult + " - 1 = " + result); // 2

originalResult = result;
result = originalResult * 7;
System.out.println(originalResult + " * 7 = " + result); // 14

originalResult = result;
result = originalResult / 3;
System.out.println(originalResult + " / 3 = " + result); // 4

result = originalResult % 3;
System.out.println(originalResult + " % 3 = " + result); // 2

double doubleResult = 14.0 / 3.0;
System.out.println("14.0 / 3.0 = " + doubleResult); // 4.66667

doubleResult = 14.0 % 3.0;
System.out.println("14.0 % 3.0 = " + doubleResult); // 2.0

doubleResult = 14.0 / 0.0;
System.out.println("14.0 / 0.0 = " + doubleResult); // Infinity

doubleResult = -14.0 / 0.0;
System.out.println("14.0 / 0.0 = " + doubleResult); // -Infinity

doubleResult = 14.0 % 0.0;
System.out.println("14.0 % 0.0 = " + doubleResult); // NaN

doubleResult = -14.0 % 0.0;
System.out.println("14.0 % 0.0 = " + doubleResult); // NaN

try {
  result = 14 / 0; // ArithmeticException: / by zero
} catch (ArithmeticException e) {
  System.out.println("14 / 0 = " + e.getMessage());
}

try {
  result = 14 % 0; // ArithmeticException: / by zero
} catch (ArithmeticException e) {
  System.out.println("14 % 0 = " + e.getMessage());
}
```

정수형일 때 조심해야 할 것은 0으로 나누거나 모듈로 연산을 수행하는 것입니다. 이는 런타임에 발생하는 오류이기 때문에 컴파일 타임에 확인할 수 없습니다.

또한, 자료형의 크기에 따른 오버플로우 문제도 조심해야 합니다.

실수형은 연산 과정 자체에서 오차가 발생할 수 있으며 이를 조심해야 합니다. 이는 특히 테스트 코드를 작성할 때, 어느정도의 수준의 오차를 허용할 것인지 반드시 설정해주어야 하는 이유입니다.

실수형은 0으로 나누거나, 모듈로 연산을 사용하면 위와 같이 `Infinity`, `-Infinity`, `NaN` 값을 가집니다.

또한 `Infinity`와 `NaN`이 피연산자인 경우 또한 존재합니다.

### 단항 연산자

`+`를 붙이면 양수 값을 나타냅니다. 하지만 생략해도 됩니다.

`-`를 붙이면 음수 값을 나타냅니다.

`++`를 붙이면 값을 1씩 증가시킵니다. 이는 피연산자의 위치에 따라 의미가 살짝 달라집니다. 오른쪽에 위치하는 경우 식을 평가하기 전 증가시키고, 왼쪽에 위치하는 경우 식을 평가한 후에 증가시킵니다.

`--`를 붙이면 값을 1씩 감소시킵니다. 피연산자의 위치에 따른 의미는 위와 동일합니다.

```java
int result = +1;
System.out.println(result); // 1

result--;
System.out.println(result); // 0

result++;
System.out.println(result); // 1

for (int i = 0; i < 3; i++) {
  System.out.println(++result); // 2, 3, 4
}

System.out.println(result); // 4(이미 이전에 증가해서 그대로 출력된다.)

for (int i = 0; i < 3; i++) {
  System.out.println(result++); // 4, 5, 6
}

System.out.println(result); // 7(마지막에 증가한다.)
```

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op1.html>

<https://www.geeksforgeeks.org/operators-in-java/>

<https://www.programiz.com/java-programming/operators>

---

[학습할 것으로](#학습할-것)

## 비트 연산자

Java에서는 정수 타입에 대해서 비트와 비트 시프트 연산을 제공하는데 이를 비트 연산자라고 합니다.

비트 연산이라는 의미는 우리가 사용하는 정수 타입은 사실 컴퓨터가 이해할 수 있도록 2진수 형식으로 표현됩니다. 이진수 조각 하나 하나를 비트라고 표현하고, 비트연산은 이 조각들마다 연산을 수행합니다.

`~` 는 비트를 반전시키는 단항 연산자입니다. '0'은 '1'로, '1'은 '0'으로 반전시키게 됩니다. 만약 `byte` 타입의 리터럴 0이 있다면 이진수로는 `00000000` 으로 표현될 것입니다. 이를 비트 반전시키면 `11111111` 이 됩니다. 이를 1의 보수를 만든다고도 표현합니다.

`&`는 비트 `AND` 연산입니다. 이항 연산자이며, 피연산자들의 비트별로 `AND` 연산을 수행하게 됩니다.

`|`는 비트 `OR` 연산입니다. 이항 연산자이며, 피연산자들의 비트별로 `OR` 연산을 수행하게 됩니다.

`^`는 비트 `XOR` 연산입니다. 이항 연산자이며, 피연산자들의 비트별로 `XOR` 연산을 수행하게 됩니다. 둘이 다를 때만 1이 됩니다.

|  x  |  y  | x \| y | x & y | x ^ y |
| :-: | :-: | :----: | :---: | :---: |
|  1  |  1  |   1    |   1   |   0   |
|  1  |  0  |   1    |   0   |   1   |
|  0  |  1  |   1    |   0   |   1   |
|  0  |  0  |   0    |   0   |   0   |

```java
// 참고로 부호비트도 고려해야 합니다.
byte a = 0b00001010; //  10  (0|0001010)
byte b = 0b00101000; //  40  (0|0101000)
byte c = -0b00001010; // -10 (1|1110110)

// ~ 연산자(1의 보수를 만들어줌)
System.out.println(~a); // 1|1110101 => -11 여기에 1을 더하면 -10이 됩니다.(2의 보수)
System.out.println(~c); // 0|0001001 => 9   여기에 1을 더하면 10이 됩니다.

// & 연산자
System.out.println(a & b); // 0|0001000 => 8
System.out.println(a & c); // 0|0000010 => 2
System.out.println(b & c); // 0|0100000 => 32

// | 연산자
System.out.println(a | b); // 0|0101010 => 42
System.out.println(a | c); // 1|1111110 => -2
System.out.println(b | c); // 1|1111110 => -2

// ^ 연산자(다른 경우에만 1)
System.out.println(a ^ b); // 0|0100010 => 34
System.out.println(a ^ c); // 1|1111100 => -4
System.out.println(b ^ c); // 1|1011110 => -34
```

### 비트 시프트 연산자 `<<`, `>>`

시프트 연산자는 피연산자의 각 비트를 오른쪽(`>>`) 또는 왼쪽(`<<`) 으로 이동시키는 연산자입니다.

연산자의 왼쪽에는 피연산자를 작성하고, 오른쪽에는 이동시킬 비트 수를 피연산자로 작성합니다.

`<<` 연산자의 경우에는 빈 칸을 0으로 채우면 되지만, `>>` 연산자는 음수일 때는 1로, 양수일 때는 0으로 채웁니다.

여기서 나타나는 중요한 특성이 있는데, `x << n` 연산은 x \* 2<sup>n</sup> 과 같습니다.

마찬가지로 `x >> n` 연산은 x / 2<sup>n</sup> 과 같습니다.

이를 사용하는 이유는 비트 연산이 일반 산술연산보다 속도가 빠르기 때문입니다. 하지만 가독성이 떨어지므로 정말 성능개선이 필요한 부분에만 사용해야 합니다.

`>>>` Unsigned Right 시프트 연산자는 오른쪽으로 이동하되, 왼쪽의 채워지는 비트를 0으로 채웁니다.

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op3.html>

<https://www.geeksforgeeks.org/operators-in-java/>

<https://www.programiz.com/java-programming/operators>

---

[학습할 것으로](#학습할-것)

## 관계 연산자

관계 연산자는 피연산자가 같은지(==), 같지 않은지(!=), 큰지(>), 크거나 같은지(>=), 작은지(<), 작거나 같은지(<=) 비교하는 연산자들입니다.

우리가 생각하는 대로 동작하는 간단한 연산자여서 사용 예만 보고 넘어가겠습니다.

대신, 문자열의 동등성을 비교할 때에는 `equals()` 메서드를 사용해야 합니다.

```java
int a = 7;
int b = 11;

System.out.println(a == b); // false
System.out.println(a != b); // true
System.out.println(a > b);  // false
System.out.println(a >= b); // false
System.out.println(a < b);  // true
System.out.println(a <= b); // true
```

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html>

<https://www.geeksforgeeks.org/operators-in-java/>

---

[학습할 것으로](#학습할-것)

## 논리 연산자

논리 연산자는 피연산자로 boolean 값을 받으며, 이는 평가의 결과가 될 수도 있습니다. 보통 식이 전달되는 경우가 많습니다.

`&&` 연산자는 두 연산결과가 참인지 판단합니다. 만약 둘 중 하나라도 거짓이라면 거짓을 반환하게 됩니다. 만약, 왼쪽의 피연산자가 거짓이라면 오른쪽의 피연산자는 평가하지 않습니다.

`||` 연산자는 두 연산결과 중 한 쪽만 참이어도 참을 반환합니다. 만약, 왼쪽의 피연산자가 참이라면 오른쪽의 피연산자는 평가하지 않습니다.

`!` 연산자는 피연산자를 하나만 받는 단항 연산자이며, `true` 를 `false`로, `false`를 `true`로 변경합니다. 식의 평가 방향은 오른쪽에서 왼쪽입니다.

위에서 말한 평가하지 않는 것을 short circuit evaluation이라고 하며, 식의 평가를 수행하지 않기 때문에 좀 더 빠른 결과를 얻을 수 있습니다.

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op1.html>

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html>

[학습할 것으로](#학습할-것)

## instanceof

`instanceof` 연산자는 타입 비교 연산자입니다. 이를 이용해서 객체가 클래스의 인스턴스, 하위 클래스의 인스턴스 또는 특정 인터페이스를 구현하는 클래스의 인스턴스인지 테스트할 수 있습니다.

```java
public class Main {

  public static void main(String[] args) {
    Animal animal1 = new Animal();
    Animal animal2 = new Cat();

    System.out.println("animal1 is instanceof Animal: " + (animal1 instanceof Animal)); // true
    System.out.println("animal1 is instanceof Cat: " + (animal1 instanceof Cat)); // false
    System.out.println("animal1 is instanceof Meowable: " + (animal1 instanceof Meowable)); // false

    System.out.println("animal2 is instanceof Animal: " + (animal2 instanceof Animal)); // true
    System.out.println("animal2 is instanceof Cat: " + (animal2 instanceof Cat)); // true
    System.out.println("animal2 is instanceof Meowable: " + (animal2 instanceof Meowable)); // true
  }
}

class Animal {}
interface Meowable {}
class Cat extends Animal implements Meowable {}
```

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html>

---

[학습할 것으로](#학습할-것)

## assignment(=) operator

할당 연산자는 연산자 오른쪽의 값을 연산자 왼쪽의 피연산자에 할당합니다.

식의 평가 방향은 오른쪽에서 왼쪽입니다.

`int x = 0;`

위와 같은 연산자는 `x`라는 이름을 가진 변수 공간에 0이라는 `int` 형 리터럴을 할당합니다.

또한, 객체를 할당할 수도 있습니다.

`Animal animal = new Cat();`

위와 같은 식은 `animal` 이라는 `Animal` 타입 참조 변수에 새로 만들어진 `Cat` 인스턴스의 참조를 할당합니다.

주의할 점은 할당할 수 없는 리터럴과 같은 값은 왼쪽 피연산자가 될 수 없다는 점입니다.

### 복합문(Compound Statement), 복합 할당(compound assignment)

다른 연산자와 할당 연산자를 결합합니다.

`a = a + 1;` 이라는 식의 경우 `a`가 중복됩니다. 이를 간단히 나타내기 위한 표현입니다.

`a += 1;` 로 바꿀 수 있습니다.

과연 컴파일되면 같은 바이트코드가 될까요?

```java
public static void main(String[] args) {
  int a = 0;

  a = a + 1;
}
```

```text
  public static void main(java.lang.String[]);
    Code:
       0: iconst_0
       1: istore_1
       2: iload_1
       3: iconst_1
       4: iadd
       5: istore_1
       6: return
```

```java
public static void main(String[] args) {
  int a = 0;

  a += 1;
}
```

```text
  public static void main(java.lang.String[]);
    Code:
       0: iconst_0
       1: istore_1
       2: iinc          1, 1
       5: return
```

둘의 바이트 코드가 다른 것을 확인할 수 있습니다.

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op1.html>
<https://www.geeksforgeeks.org/operators-in-java>

---

[학습할 것으로](#학습할-것)

## 화살표(->) 연산자

화살표 연산자는 Java8에서 추가된 람다 표현식의 한 부분입니다.

`(파라미터) -> { Body }` 형태로 작성합니다.

람다 표현식은 함수형 프로그래밍에서 가장 중요한 부분 중 하나이며, 이름 도입함으로써 자바는 함수형 언어로도 기능하게 되었습니다. 이는 일부 익명 클래스를 간단히 표현할 수 있는 방법이라고도 볼 수 있습니다.
또한 `@FunctionalInterface` 애노테이션을 붙인 인터페이스의 구현체로도 사용할 수 있습니다.

인텔리제이에서는 이런 람다 표현식으로 코드를 변경시켜주는 기능이 내장되어 있습니다.

람다식은 메서드가 매개변수로 전달되는 것이 가능하고, 결과로 반환되는 것도 가능하게 해줍니다. 이를 함수가 일급시민이다라고 표현합니다.

자세한 내용은 람다식을 공부할 때 다시 공부해봅시다.

### 참고

<https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html>

---

[학습할 것으로](#학습할-것)

## 3항 연산자

`?:` 는 논리 연산자의 일종으로 피연산자를 세 개를 받는 유일한 연산자여서 삼항 연산자로 많이 불립니다.

이는 if - else 문과 비슷하게 보이며, 한 줄로 간단하게 표현할 수도 있습니다.

`조건 ? 참일 때 값 : 거짓일 때 값` 의 표현으로 사용합니다.

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html>

---

[학습할 것으로](#학습할-것)

## 연산자 우선 순위

연산자 우선 순위는 식을 평가할 때 많은 영향을 미칩니다. 우선 순위에 따라서 의도한 대로 프로그램이 동작하지 않을 수 있기 때문입니다.

아래의 표는 연산자의 우선순위를 나타낸 표입니다. 상단에 가까울 수록 우선순위가 높으며, 같은 줄의 연산자는 동일한 우선순위를 가집니다. 이럴 때에는 식의 평가순서가 영향을 미치게 됩니다. 할당 연산자를 제외한 모든 이항 연산자는 왼쪽에서 오른쪽으로 평가되며, 할당 연산자는 오른쪽에서 왼쪽으로 평가합니다.

| Operators     | Precedence                                                   |
| ------------- | ------------------------------------------------------------ |
| 후위형        | `expr++` `expr--`                                            |
| 단항 연산자   | `++expr` `--expr` `+expr` `-expr` `~` `!`                    |
| 곱셈과 나눗셈 | `*` `/` `%`                                                  |
| 증감 연산자   | `+` `-`                                                      |
| 비트 시프트   | `<<` `>>` `>>>`                                              |
| 관계 비교     | `<` `>` `<=` `>=` `instanceof`                               |
| 동등 비교     | `==` `!=`                                                    |
| 비트 AND      | `&`                                                          |
| 비트 XOR      | `^`                                                          |
| 비트 OR       | `|`                                                          |
| 논리 AND      | `&&`                                                         |
| 논리 OR       | `||`                                                         |
| 삼항 연산자   | `?` `:`                                                      |
| 할당 연산자   | `=` `+=` `-=` `*=` `/=` `%=` `&=` `^=` `|=` `<<=` `>>=` `>>>=` |

물론 실제 프로그래밍 세계에서는 헷갈릴 가능성이 높으므로 `()` 로 먼저 수행하고 싶은 식을 묶어주는 것이 가독성 등의 여러 관점에서 더 좋습니다. 대신 자주 나오는 연산자들의 우선순위는 알아두는 편이 좋습니다.

### 참고

<https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html>

---

[학습할 것으로](#학습할-것)

## switch 연산자

---

[학습할 것으로](#학습할-것)

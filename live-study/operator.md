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
|   /    | 나누기 연산자                              |
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

---

[학습할 것으로](#학습할-것)

## 비트 연산자

---

[학습할 것으로](#학습할-것)

## 관계 연산자

---

[학습할 것으로](#학습할-것)

## 논리 연산자

---

[학습할 것으로](#학습할-것)

## instanceof

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

---

[학습할 것으로](#학습할-것)

## 3항 연산자

---

[학습할 것으로](#학습할-것)

## 연산자 우선 순위

연산자 우선 순위는 식을 평가할 때 많은 영향을 미칩니다. 우선 순위에 따라서 의도한 대로 프로그램이 동작하지 않을 수 있기 때문입니다.

---

[학습할 것으로](#학습할-것)

## switch 연산자

---

[학습할 것으로](#학습할-것)

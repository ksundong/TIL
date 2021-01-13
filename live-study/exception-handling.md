# 9주차 과제: 예외 처리

## 학습할 것

- [예외란](#예외란)
- [자바에서 예외 처리 방법 (try, catch, throw, throws, finally)](#자바에서-예외-처리-방법-try-catch-throw-throws-finally)
- [try-with-resources](#try-with-resources)
- [자바에서 예외를 발생시키는 방법](#자바에서-예외를-발생시키는-방법)
- [자바가 제공하는 예외 계층 구조](#자바가-제공하는-예외-계층-구조)
- [Exception과 Error의 차이는?](#exception과-error의-차이는)
- [RuntimeException과 RE가 아닌 것의 차이는?](#runtimeexception과-re가-아닌-것의-차이는)
- [커스텀한 예외 만드는 방법](#커스텀한-예외-만드는-방법)
- [예외 포장](#예외-포장)

## 예외란

예외는 "exceptional event"의 약어입니다.

정의: 예외는 프로그램 실행중에 발생하는 프로그램의 실행의 일반적인 흐름을 방해하는 이벤트입니다.

메서드내에서 에러가 발생하면, 메서드는 객체를 만들고 런타임 시스템에 전달합니다. 이 객체는 예외 객체라고 불리며 에러에 대한 정보와 이에 대한 타입, 에러가 발생한 시점의 프로그램의 상태에 대한 정보를 담고있습니다. 예외 객체를 생성하여 런타임 시스템에 전달하는 것을 예외를 던진다고 표현합니다.

메서드가 예외를 던지면, 런타임 시스템은 해결할 수 있는 '무엇'을 찾기위해 시도합니다. 예외를 처리할 수 있는 '무엇'의 집합은 오류가 발생한 메서드를 사용하기 위해 불려진 메서드의 순서가 있는 리스트입니다. 메서드의 리스트는 `콜 스택`이라고 알려져 있습니다.

![콜 스택](https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-callstack.gif)

**콜 스택**

이미지 출처: <https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html>

런타임 시스템은 예외를 제어할 수 있는 코드 블럭을 가지고 있는 메서드를 찾기위해 콜스택에서 검색합니다. 이 코드 블럭을 `exception handler`라고 부릅니다. 검색은 오류가 발생한 메서드로부터 시작하여 메서드가 호출된 역순으로 콜 스택을 통해서 진행됩니다. 적절한 핸들러가 발견되면, 런타임 시스템은 예외를 핸들러로 전달합니다. `exception handler`는 던져진 예외 오브젝트의 타입이 핸들러가 제어할 수 있는 타입과 일치하는 경우 적절하다고 간주됩니다.

선택된 `exception handler`는 예외를 잡았다고 말합니다. 만약에 런타임 시스템이 적절한 `exception handler`를 찾지 못하고 콜 스택의 모든 메서드를 철저하게 검색하면, 런타임 시스템(결과적으로 프로그램)은 종료됩니다. (이는 비정상 종료로 볼 수 있습니다.)

![예외 처리기를 찾기 위해 콜 스택을 찾는 과정](https://docs.oracle.com/javase/tutorial/figures/essential/exceptions-errorOccurs.gif)

**`exception handler`를 찾기 위해 콜 스택을 찾는 과정**

이미지 출처: <https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html>

예외를 사용하여 에러를 처리하면 기존 처리 기술에 비해 몇 가지 장점이 있습니다.

기존 처리 기술이라고 한다면, return을 할 때, 에러라는 응답을 해준다거나, C언어에서는 `goto` 문을 사용하는 등의 방법이 있습니다.

1. 에러를 처리하는 코드와 일반 코드가 분리될 수 있습니다.
2. 콜 스택을 따라 에러 전파가 가능해 실질적으로 처리가 될 수 있는 지점에서 처리를 해줄 수 있습니다.
3. 오류를 그룹화 할 수 있고, 분류할 수 있습니다.

### 참고

[오라클 튜토리얼(예외란 무엇인가?)](https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html)
[오라클 튜토리얼(예외의 장점)](https://docs.oracle.com/javase/tutorial/essential/exceptions/advantages.html)

## 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)

자바에서는 `try`, `catch`, `finally` 블록으로 예외 처리를 수행할 수 있습니다.

우리는 자바 튜토리얼에서 나오는 예제코드를 바탕으로 다양한 시나리오에서 발생하는 예외 처리 방법을 학습해보겠습니다.

먼저 코드의 구현 요구사항은 다음과 같습니다.

> `ListOfNumbers` 라는 클래스가 있습니다. 이 클래스가 생성되면 0부터 9까지 10개의 `Integer`타입 요소를 포함하는 `ArrayList`가 생성됩니다.  
> `ListOfNumbers` 클래스는 `writeList`라는 숫자 리스트를 `OutFile.txt`라는 파일에 쓰는 메서드 또한 정의합니다.  
> 이 예제는 `java.io` 패키지에 정의된 아웃풋을 사용합니다.

**주의 사항: 이 예제 클래스는 아직 컴파일 되지 않습니다.**

```java
import java.io.FileWriter;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

public class ListOfNumbers {
  
  private static final int SIZE = 10;

  private List<Integer> list;
  
  public ListOfNumbers() {
    list = new ArrayList<>(SIZE);
    for (int i = 0; i < SIZE; i++) {
      list.add(i); // 예제 코드에선 new Integer(i)로 되어있으나, deprecated 되었고, 차라리 Integer.valueOf(i);를 쓰는 것이 좋습니다.
    }
  }
  
  public void writeList() {
    // FileWriter 생성자는 IOException을 던지고, 이는 반드시 잡혀야합니다.
    PrintWriter out = new PrintWriter(new FileWriter("OutFile.txt"));

    for (int i = 0; i < SIZE; i++) {
      // get(int) 메서드는 IndexOutOfBoundsException을 던지고, 이는 반드시 처리되어야 합니다.
      out.println("Value at: " + 1 + " = " + list.get(i));
    }
    out.close();
  }
}
```

`new FileWriter("OutFile.txt")`는 생성자로, 이 생성자는 file의 아웃풋 스트림을 초기화합니다. 파일이 열릴 수 없다면, 생성자는 `IOException`을 던집니다.

`list.get(i)`는 `ArrayList`의 `get` 메서드로, `ArrayList`의 index 범위를 벗어나는 요청이 들어오는 경우 `IndexOutOfBoundsException을 던지게됩니다.

`ListOfNumbers` 클래스를 컴파일 하려고 하면, 컴파일러가 `FileWriter` 생성자에서 예외를 던진다는 메시지를 출력합니다. 하지만, `get`에 대해서는 에러메시지를 표시하지 않습니다.  
그 이유는 `FileWriter` 생성자에서 던지는 `IOException`은 Checked Exception이고, `get` 메서드에서 던지는 `IndexOutOfBoundsException`은 Unchecked Exception이기 때문입니다.

그럼 이제 예외가 발생할 수 있는 위치에 대해서 익숙해졌으므로, 예외를 처리하는 방법을 작성할 준비가 되었습니다.

### `try` 블럭

예외 처리를 만드는 가장 첫 걸음은 예외를 던질 수 있는 코드를 `try` 블럭 안에 두는 것입니다. `try`블럭은 일반적으로 다음과 같이 생겼습니다.

```java
try {
  code
}
catch and finally blocks ...
```

code라고 적혀있는 부분은 하나 이상의 예외를 던질 수 있는 코드 라인입니다.

예제 코드를 통해 알아봅시다.

```java
public void writeList() {
  PrintWriter out = null;
  try {
    System.out.println("try문에 들어왔습니다.")
    new PrintWriter(new FileWriter("OutFile.txt"));
    for (int i = 0; i < SIZE; i++) {
      out.println("Value at: " + 1 + " = " + list.get(i));
    }
  }
  catch and finally
}
```

만약에 `try` 안의 코드가 실행중 예외가 발생한다면, 예외는 연관된 예외 처리코드에 의해 처리됩니다. `try` 블럭과 연관된 예외 처리 코드를 선언하는 방법은 `catch` 블럭에 선언하는 것입니다.

#### 참고

[오라클 자바 튜토리얼(try 문)](https://docs.oracle.com/javase/tutorial/essential/exceptions/try.html)

### `catch` 블럭

### `finally` 블럭

### 참고

[오라클 자바 튜토리얼(예외 처리)](https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html)

## try-with-resources

## 자바에서 예외를 발생시키는 방법

## 자바가 제공하는 예외 계층 구조

## Exception과 Error의 차이는?

## RuntimeException과 RE가 아닌 것의 차이는?

### Checked Exception, Unchecked Exception

## 커스텀한 예외 만드는 방법

## 예외 포장

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

`try` 블럭의 예외를 처리하기 위해선 `catch` 블럭이 반드시 하나이상 필요합니다. `try` 블럭 바로 다음에 `catch` 블럭이 위치해야 합니다.

```java
try {

} catch (ExceptionType name) {

} catch (ExceptionType name) {

}
```

각 `catch` 블럭은 인자가 나타내는 예외 타입을 처리하는 핸들러입니다. 인자의 타입은 `ExceptionType` 으로 적었는데, 이는 `Throwable` 클래스를 상속하는 예외를 처리하기 원하는 타입의 클래스의 이름을 적는 것을 의미합니다. 핸들러는 `name`을 사용해서 예외를 참조할 수 있습니다.

`catch` 블럭에는 예외 처리를 위한 코드를 작성해주면 됩니다. 런타임 시스템은 던져진 예외가 `ExceptionType`과 일치하는 콜 스택에서 핸들러가 첫 번째 핸들러일 때 예외 처리기를 실행합니다. 시스템은 던져진 객체가 핸들러의 인수에 합법적으로 할당될 수 있는 경우에 일치한다고 판단합니다.

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
  } catch (IndexOutOfBoundsException e) {
    System.err.println("IndexOutOfBoundsException: " + e.getMessage());
  } catch (IOException e) {
    System.err.println("Caught IOException: " + e.getMessage());
  }
}
```

물론 위와 같은 예외처리는 매우 안좋은 예외처리입니다. 예외에 대한 정보를 출력하는 것이나 프로그램을 중지시키는 것보다, 복구 할 수 있다면 복구를 하거나, 사용자에게 결정을 하도록 하거나, 예외 체이닝(포장)을 이용하여 더 높은 수준의 핸들러로 전파할 수 있습니다.

Java 7이후 버전에서는 `catch` 블럭에 여러 개의 예외를 처리할 수 있는 핸들러를 등록할 수 있습니다. 이를 통해서 중복된 코드를 제거하고, 지나치게 광범위한 예외를 포착하려는(Exception과 같은) 유혹을 줄일 수 있습니다.

```java
catch (IOException | SQLException ex) {
  logger.log(ex);
  throw ex;
}
```

`catch` 블럭은 하나 이상의 예외 타입을 처리할 수 있으며, `catch` 의 파라미터는 암묵적으로 `final` 입니다. 이 예제에서 `ex`는 `final`이고, `catch` 블록 내에서 값을 할당할 수 없습니다.

### `finally` 블럭

`finally` 블럭은 `try` 블럭을 벗어날 때 항상 실행됩니다. 이런 방식은 예기치 못한 예외가 발생하더라도 `finally` 블럭의 코드는 동작한다는 것을 의미합니다. 그러나 `finally`는 예외 처리 이상의 용도로도 유용합니다. 프로그래머가 `return`, `continue`, `break`를 사용해서 정리하는 코드를 피하는 것을 방지할 수 있습니다. 정리하는 코드를 `finally` 블럭에 두는 것은 좋은 습관입니다.

참고: try 또는 catch 코드가 실행되는 동안 JVM이 종료되면 finally 블럭이 실행되지 않을 수 있습니다. 마찬가지로, try 또는 catch 코드를 실행하는 동안 스레드가 중단되거나, 종료되면 애플리케이션이 동작중이더라도, finally 블럭이 실행되지 않을 수 있습니다.

`writeList` 메서드의 `try` 블럭에서는 `PrintWriter`를 엽니다. 프로그램은 `writeList` 메서드를 종료하기 전에 해당 스트림을 닫아야 합니다. 이것은 `writeList`의 `try` 블럭이 세 가지 방법 중 하나로 종료될 수 있기 때문에 복잡한 문제가 될 수 있습니다.

1. `FileWriter` 문은 실패하면 `IOException`을 던집니다.
2. `list.get(i)` 문이 실패하면 `IndexOutOfBoundsException`을 던집니다.
3. 모두 성공하면 `try` 블럭은 평범하게 종료됩니다.

런타임 시스템은 `try` 블럭 안에서 어떤 일이 일어나는지와 상관없이 항상 `finally` 블럭을 실행합니다. 따라서 정리 코드를 두기에 최고의 위치입니다.

예제 코드를 통해 알아봅니다.

```java
public void writeList() {
  PrintWriter out = null;
  try {
    System.out.println("try문에 들어왔습니다.")
    new PrintWriter(new FileWriter("OutFile.txt"));
    for (int i = 0; i < SIZE; i++) {
      out.println("Value at: " + 1 + " = " + list.get(i));
    }
  } catch (IndexOutOfBoundsException e) {
    System.err.println("IndexOutOfBoundsException: " + e.getMessage());
  } catch (IOException e) {
    System.err.println("Caught IOException: " + e.getMessage());
  } finally {
    if (out != null) {
      System.out.println("Closing PrintWriter");
      out.close();
    } else {
      System.out.println("PrintWriter not open");
    }
  }
}
```

중요: `finally` 블럭은 리소스 누수를 막기위한 중요 도구입니다. 파일을 닫거나 리소스를 복구할 때, 코드를 `finally` 블럭에 넣어 리소스가 _항상_ 복구되도록 합니다.

혹은 이런 상황에 `try-with-resources` 문을 사용해서 자동으로 시스템 리소스가 필요없는 시점에 릴리즈하도록 하는 것을 고려하십시오.

### 참고

[오라클 자바 튜토리얼(예외 처리)](https://docs.oracle.com/javase/tutorial/essential/exceptions/handling.html)

## try-with-resources

try-with-resource는 하나 이상의 리소스를 정의하는 try와 관련된 문법입니다. 리소스는 프로그램이 끝나기 전에 반드시 종료되어야 하는 객체를 의미합니다. try-with-resource문은 마지막에 리소스를 닫도록 합니다.  
try-with-resource를 사용할 수 있는 리소스는 `java.lang.AutoCloseable`, `java.io.Closeable`을 구현해야 합니다.

아래의 예제는 파일의 첫번째 라인을 읽습니다. `BufferedReader`를 이용하여 읽습니다. `BufferedReader`는 프로그램이 종료될 때, 반드시 닫혀야하는 리소스입니다.

```java
static String readFirstLineFromFile(String path) throws IOException {
  try (BufferedReader br = new BufferedReader(new FileReader(path))) {
    return br.readLine();
  }
}
```

위 예제에서, 리소스는 try-with-resources 문에 정의된 `BufferedReader`입니다. 선언문은 `try` 키워드 바로 뒤의 괄호 안에 표시됩니다. `BufferedReader` 클래스는 자바7 이후로 `java.lang.AutoCloseable` 인터페이스를 구현합니다.  
`BufferedReader` 인스턴스는 try-with-resource문에서 선언되기 때문에 try 문이 정상적으로 완료되거나 갑작스럽게 종료되는 것과 관계없이 닫힙니다. (`IOException`을 던지는 `BufferedReader.readLine` 메서드의 결과 때문입니다.)

자바7 이전에는 `finally` 블록을 사용해서 리소스가 닫히도록 할 수 있었습니다.

```java
static String readFirstLineFromFileWithFinallyBlock(String path) throws IOException {
  BufferedReader br = new BufferedReader(new FileReader(path));
  try {
    return br.readLine();
  } finally {
    if (br != null) {
      br.close();
    }
  }
}
```

그러나, 이 예제에서 `readLine` 메서드와 `close` 모두 예외를 던질 수 있습니다. 따라서 `readFirstLineFromFileWithFinallyBlock` 메서드는 `finally` 블록에서 던져진 예외를 던집니다. 이 때, try의 예외는 억제됩니다.  
이와는 반대로 `readFirstLineFromFile`메서드 예제는 `try` 블록과 try-with-resource 문 모두에서 예외가 던져지면, `try` 블록에서 던져진 예외를 던집니다. try-with-resource의 예외는 억제됩니다. 자바7부터는 억제된 예외를 찾아올 수 있습니다.  
이는 다음 섹션에서 설명하겠습니다.

우리는 하나 이상의 리소스를 try-with-resource에 정의할 수 있습니다. 이 때, 유의할 점은 리소스 선언과 반대의 순서로 리소스가 닫힌다는 점입니다.  
또한 try-with-resource는 일반적인 `try`문 처럼 사용될 수 있고, `catch`, `finally`는 리소스가 닫힌 후에 실행됩니다.

### 참고

[오라클 자바 튜토리얼(try-with-resources)](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

### 억제된 예외

try-with-resources 문과 연결된 코드 블럭에서 예외가 발생할 수 있습니다. 이 예외는 여러개 발생할 수 있으며, 위에서 설명한 것과 같이 `try` 블록에서 던져진 예외에 의해 try-with-resource에서 발생한 예외는 억제됩니다.  
억제된 예외를 가져오는 방법은 `Throwable.getSuppressed` 메서드를 호출해서 억제된 예외를 찾아올 수 있습니다.

### `AutoCloseable`과 `Closeable` 인터페이스 구현

위의 두 인터페이스에 대한 자세한 정보는 JavaDoc을 참고하세요. `Closeable` 인터페이스는 `AutoCloseable` 인터페이스를 상속합니다. `Closeable` 인터페이스의 `close` 메서드는 `IOException` 타입의 예외를 던지는 반면, `AutoCLoseable` 인터페이스의 `close` 메서드는 `Exception` 타입의 예외를 던집니다. 결과적으로 `AutoCloseable` 인터페이스의 하위 클래스는 `close` 메서드를 오버라이딩 할 때, `IOException`과 같은 좀 더 상세한 예외로 던지거나, 예외를 전혀 발생시키지 않을 수 있습니다.

### 예외 처리 시나리오

합쳐진 `writeList` 메서드

```java
public void writeList() {
  // FileWriter 생성자는 IOException을 던지고, 이는 반드시 잡혀야합니다.
  PrintWriter out = null;
  
  try {
    System.out.println("try문 진입");
    
    out = new PrintWriter(new FileWriter("OutFile.txt"));
    for (int i = 0; i < SIZE; i++) {
      // get(int) 메서드는 IndexOutOfBoundsException을 던지고, 이는 반드시 처리되어야 합니다.
      out.println("Value at: " + 1 + " = " + list.get(i));
    }
  } catch (IndexOutOfBoundsException e) {
    System.err.println("IndexOutOfBoundsException 잡힘" + e.getMessage());
  } catch (IOException e) {
    System.err.println("IOException 잡힘: " + e.getMessage());
  } finally {
    if (out != null) {
      System.out.println("PrintWriter 닫는중");
      out.close();
    } else {
      System.out.println("PrintWriter는 안열려있습니다.");
    }
  }
}
```

코드는 3가지 종료 가능성이 있고, 이는 두 종류로 구분할 수 있습니다.

1. 비정상 종료: `try` 블럭을 실행중에 예외가 발생하여 실패하는 경우 `IOException` 혹은 `IndexOutOfBoundsException`이 발생할 수 있습니다.
2. 정상 종료

#### 비정상 종료 발생시

`IOException`은 `FileWriter`가 여러가지 이유로 던지는 예외로, 이 예외가 던져지면 `try` 블록의 실행은 중지되고, 적합한 예외 처리기를 발견할 때까지 검색합니다. 검색이 되면 매칭되는 예외 처리기로 예외가 전달되며 실행 후 `finally` 블록이 실행됩니다.  
마찬가지로 `IndexOutOfBoundsException`도 동일한 동작을 수행합니다.

#### 정상 종료 상황

정상종료 상황에서는 `catch` 블럭의 코드는 실행되지 않고 `try` 블럭의 코드만 전부 실행된 다음 `finally` 블럭의 코드가 실행됩니다.

### 예외를 메서드 내에서 처리할 수 없는 경우

예외를 메서드 내에서 처리할 수 없는 경우엔, 더 높은 위치의 메서드가 처리할 수 있도록 예외를 다시 던져주는 것이 좋습니다.  
이런 메서드 상위로 예외를 던지는 것은 메서드 선언부에 `throws` 키워드를 입력하고, `throw`될 모든 예외를 쉼표로 구분된 목록으로 구성하면 됩니다.

예를 들어 `writeList` 메서드의 경우에는

```java
public void writeList() throws IOException, IndexOutOfBoundsException {
```

과 같이 표현할 수 있습니다. 하지만 `IndexOutOfBoundsException`은 Unchecked Exception이므로 생략할 수 있습니다.

```java
public void writeList() throws IOException {
```

## 자바에서 예외를 발생시키는 방법

## 자바가 제공하는 예외 계층 구조

## Exception과 Error의 차이는?

## RuntimeException과 RE가 아닌 것의 차이는?

### Checked Exception, Unchecked Exception

## 커스텀한 예외 만드는 방법

## 예외 포장

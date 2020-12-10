# 4주차: 제어문

## 학습할 것

- [선택문](#선택문)
- [반복문](#반복문)

## 과제(선택)

- [JUnit5 학습하기](#junit5-학습하기)
- [live-study 대시보드 만들기](#live-study-대시보드-만들기)
- [LinkedList 구현하기](#linkedlist-구현하기)
- [Stack 구현하기](#stack-구현하기)
- [ListNode로 Stack 구현하기](#listnode로-stack-구현하기)
- [Queue 구현하기](#queue-구현하기)

---

일반적으로 우리가 작성한 소스코드는 위에서 아래로 실행됩니다. 그러나, 우리가 앞으로 학습할 제어문은 실행 흐름을 끊고, 조건에 따라 실행되거나, 반복해서 실행되거나, 분기처리를 통해 우리의 프로그램이 조건에 따라 실행될 수 있도록 해줍니다.

자바에는 조건에 따라 실행되도록 하는 문법인 선택문(`if-then`, `if-then-else`, `switch`), 반복해서 실행되도록 하는 문법인 반복문(`for`, `while`, `do-while`)이 있으며, 분기를 수행하는 문법(`break`, `continue`, `return`) 또한 있습니다.

[자바 튜토리얼](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/flow.html)

## 선택문

### `if-then`, `if-then-else` 문

#### `if-then` 문

`if-then`문은 가장 기본적인 제어문입니다. 이 코드는 블록 안에 있는 코드를 오직 조건이 충족될 때만 실행됨을 알려줍니다.  
예를 들어, `Bicycle` 클래스는 자전거가 움직일 때만 속도를 줄일 수 있도록 합니다.

```java
void applyBrakes() {
  // 자전거는 움직이고 있어야만 합니다.
  if (this.isMoving) {
    // then 절입니다. 현재 속도를 줄입니다.
    this.currentSpeed--;
  }
}
```

이 평가가 `false` 값이라면 자전거가 움직이지 않고 있다는 것을 의미하고, 제어는 `if-then` 문 끝으로 이동하게 됩니다.

추가로, `then` 절이 하나의 문만 가질 경우 시작과 끝의 중괄호는 선택적입니다.

하지만 저는 개인적으로 모든 if문에 괄호를 사용하는 것을 선호합니다. 일반적으로 코드의 취약성이 높아지는 smell이라고 생각하며, 문장이 두 줄이 될 경우 중괄호를 추가해주어야 할 뿐더러 잊기도 쉽습니다.

```java
if (this.isMoving)
    this.currentSpeed--;
// 만약 한 문장이 더 추가되어야 한다면?
// 중괄호도 추가해주어야 함으로 유지보수성이 떨어지게 됩니다.
```

#### `if-then-else` 문

`if-then-else` 문은 `if` 절의 평가가 `false`인 경우에 실행될 두번째 경로를 제공해줍니다. 아래의 예제는 이미 자전거가 멈춰있는 경우에 이미 멈춰있다는 에러 메시지를 출력합니다.

```java
void applyBrakes() {
  if (this.isMoving) {
    this.currentSpeed--;
  } else {
    System.err.println("자전거가 이미 멈춰있습니다!");
  }
}
```

`IfElseDemo` 라는 프로그램은 `testscore`라는 값에 따라 `grade`가 결정됩니다.

```java
class IfElseDemo {
  public static void main(String[] args) {

    int testscore = 76;
    char grade;

    if (testscore >= 90) {
      grade = 'A';
    } else if (testscore >= 80) {
      grade = 'B';
    } else if (testscore >= 70) {
      grade = 'C';
    } else if (testscore >= 60) {
      grade = 'D';
    } else {
      grade = 'F';
    }
    System.out.println("Grade = " + grade);
  }
}
```

이 프로그램의 출력은 다음과 같을 것입니다.

```java
Grade = C
```

이 코드는 조건문 두 개를 만족하지만, 보다 위에 위치한 조건문에 위치한 식만 평가되고 아래에 존재하는 식은 평가되지 않습니다.

[자바 튜토리얼(if, if else)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html)

### `switch` 문

`if-then`문, `if-then-else` 문과 달리 `switch`문은 여러 경로가 실행될 수 있습니다. `switch`문은 `byte, short, char`와 `int`와 같은 기본 데이터 타입과 함께 동작합니다. 또한, `enumerated` 타입(`Enum Type`)과도 함께 사용할 수 있습니다. `String` 클래스와 더불어 `wrapper` 클래스도 사용할 수 있습니다.

`SwitchDemo` 예제코드를 보시면, `month`라는 `int`타입의 변수의 값은 월을 의미합니다. 이 코드는 `month` 값을 바탕으로 `switch`문을 통해 해당 월의 이름을 보여줍니다.

```java
public class SwitchDemo {
  public static void main(String[] args) {
    int month = 8;
    String monthString;
    switch (month) {
      case 1: monthString = "January";
              break;
      case 2: monthString = "February";
              break;
      case 3: monthString = "March";
              break;
      case 4: monthString = "April";
              break;
      case 5: monthString = "May";
              break;
      case 6: monthString = "June";
              break;
      case 7: monthString = "July";
              break;
      case 8: monthString = "August";
              break;
      case 9: monthString = "September";
              break;
      case 10: monthString = "October";
               break;
      case 11: monthString = "November";
               break;
      case 12: monthString = "December";
               break;
      default: monthString = "Invalid month";
    }
    System.out.println(monthString);
  }
}
```

이 경우에 표준 출력의 결과는 `August`로 나올 것입니다.

`switch`문의 body는 `switch block`으로 알려져 있습니다. `switch block`안에 있는 코드들은 하나 이상의 case 또는 default로 레이블 될 수 있습니다. `switch`문의 표현식이 평가되면 만족하는 모든 `case`의 코드들이 실행됩니다.

그리고 switch문은 `if-then-else` 문으로도 변경가능합니다.

`if-then-else`문 또는 `switch`문을 선택하는 기준은 가독성과 평가문의 표현에 있습니다. `if-then-else`문은 값의 범위, 조건등으로 평가할 수 있고, `switch`문은 하나의 정수, 열거형 값, `String` 객체로 평가할 수 있습니다.

또 한가지 흥미로운 점은 `break` 문입니다. 매 `break` 문은 이를 감싸고 있는 `switch`문을 종료시킵니다. 제어 흐름은 `switch` 블록 다음의 첫번째 문에서 계속됩니다. `break`문은 필요합니다. 이 `break`문이 없으면, `switch`블록은 계속해서 떨어집니다.(한 번 조건에 맞는다면, 그 다음부터는 조건에 관계없이 실행됩니다.) 이를 _fall through_ 라고 합니다.

제가 생각하기엔 `switch`문과 `if-then-else`는 사용처가 다른느낌을 많이 받았는데, 비슷하지만 많이 다르다고 생각합니다.

기술적으로, 가장 마지막 `break`는 필요하지 않습니다. 그리고, `break`를 사용하는 편이 오류를 줄일 수 있기 때문에 권장됩니다. `default` 섹션은 `case`에서 처리되지 않는 모든 부분을 명시적으로 처리합니다.

#### `switch`문에서 `String` 사용하기

Java SE 7 이후 버전부터는 `String` 객체를 `switch`문의 평가식으로 사용할 수 있습니다. 다음의 예제는 각 달의 이름으로 숫자 값을 구하는 것입니다.

```java
public class StringSwitchDemo {

  public static int getMonthNumber(String month) {

    int monthNumber = 0;

    if (month == null) {
      return monthNumber;
    }

    switch (month.toLowerCase()) {
      case "january":
          monthNumber= 1;
          break;
      case "february":
          monthNumber = 2;
          break;
      case "march":
          monthNumber = 3;
          break;
      case "april":
          monthNumber = 4;
          break;
      case "may":
          monthNumber = 5;
          break;
      case "june":
          monthNumber = 6;
          break;
      case "july":
          monthNumber = 7;
          break;
      case "august":
          monthNumber = 8;
          break;
      case "september":
          monthNumber = 9;
          break;
      case "october":
          monthNumber = 10;
          break;
      case "november":
          monthNumber = 11;
          break;
      case "december":
          monthNumber = 12;
          break;
      default:
          monthNumber = 0;
          break;
    }

    return monthNumber;
  }

  public static void main(String[] args) {

    String month = "August";

    int returnedMonthNumber = StringSwitchDemo.getMonthNumber(month);

    if (returnedMonthNumber == 0) {
      System.out.println("Invalid month");
    } else {
      System.out.println(returnedMonthNumber);
    }
  }
}
```

이 코드의 결과는 8입니다.

`switch` 평가식의 `String`은 `String.equals` 메서드를 이용해 비교됩니다. 이 과정에서 `toLowerCase` 메서드를 통해 case의 구분 없이 비교하게 됩니다.

가급적이면 `String`과 같이 객체를 Switch로 사용할 때는 `NullPointerException`을 주의해야 합니다. `null`을 체크하는 로직을 **꼭** 추가합시다.

### 참고

[오라클 자바 튜토리얼(switch문)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html)

## 반복문

### `while` 및 `do-while` 문

`while`문은 조건이 `true`인 동안 계속해서 블록안의 문장을 반복합니다. 이를 자바에서는 이렇게 표현합니다.

```java
while (expression) {
  statement(s)
}
```

`while` 문은 `boolean` 값을 리턴하는 표현식을 평가합니다. 표현식이 `true`로 평가된다면, `while`문은 `while`문의 블록 안에 있는 문장들을 실행합니다. `while`문은 식이 `false`가 될 때 까지 평가하고, 블록안의 항목을 반복합니다.

아래의 코드는 1부터 10까지 print하는 예제입니다.

```java
class WhileDemo {
  public static void main(String[] args) {
    int count = 1;
    while (count < 11) {
      System.out.println("Count is: " + count);
      count++;
    }
  }
}
```

조건문에 `true`를 입력해서 무한 루프를 만들 수도 있습니다.

```java
while (true) {
  // code
}
```

자바는 `do-while`문 문법도 제공합니다. 다음과 같이 쓸 수 있습니다.

```java
do {
  statement(s)
} while (expression);
```

`do-while`문과 `while`문의 차이는 조건식의 평가와 반복되는 부분의 순서에 있습니다. `while`문은 식을 먼저 평가하고 반복을 하는 반면, `do-while`문은 일단 한 번 실행하고 반복여부를 평가합니다. 즉, `do-while`문은 적어도 한 번의 실행을 보장하게 됩니다.

### 참고

[오라클 자바 튜토리얼(whlie문과 do-while문)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/while.html)

### `for` 문

`for`문은 범위 반복을 하는 간결한 방법을 제공합니다. 일반적인 `for`문은 다음과 같이 표현됩니다.

```java
for (initialization; termination; increment) {
  statement(s)
}
```

`for`문을 사용할 때 유의할 점이 있습니다.

- `initialization` 표현식은 루프를 초기화합니다. 이것은 루프가 실행되기 전 한 번만 실행됩니다.
- `termination` 표현식이 `false` 값으로 평가될 때까지 반복하게 됩니다.
- `increment` 표현식은 반복이 끝날 때마다 실행됩니다. 이 값은 증가하거나 감소하게 할 수 있습니다.

```java
class ForDemo {
  public static void main(String[] args) {
    for (int i = 1; i < 11; i++) {
      System.out.println("Count is: " + i);
    }
  }
}
```

결과:

```text
Count is: 1
Count is: 2
Count is: 3
Count is: 4
Count is: 5
Count is: 6
Count is: 7
Count is: 8
Count is: 9
Count is: 10
```

`i`가 어떻게 선언되고, 초기화되며, 어떤 방식으로 평가되고, 증가하는 지를 알아야 합니다. 이 변수가 루프 외부에서 필요하지 않는 경우 초기화 식에서 선언하는 것이 좋습니다. 그렇게 하면 변수가 딱 사용되는 범위만큼 수명을 갖게되고, 오류가 줄어들 수 있습니다. `for`문에는 `i`, `j`, `k` 와 같은 변수들이 주로 사용됩니다.

`for`문의 세 식은 모두 선택사항입니다. 모두 비워두면 무한 루프가 됩니다.

```java
for (;;) {
  // 코드 입력
}
```

`enhanced for`문은 배열이나 컬렉션 같이 연속된 정보를 접근하는 방법입니다. `for`문 보다 훨씬 간결하고 읽기 쉽습니다.

```java
class EnhancedForDemo {
  public static void main(String[] args) {
    int[] numbers = {1, 2, 3, 4, 5, 6, 7, 8 ,9, 10};

    for (int item : numbers) {
      System.out.println(item);
    }
  }
}
```

결과

```text
1
2
3
4
5
6
7
8
9
10
```

가능하다면 이 형식을 사용하는 것을 권장합니다. 일단 가독성이 좋고, 내부적으로 iterator를 사용하는 경우 인덱스를 통한 접근보다 빠른 경우가 있기 때문입니다.

### 참고

[오라클 자바 튜토리얼(for문)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html)

## 분기문

### `break`문

`break`문은 두가지 형태가 있습니다. label된 것과 label되지 않은 것이 그것입니다. label 되지 않은 것은 일반적으로 우리가 볼 수 있는 용례입니다.

`break`문의 역할은 `break`문을 감싸고 있는 블록을 즉시 벗어나는 것을 의미합니다. 반복도중이라면 반복을 진행하지 않고 탈출합니다.

label된 `break`문의 역할은 해당 label에 해당하는 것을 탈출함을 의미합니다. 이는 중첩된 구조에서 의미가 있으며, 일반적으로 많이 사용하는 방법은 아닙니다.

```java
class BreakWithLabelDemo {

  int[][] arrayOfInts = {
    { 32, 87, 3, 509 },
    { 12, 1076, 2000, 8 },
    { 622, 127, 77, 955 }
  };
  int searchfor = 12;

  int i;
  int j = 0;
  boolean foundIt = false;

search:
  for (i = 0; i < arrayOfInts.length; i++) {
    for (j = 0; j < arrayOfInts[i].length; j++) {
      if (arrayOfInts[i][j] == searchfor) {
        foundIt = true;
        break search;
      }
    }
  }

  if (fountIt) {
    System.out.println("Found " + searchfor + " at " + i + ", " + j);
  } else {
    System.out.println(searchfor + " not in the array");
  }
}
```

결과는 다음과 같습니다.

```text
Found 12 at 1, 0
```

`break`문은 label이 표시된 블록을 종료하고, 제어 흐름을 그 밖으로 전달합니다.

### `continue`문

`continue`문은 현재 반복을 생략합니다. 따라서 반복문인 `for`, `while`, `do-while`문에서 사용할 수 있습니다.

```java
class ContinueDemo {
  public static void main(String[] args) {
    String searchMe = "peter piper picked a peck of pickled peppers";
    int max = searchMe.length();
    int numPs = 0;

    for (int i = 0; i < max; i++) {
      // p가 아니면 넘어간다.
      if (searchMe.charAt(i) != 'p') {
        continue;
      }

      // p의 개수를 증가시킨다.
      numPs++;
    }
    System.out.println("Found " + numPs + " p's in the string.");
  }
}
```

결과는 다음과 같습니다.

```text
Found 9 p's in the string.
```

label된 `continue`문은 위의 `break`문과 동일하게 동작합니다.

### `return`문

`return`문은 현재 메서드를 탈출하는 기능으로 실행 흐름을 메서드를 호출한 위치로 반환합니다 `return`문은 두 가지 형태가 있습니다. 값을 반환하는 것과 그렇지 않은 것 두 가지 입니다. 값을 반환하기 위해서는 `return` 키워드 뒤에 반환할 값이나 변수를 입력하면 됩니다.

반환하는 타입은 메서드에 선언된 반환 타입과 일치해야 합니다. `void` 라면 반환하지 않습니다.

### 참고

[오라클 자바 튜토리얼(분기문)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/branch.html)

## JUnit5 학습하기

## live-study 대시보드 만들기

## LinkedList 구현하기

## Stack 구현하기

## ListNode로 Stack 구현하기

## Queue 구현하기

```

```

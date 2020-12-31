# 7주차 패키지

## 학습할 것

- [package 키워드](#package-키워드)
- [import 키워드](#import-키워드)
- [클래스패스](#클래스패스)
- [CLASSPATH 환경변수](#classpath-환경변수)
- [-classpath 옵션](#-classpath-옵션)
- [접근지시자](#접근지시자)

## package 키워드

패키지는 클래스들과 인터페이스들을 번들링 하는 단위입니다.

우리가 패키지를 사용하는 이유는 위의 요소들을 찾거나 사용하기 쉽게하고, 이름의 충돌을 피하고, 접근을 제어하고, 연관된 타입을 번들하는 용도로 사용합니다.

패키지의 정의: 패키지는 연관된 타입을 그룹핑해서 접근 보호와 네임 스페이스 관리를 해줍니다. 여기서 타입은 클래스, 인터페이스, 열거 및 애노테이션 타입을 의미합니다. 열거형과 애노테이션 타입은 클래스와 인터페이스의 특별한 유형이므로 여기서는 타입을 클래스 및 인터페이스라고 하겠습니다.

자바의 일부인 타입은 기능별로 클래스를 번들하는 다양한 패키지의 멤버입니다. 기본 클래스는 `java.lang`에 있고, 읽기 및 쓰기용 클래스(입력, 출력)는 `java.io`에 있습니다. 패키지에 타입을 넣을 수 있습니다.

Graphic 객체를 나타내는 Circle, Rectangle, Line, Point 클래스 들을 작성하고, 그 중 마우스 드래그가 가능한 경우 Draggable 인터페이스를 구현한다고 가정합니다.

```java
// Draggable.java
public interface Draggable {
  ...
}

// Graphic.java
public abstract class Graphic {
  ...
}

// Circle.java
public class Circle extends Graphic implements Draggable {
  ...
}

// Rectangle.java
public class Rectangle extends Graphic implements Draggable {
  ...
}

// Point.java
public class Point extends Graphic implements Draggable {
  ...
}

// Line.java
public class Line extends Graphic implements Draggable {
  ...
}
```

우리는 위와 같은 클래스와 인터페이스를 한 패키지로 묶어야 합니다. 그 이유는 다음과 같습니다.

- 나와 다른 프로그래머가 이런 타입들의 연관성을 쉽게 확인할 수 있습니다.
- 나와 다른 프로그래머가 위치를 빠르게 찾을 수 있습니다.
- 패키지가 새로운 네임 스페이스를 생성하기 때문에 다른 위치의 패키지와 충돌하지 않습니다.
- 패키지 내의 타입들은 private이 아닌 이상 서로간에 액세스를 할 수 있지만, 패키지 외부에서는 액세스를 제한할 수 있습니다.

패키지를 생성하려면 패키지의 이름을 정하고, 소스파일의 맨 위에 `package` 키워드와 함께 지정한 패키지의 이름을 입력합니다.

패키지 문은 반드시 소스파일의 첫번째 라인에 위치해야합니다. 그리고, 이는 오직 하나만 지정이 가능합니다.

참고로 한 소스파일에 여러개의 타입을 넣을 때, 하나의 타입만 `public`일 수 있고, 이는 소스 파일의 이름과 같아야 합니다.

그리고, `public` 타입과 `non-public` 타입을 한 소스파일의 같은 레벨로 놓을 수 있지만 권장되지 않습니다. 이런 경우는 매우 작거나, 아주 밀접한 관련이 있을 때에만 사용합니다. 이 경우 외부에서 접근 가능한 타입은 `public` 타입만이고, `non-public` 타입은 패키지 내부에서만 접근 가능합니다.

위의 예제에 `graphics` 라는 패키지 이름을 지정하겠습니다.

```java
// Draggable.java
package graphics;

public interface Draggable {
  ...
}

// Graphic.java
package graphics;

public abstract class Graphic {
  ...
}

// Circle.java
package graphics;

public class Circle extends Graphic implements Draggable {
  ...
}

// Rectangle.java
package graphics;

public class Rectangle extends Graphic implements Draggable {
  ...
}

// Point.java
package graphics;

public class Point extends Graphic implements Draggable {
  ...
}

// Line.java
package graphics;

public class Line extends Graphic implements Draggable {
  ...
}
```

기본적으로 패키지를 입력하지 않으면 이름 없는 패키지에 위치하게 됩니다. 이는 프로젝트 초기의 매우 작거나 임시로 사용하는 애플리케이션에만 의미가 있으며, 그게 아니라면 이름을 지정해줘야 합니다.

### 패키지 네이밍

패키지 명은 네임 스페이스라고 했습니다. 즉, 같은 이름을 가진다면 겹치게 됩니다. 따라서 최대한 겹치지 않도록 이름을 짓는 것이 좋습니다.

자바에서는 겹치는 문제를 방지하기 위해서 규칙을 정했습니다.

패키지 이름은 클래스, 인터페이스 이름과 겹치지 않도록 모두 소문자로 작성합니다.

그리고 회사에서 사용한다면 도메인의 역순으로 패키지명을 사용하면 좋습니다. 예를들어, whiteship.me가 주소라면 me.whiteship.mypackage와 같은 식으로 패키지명을 작성합니다.

회사 내에서 충돌이 날 수도 있으므로, 프로젝트 명이나 팀 명을 붙여서 처리해야 합니다.

자바 언어에서 사용하는 패키지는 java, javax 입니다.

인터넷 도메인이 올바르지 않은 경우도 있습니다. 예를 들어, 하이픈이 들어간 경우, 특수문자가 포함된 경우, 패키지명으로 시작할 수 없는 문자인 경우, 예약어인 경우 등이 있습니다. 이럴 경우 underscore(\_)로 대체하는 것을 권장합니다.

### 패키지 멤버의 사용

패키지를 구성하는 타입들을 통틀어서 패키지 멤버라고 합니다.

`public` 패키지 멤버를 외부에서 사용하는 방법은 다음과 같습니다.

- 풀패키지 경로로 멤버를 참조합니다.
- 특정 패키지 멤버를 import합니다.
- 패키지 멤버의 전체 패키지를 import합니다.

위 세 방식은 모두 다른 상황에서 사용합니다.

### 풀패키지 경로로 멤버 참조하기

풀패키지 경로로 멤버를 import하는 방법입니다. 이는 자주 사용되지 않습니다. 왜냐하면, 이를 읽기도 힘들뿐더러 보기에 안좋기 때문입니다.

그래서 아래에서 설명할 import문을 사용합니다. 다만, 중복되는 이름이 존재하는 경우에는 둘 중 하나는 풀패키지 경로로 참조해야합니다.

```java
graphics.Line line = new graphics.Line();
```

### 패키지 멤버 import 하기

특정 멤버를 현재 파일에 import하고 싶은 경우 `import` 문을 `package` 문 다음의 파일 시작부분에 넣는 방법이 있습니다.

```java
import graphics.Rectangle;

Rectangle rectangle = new Rectangle();
```

이것도 잘 동작하지만, 엄청 많은 멤버들을 한 패키지에 넣고자 한다면 무리가 있을겁니다.

### 전체 패키지 import하기

그래서 나온것이 바로 전체 패키지 import 방식입니다. 애스터리스크(\*)를 사용해서 와일드카드로 사용할 수 있습니다.

```java
import grahpics.*;

Circle circle = new Circle();
Rectangle rectangle = new Rectangle();
```

위와 같은 방식으로 사용할 수 있지만 패턴 매칭은 되지 않습니다. 예를들면, `graphics.C*`은 지원되지 않습니다.

우리의 편의를 위해서 Java 컴파일러는 `java.lang`패키지와, 현재 패키지를 자동으로 import 해줍니다.

### static import 문

static final 필드나 static method에 자주 액세스 해야하는 경우, static import 문을 활용해서 코드를 보다 깔끔하게 정리할 수 있습니다.

불필요한 접두사를 제거하고 사용할 수 있도록 해줍니다.

`java.lang.Math` 클래스를 예로 들어보겠습니다.

```java
public static final double PI = 3.11592653589793;

public static double cos(double a) {
  ...
}
```

일반적으로 Math 클래스의 static 멤버를 사용하려면 다음과 같이 사용할 겁니다.

`double r = Math.cos(Math.PI * theta);`

하지만 다음과 같은 문장을 사용해서 줄여서 사용할 수 있습니다.

`import static java.lang.Math.PI;` or `import static java.lang.Math.*`

그렇게 하면 이런식으로 사용할 수 있습니다.

```java
import static java.lang.Math.*;

...
double r = cos(PI * theta);
```

자주 사용하는 상수나 메서드들은 이를 포함하는 고유한 클래스를 만들어 사용하는 편이 좋습니다.

하지만, 이를 너무 과도하게 사용하는 경우 코드를 읽는 사람 입장에서 굉장히 어렵게 느껴질 수 있습니다. 코드를 쉽게 읽을 수 있도록 하는 것에 초점을 맞추는게 좋습니다.

### 소스와 클래스파일 관리하기

자바 플랫폼의 많은 구현들은 소스와 클래스 파일을 관리하기 위해 계층적인 파일 시스템에 의존합니다. 하지만 자바 언어 스펙상으로는 이를 필요로 하지 않습니다.

하지만 우리는 계층적인 파일 시스템에서 작업을 하므로 다음과 같은 전략을 취합니다.

클래스, 인터페이스, Enum, 애노테이션 타입에 대한 소스코드는 `타입의 이름.java`와 같은 텍스트 파일에 기록합니다.

```java
// Rectangle.java
package graphic;

public class Rectangle {
  ...
}
```

위와 같은 식으로 파일을 작성합니다. 그런 다음 다음과 같이 디렉토리에 파일을 넣습니다.

`...../grahpics/Rectangle.java`

윈도우즈에선 `\`를 사용하지만 UNIX Like 운영체제의 경우 `/`를 사용합니다.

이렇게 하면 다음과 같이 표현할 수 있습니다.

- 클래스 이름 - graphics.Rectangle
- 파일 경로 이름 - grahpics/Rectangle.java

그리고 우리는 보통 앞에 도메인을 붙여서 패키지를 만듭니다. 예를들어 `com.example.graphics.Rectangle` 과 같은 식으로 관리합니다.

`...../com/example/graphics/Rectangle.java` 와 같은 경로로 표현될 수 있습니다.

이런 소스 파일들은 자바 컴파일러를 이용해 컴파일 하면 동일한 이름의 `.class` 확장자 파일이 생성됩니다.  
그리고 또 한가지 사실은 각각의 클래스마다 클래스 파일이 생성된다는 것입니다.

```java
package com.example.graphics;

public class Rectangle {
  ...
}

class Helper {
  ...
}
```

위와 같은 파일을 컴파일 하는 경우 `com/example/grahpics/Rectangle.class`, `com/example/grahpics/Helper.class` 파일이 생성됩니다.

클래스 파일 경로가 꼭 자바 파일과 같은 경로에 있을 필요는 없습니다. 소스를 공개하지 않고 컴파일 된 코드만 제공할 수 있다는 의미지요.

이를 `classes` 디렉토리에 넣어서 관리하는 방식을 사용하면 컴파일러나 JVM이 프로그램에서 사용하는 모든 타입을 찾을 수 있고, 이렇게 할 수 있도록 소스와 클래스파일을 관리해야합니다.

`classes` 디렉토리의 경로를 시스템 변수 CLASSPATH에 설정하는데 이를 `classpath` 라고 합니다. 컴파일러와 JVM은 `.class` 파일을 패키지 이름과 함께 클래스 패스에 생성합니다.

그리고 이 클래스 패스는 세미콜론(windows), 콜론(UNIX)로 구분된 여러 경로가 포함될 수 있습니다.

### CLASSPATH 시스템 변수 설정하기

CLASSPATH 변수를 표시하려면, 아래의 명령어를 입력하세요. 유닉스 환경만 정리합니다.

`% echo $CLASSPATH`

CLASSPATH 변수를 지우려면 다음과 같이 입력하세요.

`% unset CLASSPAth; export CLASSPATH`

CLASSPATH를 설정하려면 다음과 같이 입력하시면 됩니다.

`% CLASSPATH=/home/george/java/classes; export CLASSPATH`

### 참고

- [오라클 자바 튜토리얼(패키지)](https://docs.oracle.com/javase/tutorial/java/package/index.html)

## -classpath 옵션

JDK 도구들에는 `-classpath` 라는 옵션을 줄 수 있습니다. `java`, `jdb`, `javac`, `javah` 라는 도구들이 이 옵션을 사용할 수 있는 도구들입니다.

이 옵션을 사용하면 도구들이 실행되는 동안 입력한 경로가 CLASSPATH 환경변수를 대체하게 됩니다.

약어로 `-cp`를 쓸 수도 있습니다.

### 참고

- [오라클 공식 문서(Classpath)](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)

## 접근지시자

접근 제어자는 우리가 해당 필드가 접근할 수 있는 클래스들을 제어할 수 있도록 해줍니다.

| 접근 제어자          | 클래스 내부 | 패키지 내부 | 패키지 외부의 서브 클래스 | 전체 |
| -------------------- | ----------- | ----------- | ------------------------- | ---- |
| public               | Y           | Y           | Y                         | Y    |
| protected            | Y           | Y           | Y                         | N    |
| no modifier(default) | Y           | Y           | N                         | N    |
| private              | Y           | N           | N                         | N    |

캡슐화 때문에 일반적으로 필드는 `private`으로 만듭니다. 하지만 외부에서 값에 접근하고 싶을 수 있기 때문에, 간접적으로 값에 접근할 수 있도록 할 수 있습니다.

보통 값을 가져오는 메서드를 `getName()` 과 같은 식으로 작성하고, 이를 getter라고 합니다.

값을 설정하는 메서드는 `setName(toBe)` 과 같은 식으로 작성하고, 이를 setter라고 합니다. 보통 객체지향적이지 않은 경우가 많기 때문에 사용을 지양하는 편이 좋습니다.

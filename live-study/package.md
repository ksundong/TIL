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

### 참고

- [오라클 자바 튜토리얼(패키지)](https://docs.oracle.com/javase/tutorial/java/package/index.html)

## import 키워드

## 클래스패스

## CLASSPATH 환경변수

## -classpath 옵션

## 접근지시자

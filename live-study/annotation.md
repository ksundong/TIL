# 12주차: 애노테이션

## 학습할 것

- [애노테이션이란?](#애노테이션이란?)
- [애노테이션 정의하는 방법](#애노테이션-정의하는-방법)
- [`@retention`](#retention)
- [`@target`](#target)
- [`@documented`](#documented)
- [애노테이션 프로세서](#애노테이션-프로세서)

## 애노테이션이란?

애노테이션은 프로그램의 소스코드에 다른 프로그램을 위해서 정보를 사전에 약속된 형식으로 표현한 것을 의미합니다. 애노테이션은 프로그램의 실행에는 별 다른 영향을 미치지 않고, 다른 프로그램에 유용한 정보를 제공할 수 있습니다.

이러한 애노테이션은 정보를 제공받을 프로그램에서 지정한 종류와 형식을 따라야만 의미가 있습니다.  
애너테이션은 JDK에서 기본적으로 제공되는 애노테이션과 특정 프로그램에서 제공하는 애노테이션이 있습니다. 

JDK 에서 제공하는 애노테이션은 주로 컴파일러를 위한 것이고, 새로운 애노테이션을 정의할 때 사용하는 메타 애노테이션 또한 제공합니다.

### 표준 애노테이션

| 애노테이션             | 설명                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `@Override`            | 컴파일러에게 오버라이딩 되는 메서드임을 알려줍니다.          |
| `@Deprecated`          | 기존의 것을 함부로 삭제하기 어려우므로, 이 코드는 다른 것으로 대체되었으므로 사용하지 않는 것을 권장함을 알려줍니다. |
| `@SuppressWarnings`    | 경고의 대상을 억제합니다.<br />- "deprecation": `@Deprecated` 된 대상 사용시 발생하는 경고 억제 <br />- "unchecked": 제네릭 타입을 지정하지 않았을 때 발생하는 경고 무시<br />- "rawtypes": 로(raw) 타입을 사용해서 발생하는 경고 무시<br />- "varargs": 가변인자의 타입이 제네릭인 경우 발생하는 경고 무시(가변인자는 배열로 변환됩니다.)<br />대괄호로 여러개 지정할 수도 있습니다. |
| `@SafeVarargs`         | 메서드에 선언된 가변인자의 타입이 *non-reifiable* 타입인 경우 해당 메서드를 사용하는 코드에서 "uncheked" 경고가 발생하는데 이를 무시합니다.(JDK1.7) |
| `@FunctionalInterface` | 함수형 인터페이스를 선언할 때, 컴파일러가 이 인터페이스가 함수형 인터페이스로 선언가능한지 체크해줍니다.(추상메서드가 하나만 존재해야 한다는 제약)(JDK1.8) |
| `@Native`              | native 메서드에서 참조되는 상수 앞에 붙입니다. (JDK1.8)      |
| **`@Target`**          | 애노테이션이 적용가능한 대상을 지정하는데 사용합니다.        |
| **`@Documented`**      | 애노테이션 정보가 javadoc으로 작성된 문서에 포함되도록 합니다. |
| **`@Inherited`**       | 애노테이션이 자손 클래스에 상속되도록 합니다.                |
| **`@Retention`**       | 애노테이션이 유지되는 범위를 지정하는데 사용합니다.          |
| **`@Repeatable`**      | 애노테이션을 반복해서 적용할 수 있게 합니다.(JDK1.8)         |

굵게 처리된 부분은 메타 애노테이션입니다.

## 애노테이션 정의하는 방법

### 기본형

```java
@interface 애너테이션명 {
  Type 요소이름();
  ...
}
```

새로운 애노테이션을 정의하는 방법은 위와 같습니다. 기본적으로 인터페이스 정의와 동일하고 `@` 기호를 붙이는 부분만 다릅니다.

### 애노테이션의 요소

애노테이션의 요소는 메서드 형태로 선언합니다.

요소는 기본형, String, 배열, Enum, 다른 애노테이션, 클래스들이 올 수 있습니다.

반환값이 있고, 매개변수가 없는 추상 메서드의 형태로 작성하면 됩니다. 매개변수는 받을 수 없습니다.

요소의 이름도 같이 적어주기 때문에 순서가 상관없습니다. (개인적으로 이 부분은 클래스 생성자에 들어오면 좋겠습니다.)

기본값을 `Type 속성명() default <기본값>` 형태로 지정할 수 있으며, 애노테이션을 사용할 때, 값이 지정되지 않은 경우 기본값이 사용됩니다.

애노테이션 요소가 하나뿐이고 이름이 `value`인 경우 애노테이션을 적용할 때, `value`를 생략하고 값만 적어도 됩니다.

배열타입 요소는 `{}` 를 사용해서 여러 개의 값을 전달해주면 됩니다.

애노테이션에는 예외를 선언할 수 없고, 제네릭 사용은 불가능합니다.

## @Retention

`@Retention`은 애노테이션이 유지되는 기간을 지정하는데 사용합니다. 애노테이션을 유지하는 기간을 설정하는 정책의 종류는 다음과 같습니다.

| 유지 정책 | 의미                                                         |
| --------- | ------------------------------------------------------------ |
| SOURCE    | 소스 파일에만 존재하고 클래스 파일에는 존재하지 않습니다.    |
| CLASS     | 클래스 파일에만 존재하고, 실행시에는 사용 불가 합니다. 기본 값입니다. |
| RUNTIME   | 클래스 파일에 존재하고, 실행 시점에도 존재합니다.            |

SOURCE 유지정책은 사실 컴파일러를 직접 작성하는 것이 아닌이상 우리가 사용할 일은 없습니다.

보통 `@Override`, `@SuppressWarnings` 같은 컴파일 시점에 컴파일러에 전달하는 정보의 경우에 사용하는 유지정책입니다.

우리가 흔히 사용하는 롬복의 정책이 바로 SOURCE 였습니다.

```java
@Target({ElementType.FIELD, ElementType.TYPE})
@Retention(RetentionPolicy.SOURCE)
public @interface Getter {
  ...
}
```

유지 정책을 RUNTIME으로 한다면 우리의 애플리케이션이 실행 중일 때, 리플렉션을 이용해서 클래스 파일에 저장된 애노테이션의 정보를 읽어서 처리해줄 수 있습니다.

우리가 자주 사용하는 JUnit의 Test 애노테이션의 정책이 바로 RUNTIME 입니다.

```java
@Target({ ElementType.ANNOTATION_TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@API(status = STABLE, since = "5.0")
@Testable
public @interface Test {
}
```

CLASS는 기본 값이지만 잘 사용되지 않는데, 그 이유는 클래스파일엔 존재하지만 런타임에는 애노테이션 정보를 읽어올 수 없기 때문입니다.

## @Target

`@Target`은 애노테이션이 적용가능한 대상을 지정하는데 사용합니다.

적용가능한 대상은 다음과 같습니다.

| 대상 타입       | 의미                            |
| --------------- | ------------------------------- |
| ANNOTATION_TYPE | 애너테이션                      |
| CONSTRUCTOR     | 생성자                          |
| FIELD           | 필드(멤버변수, enum 상수)       |
| LOCAL_VARIABLE  | 지역변수                        |
| METHOD          | 메서드                          |
| PACKAGE         | 패키지                          |
| PARAMETER       | 매개변수                        |
| TYPE            | 타입(클래스, 인터페이스, enum)  |
| TYPE_PRARMETER  | 타입 매개변수(JDK1.8)           |
| TYPE_USE        | 타입이 사용되는 모든 곳(JDK1.8) |

````java
@Target({FIELD, TYPE, TYPE_USE})
public @interface MyAnnotation {
  
}
````

위와 같은 식으로 정의해줄 수 있습니다.

## @Documented

애노테이션에 대한 정보가 javadoc 문서에 포함되도록 합니다.

### 예제로 배우는 @Documented

`DocumentTest.java`

```java
import java.lang.annotation.Documented;

@Documented
public @interface DocumentTest {
  String test();
}
```

`NonDocumentTest.java`

```java
public @interface NonDocumentTest {
  int age();
}
```

`AnnotationTest.java`

```java
public class AnnotationTest {

  public static void main(String[] args) {
    new AnnotationTest().hello();
    new AnnotationTest().annotatedHello();
  }

  /**
   * javadoc에 애노테이션 정보가 추가되지 않습니다.
   */
  @NonDocumentTest(age = 20)
  public void hello() {
    System.out.println("hello");
  }

  /**
   * javadoc에 애노테이션 정보가 추가됩니다.
   */
  @DocumentTest(test = "Hello!")
  public void annotatedHello() {
    System.out.println("Hello!!!");
  }
}
```

javadoc을 만들어봅시다. javadoc은 Java가 프로그래밍 역사에 큰 영향을 미친 부분중 하나라고 생각합니다.

`-d` 옵션은 directory를 지정하는 명령어입니다. [출처](https://docs.oracle.com/javase/9/javadoc/javadoc-command.htm)

> `-d` directory
> Specifies the destination directory where the javadoc command saves the generated HTML files. If you omit the `-d` option, then the files are saved to the current directory. The directory value can be absolute or relative to the current working directory. The destination directory is automatically created when the javadoc command runs.

```shell
$ javadoc -d <javadoc이 저장될 경로> <해당 패키지 경로>
$ javadoc -d doc ./*
Loading source file ./AnnotationTest.java...
Loading source file ./DocumentTest.java...
Loading source file ./NonDocumentTest.java...
Constructing Javadoc information...
Creating destination directory: "doc/"
Standard Doclet version 11.0.9
Building tree for all the packages and classes...
Generating doc/dev/idion/annotation/AnnotationTest.html...
Generating doc/dev/idion/annotation/DocumentTest.html...
Generating doc/dev/idion/annotation/NonDocumentTest.html...
Generating doc/dev/idion/annotation/package-summary.html...
Generating doc/dev/idion/annotation/package-tree.html...
Generating doc/constant-values.html...
Building index for all the packages and classes...
Generating doc/overview-tree.html...
Generating doc/index-all.html...
Building index for all classes...
Generating doc/allclasses-index.html...
Generating doc/allpackages-index.html...
Generating doc/deprecated-list.html...
Building index for all classes...
Generating doc/allclasses.html...
Generating doc/allclasses.html...
Generating doc/index.html...
Generating doc/help-doc.html...
$ cd doc
$ open index.html
```

![javadoc](https://i.imgur.com/kK2G3XG.png)

javadoc에 대해서는 아래 링크를 참고하시면 더 많은 정보를 보실 수 있습니다.

<https://www.baeldung.com/javadoc>

## 애노테이션 프로세서

### 애노테이션 프로세서란

애노테이션 프로세서에 대해서 설명하기 전에, 애노테이션 프로세서는 런타임에 리플렉션을 이용해서 애노테이션을 평가하는 것이 아닙니다. 애노테이션 프로세싱은 컴파일 타임에 일어납니다.

애노테이션 프로세서는 **컴파일 타임**에 애노테이션을 스캔하고 처리하기 위해 javac에서 확장해서 사용하는 도구라고 볼 수 있습니다. 그리고 우리는 특정 애노테이션을 위한 우리만의 애노테이션 프로세서를 등록할 수 있습니다.

보통 애노테이션 프로세서는 Java코드 혹은 컴파일된 Java Bytecode를 입력으로 받아서, 파일을(대개는 `.java` 파일) 출력으로 생성합니다. 이것이 의미하는 바는 우리가 자바 코드를 생성해낼 수 있다는 것이고, 이 생성된 코드가 `.java` 파일에 담기게 된다는 것입니다.  
간단히 말해서 우리가 메서드를 추가하기 위해서 이미 존재하는 자바 클래스파일을 조작할 필요가 없다는 것입니다. 이 생성된 java 파일은 다른 java 파일과는 동일하게 javac로 컴파일 됩니다.

대표적인 애노테이션 프로세서의 예로는 QueryDSL, JPA, Lombok, MapStruct 정도를 들 수 있겠습니다.

사실 정확한 애노테이션 프로세서의 동작원리는 기존 파일을 변경하는 것이 아니라, 새로운 파일을 생성한다는 점입니다. 이를 유의해야겠습니다.  
그리고 Lombok은 좋은 애노테이션 프로세싱의 사례이기도 하면서, 그 이상의 학습도 요구한다고 알려져있습니다.

### 애노테이션 프로세서 API

애노테이션 프로세서는 여러 라운드로 수행됩니다. 각각은 컴파일러가 소스 파일에서 애노테이션을 검색하고, 이러한 애노테이션에 적합한 애노테이션 프로세서를 선택하는 것 부터 시작합니다. 각각의 애노테이션 프로세서는 그것과 일치하는 애노테이션이 발견되었을 때, 호출됩니다.

각 라운드에서 만들어진 java 파일은 이 파일을 입력으로 해서 새로운 라운드가 시작됩니다. 이러한 프로세스 과정은 새로운 파일이 더 이상 생겨나지 않을 때 까지 실행됩니다.

예를 들어서, 제가 겪었던 문제의 경우에는 Lombok과 MapStruct를 같이 사용하는 경우 Lombok이 실행되기 전에 먼저 MapStruct Annotation Processor가 동작해서 Lombok으로 생성하려 했던 코드가 생성되기 전에 동작했기 때문에 결국 컴파일 타임에 코드 생성에 실패하는 경우가 있었습니다. 이러한 경우를 방지하기 위해서는 애노테이션 프로세서가 각 라운드별로 동작한다는 점을 숙지하는게 좋을 것 같습니다.

애노테이션 프로세서 API는 `javax.annotation.processing` 패키지에 있으며, 우리가 구현해야 할 주요 인터페이스는 `Processor` 인터페이스이며, 이를 부분적으로 구현한 추상 클래스인 `AbstractProcessor` 클래스입니다. 우리는 `AbstractProcessor`라는 추상클래스를 확장해서 자체적인 애노테이션 프로세서를 만들 수 있습니다.

### AbstractProcessor

우리가 작성할 Annotation Processor는 위에서 말했듯 `AbstractProcessor`를 상속해야합니다.

기본적으로 `AbstractProcessor`를 상속한다면, `process()`메서드는 반드시 구현해야 합니다.

예제를 통해서 알아봅시다.

```java
import java.util.Set;
import javax.annotation.processing.AbstractProcessor;
import javax.annotation.processing.ProcessingEnvironment;
import javax.annotation.processing.RoundEnvironment;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.TypeElement;

public class MyProcessor extends AbstractProcessor {

  @Override
  public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
    return false;
  }

  @Override
  public Set<String> getSupportedAnnotationTypes() {
    return super.getSupportedAnnotationTypes();
  }

  @Override
  public SourceVersion getSupportedSourceVersion() {
    return super.getSupportedSourceVersion();
  }

  @Override
  public synchronized void init(ProcessingEnvironment processingEnv) {
    super.init(processingEnv);
  }
}
```

- `process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv)`: 이 메서드는 애노테이션 프로세서의 `main()` 메서드라고 볼 수 있습니다. 여기에 애노테이션에 대한 스캔, 평가, 프로세싱과 자바 파일 생성에 대한 코드를 작성하시면 됩니다. `RoundEnvironment` 파라미터는 애노테이션 프로세서가 애노테이션 프로세싱의 라운드를 쿼리할 수 있도록 해줍니다.
- `getSupportedAnnotationTypes()`: 애노테이션 프로세서가 어떤 애노테이션들을 위해 등록되었는지 특정해줍니다. 여기서 반환되는 타입인 문자열 셋은 애노테이션 프로세서로 처리하려는 애노테이션 타입에 대한 완전한 이름을 포함해야 합니다. 다시 말해서, 우리는 여기에 우리가 작성한 애노테이션 프로세서가 어떤 애노테이션들을 위해서 만들어졌는지 애노테이션 이름의 집합을 작성해주면 된다는 뜻입니다.
- `getSupportedSourceVersion()`: 어떤 자바 버전을 사용할지 정의합니다. 대부분 `SourceVersionlateestSupported()`를 반환할 것입니다. 그러나, 특정 버전을 정의하는 경우엔 `SourceVersion.RELEASE_6`과 같은 식으로 정의해줄 수 있습니다. 이렇게 작성하면 자바 6버전에 고정됩니다. 하지만 전자의 경우를 사용하는 것을 권장합니다.
- `init(ProcessingEnvironment processingEnv)`: 모든 애노테이션 프로세서는 빈 생성자를 반드시 가져야 합니다. 그러나, annotation processing tool에 의해 실행되는 `init()`메서드를 사용하는 방법이 있습니다. 인자로 제공되는 `ProcessingEnvironment` 타입은 몇가지 유용한 유틸리티 클래스인 `Elements`, `Types`, `Filer`를 제공합니다.

Java7 이후로는 애노테이션을 활용한 다음의 표현을 사용하는 것을 추천합니다.

```java
import java.util.Set;
import javax.annotation.processing.AbstractProcessor;
import javax.annotation.processing.ProcessingEnvironment;
import javax.annotation.processing.RoundEnvironment;
import javax.annotation.processing.SupportedAnnotationTypes;
import javax.annotation.processing.SupportedSourceVersion;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.TypeElement;

@SupportedSourceVersion(SourceVersion.RELEASE_11)
@SupportedAnnotationTypes({"My"})
public class MyProcessor extends AbstractProcessor {

  @Override
  public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
    return false;
  }

  @Override
  public synchronized void init(ProcessingEnvironment processingEnv) {
    super.init(processingEnv);
  }
}
```

애노테이션 프로세서는 별개의 JVM에서 동작합니다. 따라서 다른 Java 애플리케이션 또한 사용할 수 있다는 의미가 됩니다.

주의할 점은 아무리 작은 프로세서를 만든다 한들 다른 자바 애플리케이션과 동일하게 효율적인 알고리즘과 디자인 패턴을 고민하는 모습을 보여야 합니다.

### 프로세서 등록하기

'작성은 했는데, 이걸 어떻게 javac에 제공해줘야하지?' 라는 고민은 저만 하는 것이 아닐겁니다. 프로세서를 제공하는 방법은 jar 파일을 제공하는 것입니다. 다른 jar 파일과 마찬가지로 컴파일된 애노테이션 프로세서를 jar파일에 패키징합니다. 또한 jar파일에 `META-INF/services`에 위치한 `javax.annotation.processing.Processor`라고 불리는 특별한 파일 또한 패키징해야합니다.

이렇게 작성된 jar파일은 아래의 구조를 가지게됩니다.

```text
MyProcessor.jar
  - dev
    - idion
      - annotationprocessor
        - MyProcessor.class

  - META-INF
    - services
      - javax.annotation.processing.Processor
```

javax.annotation.processing.Processor 파일의 내용은 완전한 클래스 이름의 목록이며, 새 줄로 구분합니다.

예를 들어서 다음과 같은 내용을 가질 수 있습니다.

```text
dev.idion.annotationprocessor.MyProcessor
com.foo.BarProcessor
net.blabla.SpecialProcessor
```

빌드 경로에 MyProcessor.jar를 두면 javac가 자동으로 javax.annotation.processing.Processor 를 감지하고, 이를 읽은 다음 MyProcessor 애노테이션 프로세서를 등록합니다.

---

그 밖의 애노테이션을 작성하고, 애노테이션 프로세서를 이용해 코드 생성기능까지 구현하는 예제가 애노테이션 프로세싱 101과 밸덩에 포함되어 있으니, 관심이 있다면 해보면 좋을 것 같습니다.

- 애노테이션 프로세싱 101은 기본적인 기능으로만 구현합니다.
- 밸덩은 메이븐을 활용해서 애노테이션 프로세서를 메이븐으로 등록하는 방법 등을 비롯한 다른 방법을 알려줍니다.

### 롬복

사실 롬복은 애노테이션 프로세서가 제공해주는 공개된 API를 벗어나서 자바 코드 그 자체를 조작한다는 문제점을 갖고 있습니다.

또한, 롬복에 대해서 다루는 것은 애노테이션 프로세서를 사용한다는 정도만 알아두면 될 것 같아서 이곳에 정리하지는 않겠습니다.

## 참고자료

- [자바의 정석 3/e](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788994492032&orderClick=LEa&Kc=)
- [밸덩 애노테이션 프로세싱과 빌더 만들기](https://www.baeldung.com/java-annotation-processing-builder)
- [더 자바, 코드를 조작하는 다양한 방법](https://www.inflearn.com/course/the-java-code-manipulation?inst=c160e128)
- [애노테이션 프로세싱 101](http://hannesdorfmann.com/annotation-processing/annotationprocessing101)

## 라이브 스터디

### 추천하는 책

- 아웃라이어(1만시간의 법칙)
- 부의 추월차선

### Annotation의 발음

애너테이션으로 들린다

참고로 null은 널

### Annotation은 무엇인가?

주석 같은 것이다. 애너테이션은 기능을 가지고 있지 않다.

따라서 정적이고, 바뀌지 않는 요소만 애너테이션에 사용할 수 있다.

문서화 할 때도 사용해야 하는데, 어떤 값이 들어갈 것인지도 알려주어야 한다.

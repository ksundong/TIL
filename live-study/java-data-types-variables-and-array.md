# 자바 데이터 타입, 변수 그리고 배열

## 학습할 것

- [프리미티브 타입 종류와 값의 범위 그리고 기본 값](#프리미티브-타입-종류와-값의-범위-그리고-기본-값)
- [프리미티브 타입과 레퍼런스 타입](#프리미티브-타입과-레퍼런스-타입)
- [리터럴](#리터럴)
- [변수 선언 및 초기화하는 방법](#변수-선언-및-초기화하는-방법)
- [변수의 스코프와 라이프타임](#변수의-스코프와-라이프타임)
- [타입 변환, 캐스팅 그리고 타입 프로모션](#타입-변환,-캐스팅-그리고-타입-프로모션)
- [1차 및 2차 배열 선언하기](#1차-및-2차-배열-선언하기)
- [타입 추론, var](#타입-추론,-var)

## 프리미티브 타입 종류와 값의 범위 그리고 기본 값

primitive type은 자바의 기본 타입들입니다. 총 8개 입니다.

자바에서는 필드 선언시 초기화를 하지 않으면, 기본 값으로 초기화가 됩니다. 프리미티브 타입은 유의미한 값을 가지며, 레퍼런스 타입은 null로 초기화가 됩니다.

- **boolean**: **참(true)** 혹은 **거짓(false)**를 나타내는 boolean 타입입니다. 1bit로도 표현가능하지만, 일반적인 JVM의 구현은 1byte를 사용하는 것으로 알려져있습니다. (크기는 JVM의 구현에 의존적입니다.) 기본 값은 `false`입니다.
- **char**: **유니코드 문자**를 나타내는 정수형 타입입니다. 특이점으로는 unsigned 2byte기 때문에 표현할 수 있는 값의 범위는 '\u0000'(0)에서 '\uffff'(65535) 입니다. 기본 값은 `'\u0000'`입니다.
- **byte**: 이름과 같이 1byte를 차지하는 정수형 타입입니다. -128 ~ 127의 범위를 가집니다. 기본 값은 `0`입니다.
- **short**: 2byte를 차지하는 정수형 타입입니다. -32,768 ~ 32,767의 범위를 가집니다. 기본 값은 `0`입니다.
- **int**: 4byte를 차지하는 정수형의 타입입니다. -2<sup>31</sup> ~ 2<sup>31</sup>-1 의 범위를 가집니다. 기본 값은 `0`입니다. 또, 리터럴로 정수형 값을 할당할 때, 기본적으로 사용되는 타입입니다.
- **long**: 8byte를 차지하는 정수형 타입입니다. -2<sup>63</sup> ~ 2<sup>63</sup>-1의 범위를 가집니다. 정수형 리터럴에 `L`이나 `l`을 뒤에 붙이면 long 타입으로 생성되며, 일반적으로 대문자 L이 혼동을 방지할 수 있어 (소문자 l의 경우엔 유사한 문자가 많아서) **대문자를 사용하는 것이 일반적**입니다. 기본 값은 `0`입니다.
- **float**: 4byte를 차지하는 IEEE 754 단정밀도 부동소수점 타입입니다. 부동소수점 타입은 표현가능한 값의 범위는 굉장히 넓으나, 정확히 표현하지 못하는 한계가 있기 때문에 보통은 **유효 숫자 범위**를 더 중요하게 생각합니다. float 타입은 **6~7 자리의 정밀도**를 가집니다. 부동소수점 리터럴에 `f`혹은 `F`를 붙여서 표현합니다. 기본값은 `0.0f`입니다.
- **double**: 8byte를 차지하는 IEEE 754 배정밀도 부동소수점 타입입니다. double 타입은 **15자리의 정밀도**를 가집니다. 부동소수점 리터럴은 기본적으로 double 형을 사용하나 `d`혹은 `D`를 사용해서 표현할 수도 있습니다. 기본값은 `0.0d`입니다.

부동소수점은 [IEEE 754 Wikipedia](https://ko.wikipedia.org/wiki/IEEE_754) 여기에 자세히 설명되어 있으며, 부동소수점이 어떻게 구성되어 있는지 설명하는 것은 별개의 범위인 것 같아 여기까지 하겠습니다.

부동소수점은 정밀도가 있다고는 하지만 값의 정확한 연산을 보장하지 못하므로 화폐 계산과 같은 연산을 할 때에는 `BigDecimal` 클래스를 사용하는 것이 좋습니다.

### 표

| type    | size in bytes                   | range                                | default value |
| ------- | ------------------------------- | ------------------------------------ | ------------- |
| boolean | 정의되지 않음(일반적으로 1byte) | true or false                        | false         |
| char    | unsigned 2byte                  | '\u0000'(0) to '\uffff'(65535)       | '\u0000'      |
| byte    | 1byte                           | -128 to 127                          | 0             |
| short   | 2byte                           | -32,768 to 32,767                    | 0             |
| int     | 4byte                           | -2<sup>31</sup> to -2<sup>31</sup>-1 | 0             |
| long    | 8byte                           | -2<sup>63</sup> to -2<sup>63</sup>-1 | 0             |
| float   | 4byte                           | (6~7자리 정밀도)                     | 0.0f          |
| double  | 8byte                           | (15자리 정밀도)                      | 0.0d          |

### 참고

[오라클 자바 튜토리얼, 데이터 타입](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)

[cs-fundamentals](https://cs-fundamentals.com/java-programming/java-primitive-data-types)

[Oracle JLS 4. Types, Values, and Variables](https://docs.oracle.com/javase/specs/jls/se11/html/jls-4.html)

---

[학습할 것으로](#학습할-것)

## 프리미티브 타입과 레퍼런스 타입

자바의 타입은 크게 두 종류로 나눌 수 있습니다. primitive type과 reference type입니다.

primitive type은 위에서 설명했고, reference type에 대해서 설명하겠습니다.

reference type은 primitive type을 제외한 모든 타입들이 해당됩니다. 이런 reference type은 변수 선언시 변수에 값이 저장되는 것이 아니라 객체에 대한 힙 영역의 참조를 저장하게 됩니다. 자바의 참조는 포인터가 아니기 때문에, 개발자는 직접적으로 메모리에 접근해서 조작할 수 없습니다.

이 참조를 저장한다는 말은, 같은 참조를 가리키고 있다면 한 쪽에서 객체의 상태를 변경하는 경우 다른 쪽에서도 영향을 받을 수 있다는 것입니다.

아래의 코드는 그러한 사항을 간단히 확인해보는 코드입니다.

```java
void 상태변경() {
    // 새로운 Hello type 인스턴스를 생성해서 a라는 변수에 참조를 저장하고,
    // b라는 변수에 a가 가지고 있는 참조를 할당했습니다.
    Hello a = new Hello();
    Hello b = a;

    // a, b 둘 다 새로 만든 Hello 인스턴스에 대한 참조를 가지고 있는 상태입니다.
    // 이 때, b를 통해서 상태를 조작할 경우 a에서도 영향을 받게됩니다.
    // property는 초기화시 0으로 초기화 되었다고 가정합니다.
    a.showProperty(); // 0
    b.showProperty(); // 0
    b.changeProperty(999);
    b.showProperty(); // 999
    a.showProperty(); // 999로 변경된 상태를 표시합니다.
}
```

또, reference type에는 특별한 타입인 `null`을 할당할 수 있지만, primitive type에는 할당할 수 없습니다. `null`의 의미가 null 참조를 의미하기 때문입니다. 공부를 하면서 든 생각인데, null 참조라고 하면서 Exception의 이름은 `NullPointerException` 이라는게 좀 신기하게 느껴졌습니다. 큰 의미를 두고 생각하진 않았습니다.

특이한 것은 reference type의 구분을 `Class`, `Interface`, `Type Variable`(흔히 `Generic`이라고 말하는), `Array` 로 구분한다는 점입니다.

### 참고

[Geeks for Geeks - C/C++의 포인터와 자바의 참조의 차이](https://www.geeksforgeeks.org/is-there-any-concept-of-pointers-in-java/)

[Oracle JLS 4. Types, Values, and Variables](https://docs.oracle.com/javase/specs/jls/se11/html/jls-4.html)

---

[학습할 것으로](#학습할-것)

## 리터럴

리터럴은 고정된 값을 나타내는 소스코드상에서의 표현입니다. 리터럴은 별도의 연산이 필요없이 표현됩니다.

### 정수 리터럴

정수 리터럴은 정수형 숫자, 정수형 숫자가 L 또는 l로 끝나는 형태로 표현됩니다. `byte`, `short`, `int`, `long` 타입은 `int` 리터럴로 생성될 수 있습니다. `int` 범위를 벗어나는 값은 `long` 리터럴로 생성됩니다.

```java
void 정수_리터럴() {
    byte byteVal = 1;
    short shortVal = 1;
    int intVal = 1;
    long longVal = 1;
}
```

정수 리터럴은 10진법을 기본으로, 16진법, 2진법으로도 표현될 수 있습니다.

- 10진법: 0부터 9로 구성되는 일반적으로 사용하는 숫자체계입니다.
- 16진법: 0부터 9, A부터 F로 구성된 16진수입니다. 16진법을 많이 사용하는 이유는 2<sup>4</sup>를 한 자리에서 표현이 가능하기 때문입니다.
- 2진법: Java 7 이상부터 작성할 수 있는 리터럴로 0과 1로 구성되는 숫자체계입니다.

```java
void 진법별_26_표현() {
    int decVal = 26;
    int hexVal = 0x1a;
    int binVal = 0b11010;
}
```

### 부동 소수점 리터럴

부동 소수점 리터럴은 소숫점이 포함된 숫자가 F또는 f로 끝나는 형태로 표현됩니다. 이 경우엔 `float` 형 리터럴이고, 나머지 경우와 D또는 d로 끝나는 경우엔 `double` 형 리터럴입니다.

부동 소수점은 E또는 e를 사용해서 지수 표현식으로도 작성할 수 있습니다.

```java
void 지수_표기법() {
    double d1 = 123.4;
    // 지수 표기법으로는 다음과 같습니다.
    double d2 = 1.234e2;
}
```

### 숫자 리터럴에서의 밑줄 문자사용

자바7부터 추가된 기능으로, 숫자 리터럴을 읽기 쉽게해줍니다. 자릿수가 긴 리터럴을 표현할 때 유용합니다.

숫자 사이에만 밑줄을 넣을 수 있고, 다음 위치에는 넣을 수 없습니다. 연속으로 입력도 가능합니다.

- 숫자의 시작 또는 끝
- 부동 소수점 리터럴에서 소수점에 붙은경우
- 뒤에 붙는 F, L, D등의 문자의 앞 뒤에 붙은경우
- 문자열에서 문자가 올 것으로 예상되는 위치

```java
void 밑줄_사용한_숫자_표현() {
    long phoneNumber = 010_1234_5678;
    long bigNumber = 999_999_999_999_999_999L;
    long manyUnderScore = 9________9;
}
```

### 문자 및 문자열 리터럴

`char`와 `String` 은 유니코드로 표현됩니다. 직접 해당 문자를 코드에 입력할 수도 있고, 유니코드 이스케이프('\u0000')를 사용할 수 있습니다.

`char` 형 리터럴은 작은 따옴표(')를 사용하고, `String` 형 리터럴은 큰 따옴표(")를 사용합니다.

그리고 Java는 몇 가지 특별한 문자를 가지고 있고, 이를 이스케이프 문자라고 합니다. `\`를 이스케이프 문자라고 하며 백슬래시를 입력해서 사용할 수 있습니다.  
`\b`(백 스페이스), `\t`(탭), `\n`(라인 피드), `\f`(폼 피드), `\r`(캐리지 리턴), `\"`, `\'`, `\\`

### 불린 리터럴

불린 리터럴은 논리값을 나타내는 표현입니다. `true`, `false`로 나타낼 수 있습니다.

### 그 외 리터럴

`null` 도 리터럴이며 특수 리터럴로 분류됩니다.

`Class` 리터럴이라는 특수한 리터럴도 존재합니다. 이는 `<type의 이름>.class`의 형태로 표현합니다. 이는 `Class` 타입 자체를 나타내는 객체의 참조를 반환하며, reflection을 사용할 때, 많이 이용합니다.

### 참조

[오라클 자바 튜토리얼, 데이터 타입](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)

---

[학습할 것으로](#학습할-것)

## 변수 선언 및 초기화하는 방법

자바의 변수는 다음과 같은 종류로 구분할 수 있습니다.

- 인스턴스 변수: 클래스 선언시 `static` 키워드 없이 선언된 필드입니다. 이 필드는 인스턴스 별로 다른 값을 가질 수 있기 떄문에, 인스턴스 변수라고 불립니다.
- 클래스 변수: 클래스 선언시 `static` 키워드와 함께 선언된 필드입니다. 이 필드는 모든 인스턴스들이 공유하는 값입니다. 클래스 명으로 접근이 가능하고, 클래스 하나에 한 값이기 때문에 클래스 변수라고 불립니다.
- 로컬 변수: 메서드 선언 사이에 등장하는 변수로 다른 클래스에서 접근할 수 없는 변수입니다. 메서드 영역에서만 임시로 사용되는 변수입니다.
- 매개 변수: 매개 변수는 메서드의 인자로 전달되는 변수를 의미합니다.

인스턴스 변수와 클래스 변수는 멤버 변수라고 통칭하기도 합니다. 멤버 변수는 꼭 초기화를 해주지 않더라도 기본값으로 초기화되지만, 로컬 변수는 반드시 초기화를 해주어야 합니다.

```java
class Variables {
    int instanceVar; // 0으로 초기화되는 인스턴스 변수
    static int classVar; // 0으로 초기화되는 클래스 변수
    int initInstanceVar = 10; // 명시적으로 초기화
    static int initClassVar = 10; // 명시적으로 초기화

    void method(int num) { // 매개변수는 초기화 할 수 없고, 전달받는 값을 사용만 할 수 있음
      int a; // 선언은 가능
      // int b = a; 자동으로 초기화 되지 않으므로 동작하지 않음
      a = 10; // 선언을 미리 해줬다면 이렇게 초기화 가능
      int b = a; // 선언과 동시에 초기화
    }
}
```

### 참조

[오라클 자바 튜토리얼, 변수](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)

---

[학습할 것으로](#학습할-것)

## 변수의 스코프와 라이프타임

스코프란? 변수가 유효한 범위를 의미합니다. 의도하신 부분이 코드에서의 스코프를 의미하신 것 같습니다. (자바는 추가로 Access modifier에 의한 스코프도 추가로 정의됩니다.)

| Variable Type     | Scope                                                  | LIfetime                                      |
| ----------------- | ------------------------------------------------------ | --------------------------------------------- |
| Instance Variable | (`static` 블록과 `static` 메서드를 제외한) 클래스 전체 | 객체가 생성되고 객체가 메모리에 살아있는 동안 |
| Class Variable    | 클래스 전체                                            | 클래스가 초기화되고 프로그램이 끝날 때 까지   |
| Local Variable    | 변수가 선언된 블록내부                                 | 변수 선언 이후 부터 블록을 벗어날 때까지      |

### 참고

<https://www.learningjournal.guru/article/programming-in-java/scope-and-lifetime-of-a-variable/>

---

[학습할 것으로](#학습할-것)

## 타입 변환, 캐스팅 그리고 타입 프로모션

타입 변환이라고 하는 것은 어떤 값이나 변수의 타입을 다른 타입으로 변경하는 것을 말합니다.

이 때 타입은 두 가지 방향으로 변환될 수 있습니다. **확장(자동 형변환)**, **축소(명시적 형변환)**

### 확장, 자동 형변환

확장은 두 데이터 타입이 자동으로 변환되는 경우입니다. 단, 조건이 있습니다.

- 두 데이터 타입은 호환이 되어야 합니다.
- 더 작은 데이터 타입의 값을 더 큰 범위의 타입에 할당할 때만 동작합니다.(바이트 크기가 아닌 표현 가능한 값의 범위)

자바에서는 숫자 타입은 `byte -> short -> int -> long -> float -> double` 순으로 자동 형변환이 됩니다.

```java
void 자동_형변환() {
    int i = 100;
    long l = i;
    float f = l;
}
```

### 축소, 명시적 형변환(Casting)

축소는 더 작은 범위의 타입에 더 큰 범위의 타입의 값을 할당하기 위해선 반드시 형변환을 명시해줘야합니다.  
또, 호환되지 않는 데이터 타입에도 사용할 수 있습니다.

`(target-type) <value>or<variable>` 형태로 형변환을 강제할 수 있습니다.

```java
void 명시적_형변환() {
    // 동작하지 않는 코드
    char ch = 'c';
    int num = 88;
    ch = num;

    // 동작하기 위해선 아래와 같이 수행해야 합니다.
    ch = (char) num;
}
```

### 타입 프로모션

식을 평가할 때, 중간에 피연산자의 범위를 초과할 수 있기 때문에 자동으로 값이 승격되는데, 이를 타입 프로모션이라고 합니다.

1. Java에서는 `byte`, `short`, `char`를 식 평가시 자동으로 `int`로 프로모션 됩니다.
2. 피 연산자가 `long`, `float`, `double` 인 경우 전체 표현식이 각각 `long`, `float`, `double`로 프로모션됩니다.

```java
void 타입_프로모션() {
    byte b = 42;
    char c = 'a';
    short s = 1024;
    int i = 50000;
    float f = 5.67f;
    double d = .1234;

    // 표현식(명시적 형변환 없이 자동으로 형변환 되며, 값은 피연산자의 타입으로 프로모션 됩니다.)
    double result = (f * b) + (i / c) - (d * s);
}
```

### 식에서의 명시적인 형변환

반대로, 결과를 더 작은 데이터 타입에 저장하려면 명시적으로 형변환을 해줘야 합니다.

```java
void 식_명시적_형변환() {
    byte b = 50;

    // byte는 식 평가시 자동으로 int로 프로모션 됩니다. 따라서 캐스팅 해줘야 합니다.
    b = (byte) (b * 2);
}
```

### 오토박싱/언박싱

Java에서는 `primitive` 타입에 대한 `Wrapper` 클래스가 있습니다. Java 1.5 이후로는 이러한 값 끼리 명시적인 형변환을 해줄 필요가 없는데, Java 컴파일러가 이를 대신 해주기 때문입니다.

오토박싱과 언박싱을 사용하면 개발자가 더 깔끔한 코드를 작성할 수 있게 되어 가독성이 높아집니다.

#### 오토박싱

오토박싱은 `primitive` 타입에서 `Wrapper` 클래스로 자동 변환되는 것으로 `int`를 `Integer`로 변환하는 식으로 동작합니다.

`Integer integer = 1;`

이 코드는 컴파일러가 `Integer integer = Integer.valueOf(1);` 로 변환합니다.

#### 언박싱

언박싱은 `Wrapper` 타입을 `primitive` 타입으로 변환해줍니다. Java에서는 `Wrapper` 클래스의 객체가 다음과 같은 경우일 때, 언박싱을 해줍니다.

- 메소드에서 `primitive` 타입으로 매개변수를 받을 때, 매개변수로 전달되는 경우
- 해당 `primitive` 타입의 변수에 할당되는 경우

```java
void 언박싱_예제() {
    Integer integer = 10;

    // 매개변수로 전달되는 경우
    예제_메서드(integer);

    // primitive 타입의 변수에 할당되는 경우
    int i = integer;
}

void 예제_메서드(int num) {
    System.out.println(num);
}
```

### 객체의 형변환

`primitive` 타입의 형변환은 값 자체의 변환을 의미합니다. 하지만, 객체의 형변환은 참조 변수에서 객체를 바라보는 관점의 변환을 의미합니다. 즉, 힙에 있는 객체 자체는 변경되지 않습니다.

#### 업 캐스팅

서브 클래스에서 슈퍼 클래스로 캐스팅하는 것을 업 캐스팅이라고 하고, 컴파일러에 의해 수행됩니다.

업 캐스팅은 상속과 밀접한 관련이 있습니다. 예제를 통해서 알아봅시다.

```java
public class Animal {
    public void eat() {
        // ...
    }
}
```

`Animal` 클래스를 생성했습니다.

```java
public class Cat extends Animal {
    @Override
    public void eat() {
        // ...
    }

    public void meow() {
        // ...
    }
}
```

`Animal` 클래스를 확장하는 `Cat` 클래스를 정의했습니다.

```java
// 먼저 Cat 클래스는 다음과 같이 생성할 수 있습니다.
Cat cat = new Cat();
// 또한 새로생성된 Cat 객체는 Animal 타입의 참조 변수에도 할당할 수 있습니다.
Animal animal = cat;
// 이는 다음과 같이 컴파일러에 의해 변경됩니다.
Animal animal = (Animal) cat;
```

참조변수는 선언된 타입의 하위 타입을 참조할 수 있습니다. 대신, 그 타입에서 사용할 수 있는 메서드는 제한될 수 있습니다. 하지만 인스턴스 자체는 변경되지 않습니다.

`cat` 에서는 `meow()` 를 호출할 수 있지만, `animal` 에서는 `meow()`를 호출할 수 없습니다.

업 캐스팅 덕분에 우리는 다형성을 활용할 수 있습니다.

이 다형성은 객체지향을 다룰 때 다시 한 번 짚고 넘어가는게 좋을 것 같습니다.

#### 다운 캐스팅

가끔 우리는 위와 같이 상위 타입으로 참조하는 하위 타입 객체를 활용하는 경우가 있습니다. 이 때, 하위 타입에서만 제공하는 메서드를 사용하고 싶은 경우 우리는 다운 캐스팅을 활용할 수 있습니다.

다운캐스팅은 슈퍼 클래스에서 서브 클래스로 명시적 캐스팅을 하는 것입니다.

위의 코드에 이어서 다음과 같은 코드를 작성할 수 있습니다.

```java
// error: incompatible types: Animal cannot be converted to String
((Cat) animal).meow();
```

위의 코드는 명시적으로 `Cat` 클래스로 캐스팅했으며, 정상적으로 동작합니다. 하지만, `Animal` 클래스를 상속하는 다른 클래스의 인스턴스를 사용한다거나, `Animal` 클래스의 인스턴스를 사용한 경우 `ClassCastException`이 발생하게 됩니다.

컴파일 타임에는 잡아주지 못하지만, IntelliJ는 이 부분에 밑줄을 표시에 알려줍니다. 즉, 문제없이 컴파일 되지만, 런타임에 예외가 발생하게 됩니다.

단, 관계가 없는 타입으로 캐스팅하는 경우엔 컴파일 에러가 발생합니다. 즉, 서로 연관이 있어야만 컴파일이 되고, 연관이 있더라도 타입이 일치하지 않는다면, 런타임에 `ClassCastException`이 발생하게됩니다.

```java
String s = (String) animal;
```

다시 돌아와서, 우리는 이런 런타임 예외를 방지하기 위해 `instanceof` 연산자를 사용할 수 있습니다.

```java
if (animal instanceof Cat) {
    ((Cat) animal).meow();
}
```

이렇게 해야만, 안전하게 다운 캐스팅을 수행할 수 있습니다.

##### 요약

- 하위 클래스의 특정 멤버(변수, 메서드)에 액세스하려면 다운 캐스팅을 해야합니다.
- 다운 캐스팅은 캐스트 연산자'`(type)`'를 사용해서 수행됩니다.
- 실제 객체가 다운 캐스트 한 타입과 일치하지 않으면 런타임에 `ClassCastException`이 발생합니다.
- 따라서 객체를 안전하게 다운 캐스팅하려면 `instanceof` 연산자를 사용해야 합니다.

### 참고

<https://www.geeksforgeeks.org/type-conversion-java-examples/>

[오라클 자바 튜토리얼-오토박싱/언박싱](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)

[밸덩(자바 객체 타입 캐스팅)](https://www.baeldung.com/java-type-casting)

---

[학습할 것으로](#학습할-것)

## 1차 및 2차 배열 선언하기

차원이 맞는 표현인 것 같습니다. _multidimensional_ array라는 표현이 튜토리얼에서 나와서 미루어 짐작했습니다.

### 1차원 배열 선언

```java
int[] intArr1;
int []intArr2;
int intArr3[];
```

셋 다 가능하지만, 가장 위의 스타일이 선호됩니다. 나머지 둘은 권장되지 않습니다.

### 1차원 배열 초기화

단순 선언만 하면 사용할 수 없으니 초기화를 해줘야 합니다.

```java
int[] intArr1 = new int[10];
```

위와 같이 선언하면, 해당 크기만큼 배열의 요소가 초기화되어 생성됩니다.

배열의 각 요소에는 인덱스로 접근할 수 있습니다.

```java
intArr1[0] = 100;
System.out.println(intArr1[0]); // 100
```

다음과 같이 초기화 할 수도 있습니다.

```java
int[] anArray = {
    100, 200, 300,
    400, 500, 600,
    700, 800, 900, 1000
};

int[] anArray = new int[] {100, 200, 300};
```

배열의 길이는 중괄호 사이에 선언된 요소의 수로 결정됩니다.

사실 여기서 그냥 넘어가기 쉬운것이 이 `new`라는 키워드인데 위에서 설명했듯 Java는 배열도 객체입니다. 따라서 힙 영역에 배열이 생성되게 됩니다.

### 2차원 배열 선언

```java
int[][] ints;
```

위와 같이 선언할 수 있습니다. 또한, 자바에서의 다차원 배열은 **요소 자체가 배열**인 배열입니다.

각 요소 배열간 길이는 다를 수 있습니다.

### 2차원 배열 초기화

```java
String[][] names = {
    { "Mr.", "Mrs.", "Ms." },
    { "Smith", "Jones" }
};
// 각 배열 요소의 길이는 초기화시에 지정을 해주지 않아도 됩니다. 단, null로 초기화됩니다.
String[][] names2 = new String[10][];

// 혹은 지정해줄 수도 있습니다. 이 경우 같은 크기를 가지게 됩니다. 이 경우 배열이 초기화 됩니다.
String[][] names3 = new String[10][10];
```

이차원 배열도 마찬가지로 요소의 인덱스로 접근할 수 있습니다.

```java
// Mr.Smith
System.out.println(names[0][0] + names[1][0]);
// Ms.Jones
System.out.println(naems[0][2] + names[1][1]);
```

### 추가

배열의 크기는 `length` 속성으로 알 수 있습니다. 그리고 배열은 한 번 초기화 하고 나면 변경할 수 없습니다.

### 참고

[오라클 자바 튜토리얼-배열](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)

---

[학습할 것으로](#학습할-것)

## 타입 추론, var

`var` 키워드는 자바 10에서 추가된 기능으로, 로컬 변수의 타입 추론이라고 합니다.

이 키워드의 사용은 반복되는 타입 선언을 줄여주고, 코드의 가독성을 높여줍니다.

먼저 자바의 타입 추론의 역사에 대해서 알아보겠습니다.

### 타입 추론의 역사

타입 추론은 컴파일러가 타입을 알아낼 수 있기 때문에 타입을 입력할 필요가 없다는 아이디어에서 시작됩니다.

1. Java5부터 제네릭 메서드가 있습니다. 타입 매개 변수를 추론하는 방법입니다.
   아래의 코드는

   ```java
   List<String> cs = Collections.<String>emptyList();
   ```

   다음과 같이 변경할 수 있습니다.

   ```java
   List<String> cs = Collections.emptyList();
   ```

2. Java7부터 다이아몬드 오퍼레이터(`<>`)가 추가되었습니다. 표현식에서 제네릭의 타입 매개 변수를 생략할 수 있습니다.
   아래의 코드는

   ```java
   Map<String, List<String>> myMap = new HashMap<String,List<String>>();
   ```

   다음과 같이 변경할 수 있습니다.

   ```java
   Map<String, List<String>> myMap = new HashMap<>();
   ```

   위의 아이디어는 컴파일러가 주변 코드에 따라 타입을 유추할 수 있다는 것입니다.

3. Java8에는 람다 표현식이 추가되었고, 이 역시 마찬가지로 타입을 생략알 수 있습니다.
   아래의 코드는

   ```java
   Predicate<String> nameValidation = (String x) -> x.length() > 0;
   ```

   다음과 같이 변경할 수 있습니다.

   ```java
   Predicate<String> nameValidation = x -> x.length() > 0;
   ```

### 로컬 변수 타입 추론

`Scala`나 `C#` 같은 언어는 로컬 변수 선언시 타입을 `var`라는 키워드로 바꿀 수 있으며, 컴파일러가 적절하게 타입을 채워줍니다.

예를 들어 Java10 이전에는 아래와 같은 코드가

```java
Path path = Paths.get("src/web.log");
try (Stream<String> lines = Files.lines(path)) {
    long warningCount
            = lines
                .filter(line -> line.contains("WARNING"))
                .count();
    System.out.println("Found " + warningCount + " warnings in the log file");
} catch (IOException e) {
    e.printStackTrace();
}
```

Java10 부터는 다음과 같이 바뀔 수 있습니다.

```java
var path = Paths.get("src/web.log");
try (var lines = Files.lines(path)) {
    var warningCount
            = lines
                .filter(line -> line.contains("WARNING"))
                .count();
    System.out.println("Found " + warningCount + " warnings in the log file");
} catch (IOException e) {
    e.printStackTrace();
}
```

이는 마법처럼 일어나는 것이 아니라 컴파일러가 추론해 낼 수 있기 때문입니다.

- `Paths.get()`은 `Path` 타입을 반환합니다. 따라서 로컬 변수 `path`는 `Path` 타입임을 추론할 수 있습니다.
- `Files.lines()`는 `Stream<String>` 타입을 반환합니다. 따라서 변수 `lines`는 `Stream<String>` 타입임을 추론할 수 있습니다.
- `Stream`의 `count()`는 `long` 타입을 반환합니다. 따라서 로컬 변수 `warningCount`는 `long`타입임을 추론할 수 있습니다.

이렇게 이미 타입이 정의 되어있기 때문에 `var`는 다른 타입의 재할당을 허용하지 않습니다.

### 타입 추론의 맹점

하지만, 타입 추론이 만능은 아닙니다. 특히 Java에서는 객체지향 패러다임을 사용하기 때문에 문제가 될 수 있습니다.

예를 들어 `Animal` 클래스가 있고, 이 클래스의 서브 클래스인 `Cat`, `Dog`이 있다고 가정합시다.

```java
var v = new Cat();
```

이 타입은 어떻게 추론될까요? `Animal`이 될까요? `Cat`이 될까요? 이 경우 컴파일러는 초기화한 클래스의 타입을 사용하게 됩니다.

따라서 다음의 코드는 컴파일되지 않습니다.

```java
v = new Dog();
```

즉, 다형성을 활용하는 코드는 `var` 타입 추론과 어울리지 않습니다.

### `var`를 사용할 수 없는 위치

- 필드 혹은 메서드 시그니처에서 사용이 불가능 합니다. 지역 변수에만 사용할 수 있습니다.

  ```java
  public long process(var list) { }
  ```

  위의 코드는 사용할 수 없습니다.

- 명시적인 초기화 없이 `var`만 단독으로 선언할 수 없습니다.

  ```java
  var x;

  error: cannot infer type for local variable x
  var x;
      ^
  (cannot use 'var' on variable without initializer)
  1 error
  ```

- `var` 변수는 `null`로 초기화 할 수 없습니다.

  ```java
  var x = null;

  error: cannot infer type for local variable x
  var x = null;
      ^
  (variable initializer is 'null')
  ```

- 람다식과 `var`는 명시적으로 타겟이 되는 타입을 알아야 하기 때문에 같이 사용할 수 없습니다.

  ```java
  var x = () -> {};

  error: cannot infer type for local variable x
  var x = () -> {};
      ^
  (lambda expression needs an explicit target-type)
  ```

### 주의해야할 `var` 선언

```java
var list = new ArrayList<>();
```

이 코드는 컴파일 되지만, 실제 `list`의 타입은 `ArrayList<Object>`로 컴파일되며, 제네릭의 이점을 얻지 못하기 때문에 피하는 것이 좋습니다.

리플렉션을 통해서 런타임에 사용되는 타입을 확인해보고 싶었는데, 방법을 찾지 못했네요.

대신 디컴파일해서 확인할 수 있습니다.

```text
  public static void main(java.lang.String[]);
    Code:
       0: new           #2                  // class java/util/ArrayList
       3: dup
       4: invokespecial #3                  // Method java/util/ArrayList."<init>":()V
       7: astore_1
       8: aload_1
       9: ldc           #4                  // String hello
      11: invokevirtual #5                  // Method java/util/ArrayList.add:(Ljava/lang/Object;)Z
      14: pop
      15: return
```

그냥 초기화만 하면, 타입을 확인할 수 없어서 추가를 통해 확인했습니다.

실제로 최신 기능이라 정적 분석에서 제대로 잡아주지 못하는 모습을 보이기 때문에 주의해야 합니다.

### 비표시 타입에 대한 타입 추론

Java에는 표시할 수 없는 여러 타입이 있습니다. 존재하지만 명시적으로 작성할 수 없습니다. 대표적으로 익명 클래스가 있습니다. Java 코드에서 분명히 필드와 메서드를 추가할 수 있지만, 이름을 사용할 수는 없습니다.

다이아몬드 오퍼레이터는 익명 클래스와 사용할 수 없으며, `var`는 그보다는 덜 제한적이어서 익명클래스나 교차 타입(제네릭에서)에서 사용할 수 있습니다. 교차 타입에 대해서는 아래의 참고 글을 읽어봅시다.

`var`를 사용하면 익명 클래스를 보다 효과적으로 사용할 수 있습니다.

```java
Object productInfo = new Object() {
        String name = "Apple";
        int total = 30;
};
System.out.println("name = " + productInfo.name + ", total = " + productInfo.total);
```

위의 코드는 컴파일 되지 않습니다. 왜냐하면 `Object` 클래스에는 `name`과 `total` 이라는 필드가 존재하지 않기 때문입니다.

`var`를 사용하면 이 문제를 해결할 수 있습니다.

```java
var productInfo = new Object() {
        String name = "Apple";
        int total = 30;
};
System.out.println("name = " + productInfo.name + ", total = " + productInfo.total);
```

이 코드는 정상적으로 컴파일 되고, 필드 참조가 가능합니다.

`System.out.println(productInfo.getClass().getName());` 의 출력은 `~$1` 이라는 명칭으로 익명클래스의 클래스 이름이 컴파일 되었음을 알 수 있습니다.

또 다른 방식은 익명 클래스를 중간 값의 저장소로 활용하는 것입니다.

```java
var products = List.of(
    new Product(10, 3, "Apple"),
    new Product(5, 2, "Banana"),
    new Product(17, 5, "Pear"));

var productInfos = products
    .stream()
    .map(product -> new Object() {
        String name = product.getName();
        int total = product.getStock() * product.getValue();
    })
    .collect(toList());

productInfos.forEach(prod -> System.out.println("name = " + prod.name + ", total = " + prod.total));

This outputs:
name = Apple, total = 30
name = Banana, total = 10
name = Pear, total = 85
```

와일드 카드는 프로그래머에게 복잡한 와일드 카드 관련 오류 메시지를 피하기 위해서 타입 추론이 허용되지 않습니다.

### 결론

타입 추론은 가독성을 높일 수 있는 경우와 그것을 사용한 것이 유용한 경우에만 선택적으로 사용해야 합니다.

이 말의 의미는 편하게 코드를 작성하기 위해 `var`를 쓰는 것이 아니라, 가독성을 높이는 목적으로 사용해야 한다는 의미입니다.

`var`의 유형을 개발자가 추적할 수 없거나 어려운 경우엔 `var`를 사용하지 않는 것이 좋습니다.

### 추가 사항

Java11에서는 람다 표현식에 `var`를 쓸 수 있습니다. 아래의 JEP-323을 참고하시면 됩니다.

### 참고

<https://developer.oracle.com/java/jdk-10-local-variable-type-inference.html>

[자바 제네릭 교차 타입](https://itnext.io/java-generics-intersection-types-23b2fbdddfbb)

[JEP-323](http://openjdk.java.net/jeps/323)

---

[학습할 것으로](#학습할-것)

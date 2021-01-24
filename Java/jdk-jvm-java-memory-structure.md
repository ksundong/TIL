# JDK, JVM, 자바 메모리 구조

## JDK?

Java Development Kit의 약자로 Java 언어를 개발함에 있어서 필수적인 요소를 모아둔 것입니다.

현재(2021년) 기준 가장 최신 버전은 16버전이고, 올 하반기에 LTS 버전인 17버전이 나오게됩니다.
가장 대중적으로 사용되는 JDK 버전은 8이나, 11로 Migration하는 것이 권장되고 있습니다.
8버전, 11버전, 17버전을 LTS(Long Term Support) 버전이라고 부르며 9~10, 12~16 버전은 LTS 버전이 아니어서 실제 환경에서는 권장되지 않습니다.

각 버전별로는 특징적인 차이가 포함되고, 컴파일러의 최적화 방식이 바뀌거나, 기본적인 GC 알고리즘이 교체되기도 합니다. 예를 들어 자바 8에서는 람다와 스트림, 옵셔널 등의 기존 자바와는 궤를 달리하는 함수형 프로그래밍을 언어 차원에서 지원하게 되었습니다.

옛날 면접문제로 많이들 물어봤던 것이 JDK와 JRE의 차이가 무엇이냐는 질문입니다. 요즘엔 면접문제로 나오는 문제는 아닙니다.
JRE는 Java Runtime Environment의 약자로 자바를 실행할 수 있는 환경을 의미합니다. JDK는 JRE가 포함되어있습니다. 흔히 말하는 Java 가상 머신(JVM)이 여기 포함됩니다.

JDK와 JRE는 벤더 별로 배포를 할 수 있으며, 이는 내부 구현방식이 다를 수도 있다는 의미입니다.
최근에는 Amazon Corretto를 많이 사용하는 추세입니다. (Oracle JDK는 11버전부터 유료 라이선스입니다.)
사실 어떤 벤더를 사용해도 상관은 없으나, Adopt Open JDK는 권장하지 않습니다. (기업용으로 사용가능한지 테스트하는 것에서 통과하지 못한 JDK입니다.)

## JVM?

Java Virtual Machine의 약자로, 자바 가상 머신을 의미합니다. 자바는 "Write once, run anywhere"를 표방하고 있습니다. 이를 가능하게 해주는 것이 바로 자바 가상 머신입니다.

이전에 학습했던 내용이 있어 이에 대해 링크를 첨부합니다.

[백기선님 온라인 스터디 1주차 - JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.](https://velog.io/@dion/%EB%B0%B1%EA%B8%B0%EC%84%A0%EB%8B%98-%EC%98%A8%EB%9D%BC%EC%9D%B8-%EC%8A%A4%ED%84%B0%EB%94%94-1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80)

## 자바의 메모리 구조

자바의 메모리 구조는 면접 예상 질문에서는 자주 나왔던 것 같은데, 실질적으로 메모리 구조에 대해서 물어본 회사는 없었던 것 같습니다. 아마 이미 이것도 자바 개발자에게 있어서 상식수준으로 올라온 것 같습니다.

보통 Runtime Data Area라고 부르며, 메서드 영역, 힙 영역, 스택 영역, PC Register, Native Method Stack으로 구분합니다.

1. 메서드 영역
2. 힙 영역
3. 스택 영역
4. PC 레지스터
5. Native Method Stack

1, 2번이 모든 스레드가 공유하는 영역이고, 3, 4, 5번은 각 스레드 별로 생성되고 공유되지 않는 영역입니다.

### 메서드 영역

- 클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보
- 메소드 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보
- Type 정보(Class, Interface ...)
- Constant Pool(상수 풀: 문자 상수, 타입, 필드, 객체 참조)
- static 변수, final class 변수가 생성됨.

### 힙 영역

- new 키워드로 생성된 객체와 배열이 생성되는 지역
- **메소드 영역에 로드된 클래스만** 생성이 가능
- GC가 참조되지 않는 메모리를 확인하고 제거하는 영역

힙 메모리의 구조는 다음과 같습니다.

![https://www.betsol.com/wp-content/uploads/2017/06/java-memory-management-1.jpg](https://www.betsol.com/wp-content/uploads/2017/06/java-memory-management-1.jpg)

[이미지 출처](https://www.betsol.com/blog/java-memory-management-for-java-virtual-machine-jvm/)

Heap 영역은 Young 영역과 Old 영역으로 나눌 수 있습니다.

Young 영역은 Eden 영역과 Survivor 영역 2개로 나누어 집니다. Young 영역은 새 객체나 비교적 새 객체들이 존재하는 공간입니다. Eden 영역이 가득차면 GC가 일어나고, 여기서 살아남은 객체가 Survivor 영역으로 이동합니다. 그리고 Survivor 영역도 가득차면 해당 Survivor 영역을 GC하고 다른 Survivor 영역으로 옮깁니다. 이렇게 계속해서 GC를 반복하다 보면 오래 살아남는 객체는 Old 영역으로 이동하게 됩니다. 이를 Minor GC라고 합니다.

Old 영역이 가득 차는 경우 Old 영역에 대한 GC가 일어나는데 이를 Major GC라고 하며, 이와 동시에 Minor GC도 일어나므로 Full GC라 지칭하는 경우도 있습니다.

이렇게 메모리 영역을 나눈 이유는 대부분의 객체는 수명이 짧기 떄문입니다. 또한 이러한 GC가 동작할 때, 애플리케이션이 일시적으로 멈추므로 이를 방지하기 위함입니다. 보통 Young 영역의 객체들이 더 빨리 할당 해제되는 경향이 있습니다.

최근 릴리스에는 keep 영역으로 불리는 Young 영역의 한 부분이 예약되어 있습니다. 이는 새로 생성되자 마자 GC가 발생하여 더 높은 영역으로 승격되는 것을 방지합니다.

[Java Memory Management for Java Virtual Machine (JVM)](https://www.betsol.com/blog/java-memory-management-for-java-virtual-machine-jvm/)

### 스택 영역

- 지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역
- 참조 값이 있는 영역. 힙 메모리영역의 메모리주소를 가지고 있습니다.
- 메소드를 호출할 때 마다 개별적으로 스택이 생성됩니다. (Call Stack)

### PC Register

- Thread가 생성될 때 마다 생성되는 영역, PC를 저장하고 있는 영역
- CS에서 다루는 PC랑 다른 것입니다.

### Native Method Stack

- 자바 외의 언어로 작성된 네이티브 코드를 실행하기 위한 영역(C, C++ 등)

# 자바 메모리 구조

[참고](https://jeong-pro.tistory.com/148)

[JVM Run-Time Data Area Specification](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.5)

## Runtime Data Area

JVM의 메모리 영역. 자바 애플리케이션을 실행할 때 사용되는 데이터를 저장하는 영역입니다,

Method Area, Heap Area, Stack Area, PC Register, Native Method Stack으로 구분할 수 있습니다.

1. Method Area - 코드영역(?)
   - 클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보같은 필드 정보
   - 메소드 이름, 리턴 타입, 파라미터, 접근 제어자 정보같은 메소드 정보
   - Type 정보(Class, Interface ...)
   - Constant Pool(상수 풀: 문자 상수, 타입, 필드, 객체 참조)
   - static 변수, final class 변수가 생성됨.
2. Heap Area
   - new 키워드로 생성된 객체와 배열이 생성되는 지역
   - **메소드 영역에 로드된 클래스만** 생성이 가능
   - GC가 참조되지 않는 메모리를 확인하고 제거하는 영역
3. Stack Area
   - 지역 변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역
   - 참조 값이 있는 영역. 힙 메모리영역의 메모리주소를 가지고 있는다.
   - 메소드를 호출할 때 마다 개별적으로 스택이 생성된다. (Call Stack)
4. PC Register
   - Thread가 생성될 때 마다 생성되는 영역, PC를 저장하고 있는 영역
   - CS에서 다루는 PC랑 다른 것입니다.
5. Native Method Stack
   - 자바 외 언어로 작성된 네이티브 코드를 실행하기위한 영역(보통 C, C++)

**1, 2 번은 모든 쓰레드가 공유합니다.**

**3, 4, 5 번은 각각의 쓰레드마다 생성되고 공유되지 않습니다.**

## Heap Area & Garbage Collector

힙 영역은 GC의 주요 대상(Stack 영역과 Method 영역도 대상임)입니다.

힙 영역의 구분(eden, survivor1, survivor2, old, permanent) ⇒ survivor는 두 개로 나뉜다는 것이 중요합니다.

1. eden (new)
2. survivor1 (new)
3. survivor2 (new)
4. old
5. permanent Generation (Java 8에선 Metaspace로 변경된 영역) 

Java 8까지는 default GC 방식이 MSC(Mark-Sweep-Compact)고 Java 9부터는 G1(Garbage First)로 변경되었습니다.

GC의 구분 (Minor GC, Major GC)

1. Minor GC: New 영역에서 일어나는 GC

   1. 최초에 객체가 생성되면 Eden 영역에 생성된다.
   2. Eden 영역에 데이터가 가득차게 되면 첫 번째 GC가 일어난다.
   3. survivor1 영역에 Eden 영역의 메모리를 그대로 복사한다. 그리고 survivor1 영역을 제외한 다른 영역의 객체들을 제거한다. 너무 큰 객체는 바로 Old 영역으로 이동한다.
   4. Eden 영역과 Survivor1 영역이 가득차게 되면, 참조되고 있는 객체만 survivor2 영역으로 복사한다.
   5. Survivor2 영역을 제외한 다른 영역의 객체를 제거한다.
   6. 위의 과정 중에 일정 횟수이상 참조되고 있는 객체를 survivor2영역에서 old 영역으로 이동시킨다.
   7. 위의 과정을 반복한다 survivor2영역이 가득차기 전까지 old영역으로 계속 비운다.

2. Major GC: Old 영역에서 일어나는 GC

   1. Old 영역의 모든 객체를 참조하고 있는지 확인한다.
   2. 참조되지 않은 객체들을 한 번에 제거한다.

   - Minor GC 보다 시간이 많이 걸리고, 실행중에 **GC를 제외한 모든 쓰레드가 중지**한다. (Stop the world)
     - 객체를 제거하면서, 메모리 영역에 구멍이 나게 되고 이를 없애기 위해 재구성(Compact)이 일어나기 때문에 모든 쓰레드가 정지한다.

3. Full GC: New 영역 및 Old 영역에서 일어나는 GC

> 흔히 말하는 call stack과 여기서 말하는 stack area ⇒ 실제로는 JVM Stack이 어떤 차이가 있는지 궁금했다.
>
> 나름대로의 결론을 내리자면 둘의 차이는 용어의 차이라고 생각된다.

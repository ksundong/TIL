# GC

## 가비지 컬렉션이란?

가비지 컬렉션은 이름만 들어서는 쓰레기(?)만 골라서 제거하는 느낌을 받을 수 있을 것 같습니다. 하지만 가비지 컬렉션은 모든 객체에 대해서 확인하고, 그 중 사용하지 않는 것을 제거하는 방식으로 동작합니다.

이를 간단히 표현해서 GC는 Mark와 Sweep이라고 알려진 두가지 방법으로 동작합니다.

- **Mark**: 메모리에서 사용하지 않는 부분을 식별하는 과정입니다.
- **Sweep**: Mark 과정에서 식별된 객체를 제거합니다.

### 장점

- 개발자가 메모리 관리를 해줄 필요가 없습니다. 메모리 할당/해제와 관련된 로직이 사라지게 되어, 개발자는 좀 더 작성하고자 하는 코드에 집중할 수 있습니다. GC는 자동으로 메모리에 여유 공간이 부족한 경우에 발생을 하고, 이 과정에서 사용하지 않는 객체가 메모리에서 제거되는 방식으로 동작합니다.
- Dangling Pointer에 대한 오버헤드가 없습니다. 이는 할당 해제된 객체의 메모리 위치를 포인터가 가지고 있는것을 말하는데, GC는 참조하지 않고 있는 객체만 제거하므로 이런 문제에서 해방될 수 있습니다.
- GC는 메모리 누수를 완벽히 보호해주지는 못하지만, 자동으로 어느정도 관리해줄 수 있습니다.

### 단점

- JVM이 객체 생성/제거를 관리해야 하므로 일반적인 애플리케이션 코드가 동작하는 것 보다는 많은 CPU를 사용합니다. 이는 조금 더 좋은 사양이 필요함을 의미합니다. 그리고 많은 메모리를 요구하는 동작에서 특히 영향이 큽니다.
- 프로그래머가 객체를 해제하고 할당하는 시점을 제어할 수 없습니다.
- 특정 GC 알고리즘은 GC가 동작하면서 프로그램이 예기치 않게 종료될 수 있습니다.
- GC는 자동으로 동작하지만 수동으로 메모리 할당/해제를 해주는 것에 비해 효율적으로 동작하지는 않습니다.

### GC를 알아야 하는 이유

- GC가 메모리 누수를 막아주지 못합니다. GC의 동작원리를 알아야 메모리 누수를 막아줄 수 있습니다.
- 언제 GC가 일어나는지 알고, 어떻게 해야 GC를 최대한 적게 발생시킬 수 있는지를 알아야 좋은 성능의 프로그램이 될 수 있습니다.
- 결론적으로 좋은 프로그램을 만들기 위해서 메모리 관리에 대해 알 필요가 없는 것이 아니라, 많이 생각해보아야 합니다.

[JVM Garbage Collectors | Baeldung](https://www.baeldung.com/jvm-garbage-collectors)

### Serial GC

가장 간단한 GC의 구현체입니다. 이는 싱글 스레드로 동작하기 때문에 GC 수행중 전체 애플리케이션이 멈추게 됩니다. 따라서 서버 환경에서는 이 GC는 사용하지 않는 편입니다.

이는 대부분의 클라이언트에서 실행되는 애플리케이션용 GC라고 볼 수 있습니다.

이를 적용하기 위해서는 다음 인수를 입력하면됩니다.

```java
java -XX:+UseSerialGC -jar Application.java
```

### Parallel GC

JVM이 기본적으로 사용하는 GC입니다. 때때로 Throughput Collector라고 불린다고 합니다. GC를 하기 위해서 여러 개의 스레드를 사용하는 것이 특징입니다. 그러나, GC를 하면서 애플리케이션이 멈추는 것은 동일합니다.

우리가 이 GC를 사용한다면, GC thread의 개수와 중지하는 시간, 처리량, footprint(heap size)를 지정할 수 있습니다.

GC 스레드의 수는 커맨드라인 옵션인 `-XX:ParallelGCThreads=<N>` 을 사용하여 설정할 수 있습니다.

최대 일시 중지 시간 목표(두 GC 사이의 간격)은 `-XX:MaxGCPauseMillis=<N>` 옵션을 사용해서 제어할 수 있습니다.

최대 처리량 목표(가비지 컬렉션을 수행하는 대 소요된 시간 대 외부에 소요된 시간)은 `-XX:GCTimeRatio=<N>` 을 사용해서 지정할 수 있습니다.

그리고 최대 힙 footprint(프로그램이 실행하는 동안 필요한 힙 메모리의 양)은 `-Xmx<N>` 으로 지정할 수 있습니다.

Parallel GC를 활성화하기 위해서는 다음과 같은 인수를 입력하면 됩니다.

```java
java -XX:+UseParallelGC -jar Application.java
```

### CMS(Concurrent Mark Sweep) GC

Concurrent Mark Sweep GC는 가비지 컬렉션을 위해 여러 개의 가비지 컬렉터를 사용하는 GC 알고리즘입니다. 이는 가비지 컬렉션의 일시 중지를 최소화 하는데 목적을 둔 GC입니다.

이러한 GC는 느리게 응답하지만, GC를 하기 위해서 아예 응답을 중지하지는 않습니다.

주의할 점은 GC가 동시에 동작하므로, 동시 프로세스가 동작하는 동안 `System.gc()` 를 사용하는 등의 명시적 GC를 호출하면 동시 모드가 실패하거나 중단된다는 것입니다. (사실 gc를 호출하는 것은 안티패턴입니다.)

총 시간의 98%가 GC에 의해 소비되고, 2%의 힙이 복구된다면 OOM이 발생하게 됩니다. 필요하다면, `-XX:UseGCOverheadLimit` 을 사용해서 이 기능을 비활성화 할 수 있습니다.

CMS CG를 활성화하기 위해서는 다음과 같은 플래그를 주면 됩니다.

```java
java -XX:+UseParNewGC -jar Application.java
```

Java9에서 Deprecated 되었고, 14에서는 지원까지 완전히 제거되었습니다.

### G1 GC(throughput/latency balance)

G1(Garbage First) GC는 큰 메모리 공간이 있는 멀티 프로세서 기기에서 실행되는 애플리케이션을 위해 설계되었습니다. 이는 JDK7 Update4에서 사용가능해졌으며, 이후 버전에서도 사용가능합니다.

G1 GC는 성능이 더 좋기 때문에 CMS GC를 대체하는 GC입니다.

다른 GC와는 다르게 G1 GC는 힙을 동일한 크기의 힙 지역 세트(1MB 정도)로 분류하고 각각은 연속적인 범위의 가상 메모리입니다. GC를 수행할 때, G1은 힙 영역 전체에서 객체의 활성 상태를 확인하기 위해서 전체 객체를 Marking하는 동시 전역 마킹 단계를 표시합니다.

Mark 페이즈가 완료된 후에는 G1은 영역이 대부분 비어있는 영역을 인식합니다. 그 다음 Sweeping이라고 알려진 단계에서 이 영역에 대해 먼저 Garbage를 수집하며 일반적으로 상당한 양의 여유 공간을 생성하게 됩니다. 이것이 Garbage First라고 불리는 이유입니다. (사실 얘기만 들어서는 잘 모르겠습니다..)

> G1 GC의 기본 원리는 자바 Heap 메모리를 회수할 때 최대한 살아 있는객체가 적게 들어 있는 region을 수집하는 것입니다.

G1 GC를 활성화 하는 방법은 다음의 인수를 입력하면 됩니다.

Java 9부터 기본 GC입니다.

```java
java -XX:+UseG1GC -jar Application.java
```

[[Java] Java G1 GC의 특성에 따른 Full GC 회피 튜닝 방법](https://logonjava.blogspot.com/2015/08/java-g1-gc-full-gc.html)

[Luavis' Dev Story - G1: Garbage first garbage collector](https://b.luavis.kr/server/g1-gc)

### ZGC

11에서 소개되고,Java 15에서 릴리즈 된 GC입니다. default GC는 G1 GC입니다.

[JEP 377: ZGC: A Scalable Low-Latency Garbage Collector (Production)](https://openjdk.java.net/jeps/377)

ZGC는 확장 가능한 낮은 지연을 가진 GC입니다.

- 정지 시간이 최대 10ms를 초과하지 않습니다.
- 모든 비싼 작업을 동시에 수행합니다.(스레드 스택 스캔을 제외한marking, compaction, reference processing, relocation set selection, StringTable cleaning, JNI WeakRef cleaning, JNI GlobalRefs scanning and class unloading) 이 때문에 위의 사항을 만족할 수 있습니다.

이런 특징 때문에 **낮은 지연 시간**이 필요하거나, 매우 큰 힙(수 테라 바이트)를 사용하는 애플리케이션에 적합합니다.

11버전에서 ZGC를 사용하는 방법은 다음의 인수를 입력하면 됩니다.(11버전에선 실험적 기능입니다.)

```java
java -XX:+UnlockExperimentalVMOptions -XX:+UseZGC -jar Application.java
```

ZGC에서 가장 중요한 옵션 조정은 최대 힙 크기를 설정하는 것입니다.(`-Xmx`)
ZGC는 동시 수집기이기 때문에 힙이 애플리케이션의 라이브 셋을 수용할 수 있고, GC가 동작할 때 할당을 할 수 있도록 힙에 충분한 헤드 룸이 있도록 최대 힙 크기를 선택해야 합니다.

이러한 헤드 룸이 얼마나 많이 필요한지는 할당하는 속도, 애플리케이션의 라이브 셋 크기에 따라 다릅니다. 일반적으로는 많은 메모리를 제공하면 좋지만, 메모리의 낭비는 바람직하지 않으므로, 균형점을 찾아야합니다.

동시 GC 스레드 수 설정 옵션입니다.(`-XX:ConcGCThreads`) ZGC는 휴리스틱 방식으로 동작하는데 일반적으로 잘 동작하지만 조정이 필요할 수 있습니다. 이 값은 GC가 CPU 시간을 얼마나 차지하는지와 관계있고, 너무 많으면 CPU시간을 너무 많이 쓰게 되고 적을 때는 수집하는 것보다 많이 할당이 될 수 있습니다.

[HotSpot Virtual Machine Garbage Collection Tuning Guide](https://docs.oracle.com/en/java/javase/11/gctuning/z-garbage-collector1.html#GUID-A5A42691-095E-47BA-B6DC-FB4E5FAA43D0)

[ZGC(Z Garbage Collectors)](https://sarc.io/index.php/java/2098-zgc-z-garbage-collectors)

[ZGC, The Z Garbage Collector](https://johngrib.github.io/wiki/java-gc-zgc/)

[The Z Garbage Collector algorithm (JDK 15 version)](https://medium.com/globant/the-z-garbage-collector-algorithm-jdk-15-version-ca6da00b5281)

---

참고자료들

[Java Garbage Collection Basics](

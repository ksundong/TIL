# 10주차: 멀티스레드 프로그래밍

## 학습할 것

- [Thread 클래스와 Runnable 인터페이스](#thread-클래스와-runnable-인터페이스)
- [스레드의 상태](#스레드의-상태)
- [스레드의 우선순위](#스레드의-우선순위)
- [Main 스레드](#main-스레드)
- [동기화](#동기화)
- [데드락](#데드락)

## Thread 클래스와 Runnable 인터페이스

각 thread는 `Thread` 클래스의 인스턴스와 연결됩니다. `Thread` 객체를 사용하여 동시성 프로그래밍을 하는 두 가지 기본 전략이 있습니다.

- 스레드의 생성과 관리를 직접 제어하기 위해서는 애플리케이션이 비동기 작업을 시작해야 할 때마다 `Thread`를 인스턴스화 하면 됩니다.
- 나머지 애플리케이션에서 스레드 관리를 추상화하려면 애플리케이션의 작업들을 `executor`(?)로 전달합니다.

우리는 이 중 위의 방식만 학습할 것입니다. 하지만 두 가지 방식이 있다는 점은 기억해둡시다.

### Thread 정의하고 실행하기

Thread 인스턴스를 생성하는 애플리케이션은 반드시 스레드에서 실행될 코드를 제공해야합니다. 자바에서는 이를 위한 두 가지 방법을 제공하는데 `Thread` 클래스를 상속한 클래스를 정의하는 것과 `Runnable` 인터페이스를 구현하는 방법이 있습니다.

- `Runnable` 객체 제공. `Runnable` 인터페이스는 스레드에서 실행되는 코드를 포함한다는 의미의 `run` 메서드를 정의합니다. `Runnable` 객체는 `Thread` 생성자로 전달될 수 있습니다.  
   아래의 예제 코드는 기본적인 방식으로 `Runnable` 인터페이스를 구현하는 클래스를 선언하는 방식과 람다식을 사용해서 선언하는 방식을 사용합니다.

  ```java
  public class HelloRunnable implements Runnable {

    @Override
    public void run() {
      System.out.println("Hello World! This is Runnable Thread!");
    }

    public static void main(String[] args) {
      (new Thread(new HelloRunnable())).start();

      new Thread(() -> System.out.println("Hello, This is Lambda Thread!")).start();
      }
  }
  ```

  결과

  ```text
  Hello World! This is Runnable Thread!
  Hello, This is Lambda Thread!
  ```

- `Thread` 서브클래싱. `Thread` 클래스는 그 자체로 `Runnable` 인터페이스를 구현합니다. 하지만, `run` 메서드가 아무것도 하지 않는다는 특징이 있습니다. 애플리케이션은 `Thread`의 서브클래스가 될 수 있습니다.

  ```java
  public class HelloThread extends Thread {

    @Override
    public void run() {
      super.run();
      System.out.println("Hello World From Thread!");
    }

    public static void main(String[] args) {
      new HelloThread().start();
    }
  }
  ```

  결과

  ```text
  Hello World From Thread!
  ```

주목할만한 점은 둘 다 새로운 스레드를 생성시키기 위해서 `Thread`의 `start` 메서드를 실행했다는 점입니다.

일반적으로는 `Runnable` 객체로 스레드를 생성하는 방식을 사용하는데, 그 이유는 `Runnable` 객체가 `Thread` 이외의 클래스의 하위 클래스가 될 수 있기 때문입니다. `Thread`를 사용하는 방식은 간단한 애플리케이션에서는 사용하기 쉽지만, 작업 클래스가 `Thread`의 자손 클래스여야만 한다는 것이 한계점입니다. 그러므로 `Thread`를 사용했다면 `Runnable`로 분리를 하는것도 염두에 두어야 합니다. 왜냐하면, 이 방식이 더 유연한 것 뿐만아니라 더 높은 수준의 스레드 관리 API에도 적용할 수 있습니다.

`Thread` 클래스는 스레드 관리에 유용한 다수의 메소드를 정의해두고 있습니다. 여기에는 메서드를 실행하는 스레드에 대한 정보를 제공하거나 상태에 영향을 주는 `static` 메서드도 포함되어 있습니다. 그 외의 메서드는 스레드 및 `Thread` 객체를 관리하는 것과 관련된 다른 스레드에서 호출됩니다.

### 참고

[오라클 자바 튜토리얼(스레드 객체)](https://docs.oracle.com/javase/tutorial/essential/concurrency/threads.html)

### Sleep으로 실행 일시중지 시키기

`Thread.sleep` 메서드는 현재 스레드의 실행을 일정시간동안 중단시킵니다. 이는 컴퓨터 시스템에서 실행될 수 있는 다른 애플리케이션이나 애플리케이션의 다른 스레드에서 프로세서의 시간을 사용할 수 있도록 하는 효율적인 방법입니다. `sleep` 메서드는 지연을 위해서 사용될 수 있으며, 시간 요구사항이 있는 다른 스레드를 기다릴 수 있습니다.

`sleep`에는 두 가지 오버로드 된 버전이 존재합니다. 하나는 밀리초 단위고, 하나는 밀리초, 나노초 단위입니다.

주의할 점으로는 이 `sleep`의 시간은 OS에 의존적이기 때문에 정확하지 않을 수 있다는 점입니다. 또한 `sleep` 기간은 인터럽트에 의해 종료될 수 있기 때문에, 어떤 경우라도 `sleep`을 호출하면 지정된 시간동안 스레드가 일시 중단된다고 가정하면 안됩니다.

```java
public class SleepMessages {

  public static void main(String[] args) throws InterruptedException {
    String[] importantInfo = {
        "자바",
        "스프링",
        "백기선",
        "스터디 할래"
    };

    for (String s : importantInfo) {
      // 4초 일시 중지
      Thread.sleep(4000);
      // 메시지 출력
      System.out.println(s);
    }
  }
}
```

`main` 메서드에서 `throws InterruptedException`을 던지는 것을 볼 수 있습니다. 이는 슬립하고 있는 도중에 다른 스레드가 현재의 스레드를 인터럽트 하는 경우 발생합니다. 우리가 작성한 애플리케이션은 인터럽트를 발생시키는 다른 스레드를 정의하지 않았기 떄문에 `InterruptedException`이 발생하지 않았습니다.

#### 참고

[오라클 자바 튜토리얼(sleep)](https://docs.oracle.com/javase/tutorial/essential/concurrency/sleep.html)

### 인터럽트

인터럽트는 스레드가 수행중인 작업을 중지하고 다른 작업을 수행해야 함을 나타냅니다. 스레드가 인터럽트에 반응하는 것을 정확히 결정하는 것은 프로그래머에게 달려있지만, 스레드가 종료되는 것은 매우 일반적입니다.

스레드는 스레드가 인터럽트 될 `Thread` 객체에 대해 `interrupt`를 호출하여 인터럽트를 보냅니다. 인터럽트 메커니즘이 올바르게 작동하려면 인터럽트 된 스레드가 자체적으로 인터럽트를 지원해야합니다.

#### 인터럽트 지원

어떻게 자체적으로 인터럽트를 지원하도록 하게 할 수 있을까요? 이는 현재 무엇을 하고 있는지에 달려있습니다. 스레드가 자주 메서드에서 `InterruptedException`을 던진다면, 간단하게 `run` 메서드에서 이를 잡은 다음 반환하면 됩니다. `SleepMessages` 예제의 코드를 조금 수정해보자면, 인터럽트가 발생했을 때, 이를 자체적으로 처리하는 방법을 구현할 수 있습니다.

```java
public static void main(String[] args) {
  String[] importantInfo = {
      "자바",
      "스프링",
      "백기선",
      "스터디 할래"
  };

  for (String s : importantInfo) {
    // 4초 일시 중지
    try {
      Thread.sleep(4000);
    } catch (InterruptedException e) {
      return;
    }
    // 메시지 출력
    System.out.println(s);
  }
}
```

`sleep`과 같이 `InterruptedException`을 던지는 많은 메서드는 현재 작업을 취소하고 인터럽트가 수신될 때 즉시 반환하도록 설계되었습니다.

`InterruptedException`을 던지는 메서드를 호출하지 않고 스레드가 오랫동안 지속되면 어떻게 될까요? 그럴 때는 반드시 `Thread.interrupted`를 주기적으로 호출해야합니다. 이는 인터럽트가 수신되면 `true`를 반환합니다.

간단한 경우에는 그냥 `return`으로 스레드를 종료시켜도 되지만, 복잡한 경우에는 `InterruptedException`을 던져주는 것이 더 합리적 일 수 있습니다.

```java
if (Thread.interrupted()) {
  throw new InterruptedException();
}
```

이렇게 하면 중앙에서 `catch` 절로 핸들링을 해줄 수 있습니다.

#### 인터럽트 상태 플래그

인터럽트 메커니즘은 `interrupt status`로 알려진 내부 플래그를 사용해 구현됩니다. `Thread.interrupt`는 이 플래그를 설정합니다. 스레드가 static 메서드 `Thread.interrupted`를 호출하여 인터럽트를 확인하면 인터럽트 상태는 지워집니다. `isInterrupted`라는 non-static 메서드는 한 스레드가 다른 스레드의 인터럽트 상태를 묻는 데 사용하고, 이는 인터럽트 상태 플래그를 변경하지 않습니다.

관례적으로, `InterruptedException`을 던져서 종료되는 모든 메서드는 종료될 때, 인터럽트 상태를 비웁니다. 그러나, 다른 스레드가 인터럽트를 호출해서 인터럽트 상태가 즉시 다시 설정되는 것은 가능합니다.

#### 참고

[오라클 자바 튜토리얼(인터럽트)](https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html)

### 조인

`join` 메서드는 스레드가 다른 스레드가 완료될 때 까지 기다리도록 합니다. `t`라는 `Thread` 객체가 현재 실행중인 스레드인 경우에

```java
t.join();
```

이 코드를 사용하면, 현재 스레드는 `t`의 스레드가 종료될 때 까지 실행을 멈추게 됩니다. `join`의 오버로딩은 프로그래머로 하여금 얼마나 기다릴지 정하도록 합니다. 그러나 `sleep`과 마찬가지로 `join`도 OS의 시간에 의존하게 됩니다. 따라서 `join`이 우리가 정의한 만큼 정확하게 대기할 것이라고 가정하면 안됩니다.

`sleep`처럼 `join`도 인터럽트가 발생하면 `InterruptedException`과 함께 종료됩니다.

#### 참고

[오라클 자바 튜토리얼(조인)](https://docs.oracle.com/javase/tutorial/essential/concurrency/join.html)

### 예제 코드

```java
public class SimpleThreads {

  static void threadMessage(String message) {
    String threadName = Thread.currentThread().getName();
    System.out.format("[%s] %s: %s%n", LocalDateTime.now().format(DateTimeFormatter.ofPattern("YYYY-MM-dd hh:mm:ss.SS")), threadName, message);
  }

  private static class MessageLoop implements Runnable {

    @Override
    public void run() {
      String[] importantInfo = {
          "감자",
          "고구마",
          "닭가슴살",
          "양상추",
          "달걀"
      };

      try {
        for (String info : importantInfo) {
          // 4초 쉬기
          Thread.sleep(1000 * info.length());
          // 메시지 출력
          threadMessage(info);
        }
      } catch (InterruptedException e) {
        threadMessage("아직 안끝났어요!");
      }
    }
  }

  public static void main(String[] args) throws InterruptedException {
    // 딜레이, 1시간
    long patience = 1000 * 60 * 60;

    // 커맨드라인 인자가 있으면 변경(단위는 초)
    if (args.length > 0) {
      try {
        patience = Long.parseLong(args[0]) * 1000;
      } catch (NumberFormatException e) {
        System.err.println("숫자를 입력해주세요.");
        System.exit(1);
      }
    }

    threadMessage("MessageLoop 스레드를 시작합니다.");
    long start = System.currentTimeMillis();
    Thread t = new Thread(new MessageLoop());
    t.start();

    threadMessage("MessageLoop 스레드가 끝나기 까지 기다리고 있습니다.");
    while (t.isAlive()) {
      threadMessage("기다리는중.......");
      t.join(1000); // 1초 기다립니다.
      if ((System.currentTimeMillis() - start) > patience && t.isAlive()) {
        threadMessage("기다리다 지쳐버렸다! 인터럽트다!!!!!!!");
        t.interrupt();
        t.join(); // 이 코드가 필요한 이유가 무엇일까요? 스레드가 종료될 때 까지 기다리기 위함입니다.
      }
    }
    threadMessage("끝났다!!!");
  }
}
```

결과

```text
[2021-01-20 10:20:16.95] main: MessageLoop 스레드를 시작합니다.
[2021-01-20 10:20:16.99] main: MessageLoop 스레드가 끝나기 까지 기다리고 있습니다.
[2021-01-20 10:20:16.99] main: 기다리는중.......
[2021-01-20 10:20:17.99] main: 기다리는중.......
[2021-01-20 10:20:19.00] Thread-0: 감자
[2021-01-20 10:20:19.00] main: 기다리는중.......
[2021-01-20 10:20:20.00] main: 기다리는중.......
[2021-01-20 10:20:21.01] main: 기다리는중.......
[2021-01-20 10:20:22.00] Thread-0: 고구마
[2021-01-20 10:20:22.01] main: 기다리는중.......
[2021-01-20 10:20:23.02] main: 기다리는중.......
[2021-01-20 10:20:24.02] main: 기다리는중.......
[2021-01-20 10:20:25.02] main: 기다리는중.......
[2021-01-20 10:20:26.00] Thread-0: 닭가슴살
[2021-01-20 10:20:26.03] main: 기다리는중.......
[2021-01-20 10:20:27.03] main: 기다리는중.......
[2021-01-20 10:20:28.04] main: 기다리는중.......
[2021-01-20 10:20:29.00] Thread-0: 양상추
[2021-01-20 10:20:29.04] main: 기다리는중.......
[2021-01-20 10:20:30.04] main: 기다리는중.......
[2021-01-20 10:20:31.01] Thread-0: 달걀
[2021-01-20 10:20:31.01] main: 끝났다!!!
```

#### 참고

[오라클 자바 튜토리얼(예제)](https://docs.oracle.com/javase/tutorial/essential/concurrency/simple.html)
[스택오버플로우 - interrupt 뒤에 join을 사용하는 이유](https://stackoverflow.com/questions/27469761/thread-join-and-thread-interrupt-doesnt-stop-the-thread)

## 스레드의 상태

자바의 스레드는 임의의 시점에서 아래의 상태 중 하나로 존재합니다.

1. New
1. Runnable
1. Blocked
1. Waiting
1. Timed Waiting
1. Terminated

아래의 다이어그램은 임의의 순간의 다양한 스레드의 상태를 나타냅니다.

![thread status diagram](https://i.imgur.com/WaCy0Jf.png)

이미지 출처: <https://www.geeksforgeeks.org/lifecycle-and-states-of-a-thread-in-java/>

원본 출처: [Core Java Vol 1, 9th Edition, Horstmann, Cay S. & Cornell, Gary_2013](https://www.google.co.in/search?q=Core+Java+Vol+1%2C+9th+Edition%2C+Horstmann%2C+Cay+S.+%26+Cornell%2C+Gary_2013&oq=Core+Java+Vol+1%2C+9th+Edition%2C+Horstmann%2C+Cay+S.+%26+Cornell%2C+Gary_2013&aqs=chrome..69i57.383j0j7&sourceid=chrome&ie=UTF-8)

1. New Thread: 새로운 스레드가 생성되면, `new` 상태입니다. 이 상태에 있는 스레드는 아직 실행되지 않았습니다. 스레드가 `new` 상태에 있으면, 코드가 아직 실행되지 않았고, 시작되지 않은 것입니다.
1. Runnable 상태: 실행할 준비가 된 스레드는 `runnable` 상태로 이동합니다. 이 상태에서는 스레드가 이미 실행중이거나, 언제든지 실행될 준비가 되어있을 수 있습니다. 스레드가 실행될 시간을 주는 것은 스레드 스케쥴러의 책임입니다.  
   멀티 스레드 프로그램에서는 각 스레드에 고정된 시간이 할당됩니다. 각각의 모든 스레드는 잠시동안 실행 된 다음 일시 중지하고 다른 스레드가 실행 될 수 있도록 CPU를 다른 스레드에 양도합니다. 이렇게 되면, 실행할 준비가 되었거나, CPU를 기다리고 있거나, 이미 실행중인 모든 스레드들이 `runnable` 상태로 있게됩니다.
1. Blocked/Waiting 상태: 스레드가 일시적으로 비활성상태가 됬을 때, 스레드는 다음 두 가지 상태 중 하나의 상태가 됩니다.

   - Blocked
   - Waiting

   예를 들어, 스레드가 I/O가 끝나길 디가리고 있을 때, 스레드는 `blocked` 상태가 됩니다.  
   `blocked/waiting` 스레드를 다시 활성화하고 스케줄링하는 것은 스레드 스케쥴러의 책임입니다. 이런 상태에 있는 `runnable` 상태로 이동할 때 까지 더이상 실행을 계속 할 수 없습니다. 어느 스레드건 이 상태에 있으면 CPU cycle을 소비하지 않습니다.  
   `blocked` 상태에 있는 스레드는 현재 다른 스레드에 의해 lock 되어있는 보호된 코드 영역에 액세스하려고 했을 때, 차단된 상태입니다. 보호된 영역이 unlock되면 스케쥴러는 block된 스레드 중 하나를 선택해서 실행 가능 상태로 이동합니다.  
   또, 스레드는 조건에 따라 다른 스레드를 기다리는 상태에 있을 수도 있습니다. 이 조건이 충족되면 스케쥴러에 알림이 전송되고, 대기중인 스레드가 실행 가능 상태로 이동됩니다.  
   현재 실행중인 스레드가 `blocked/waiting` 상태로 이동하면, `runnable` 상태의 다른 스레드가 스레드 스케쥴러에 의해 실행되도록 스케쥴됩니다.

1. Timed Waiting: 스레드는 `time out` 매개변수가 있는 메서드를 호출할 때, `waiting` 상태에 있게됩니다. 스레드는 `timeout`이 완료될 때까지 기다리거나, 알림이 수신될 때까지 이 상태에 있습니다. 예를 들어, 스레드가 sleep 또는 조건부 대기를 호출하면, `timed waiting` 상태로 이동하게 됩니다.
1. Terminated State: 스레드는 다음과 같은 이유로 종료됩니다.

   - 일반적으로 종료되었기 때문입니다. 이는 스레드의 코드가 완전히 프로그램에 의해 실행되었을 때 발생합니다.
   - `segmentation`(?) 실패 또는 처리되지 않은 예외와 같은 비정상적인 오류 이벤트가 발생했기 때문입니다.

   `terminated` 상태에 있는 스레드는 더이상 CPU cycle을 소비하지 않습니다.

### 자바 코드로 스레드의 상태 구현하기

Java에서 현재 스레드의 상태를 얻는 방법은 `Thread.getState()` 메서드를 사용하는 것입니다. 자바는 `java.lang.Thread.State` 라는 클래스를 Enum으로 선언하여, 스레드의 상태를 정의합니다.

1. `Thread.State NEW`
2. `Thread.State RUNNABLE`
3. `Thread.State BLOCKED`
4. `Thread.State WAITING`
5. `Thread.State TIMED_WAITING`
   - timeout이 없는 `Object.wait`
   - timeout이 없는 `Thread.join`
   - `LockSupport.park`
6. `Thread.State TERMINATED`
   - `Thread.sleep`
   - timeout이 있는 `Object.wait`
   - timeout이 있는 `Thread.join`
   - `LockSupport.parkNanos`
   - `LockSupport.parkUntil`

### 참고

[Geeks for Geeks-자바에서의 스레드의 생명주기와 상태](https://www.geeksforgeeks.org/lifecycle-and-states-of-a-thread-in-java/)

## 스레드의 우선순위

## Main 스레드

멀티스레드 실행은 자바 플랫폼의 필수 기능입니다. 모든 애플리케이션에는 적어도 하나의 스레드가 있고 메모리 관리 및 신호 처리와 같은 시스템 스레드를 포함하면 여러개라고 볼 수도 있습니다.

애플리케이션 개발자의 관점에서 보면, 우리는 `main thread`라고 부르는 한 스레드로부터 시작합니다. 이 스레드는 추가적인 스레드를 만들 수 있는 능력이 있습니다.

### 참고

[오라클 자바 튜토리얼(프로세스와 스레드)](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)

## 동기화

## 데드락

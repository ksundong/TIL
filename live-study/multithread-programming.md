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

### 예제 코드

```java
package dev.idion.threadstatus;

public class Test implements Runnable {

  public static Thread thread1;
  public static Test obj;

  public static void main(String[] args) {
    obj = new Test();
    thread1 = new Thread(obj);

    System.out.println("1. thread1을 생성하고 나서의 상태: " + thread1.getState());
    thread1.start();

    System.out.println("2. start 메서드를 실행하고 thread1의 상태: " + thread1.getState());
  }

  @Override
  public void run() {
    SimpleThread simpleThread = new SimpleThread();
    Thread thread2 = new Thread(simpleThread);

    System.out.println("3. thread2를 생성하고 나서의 상태: " + thread2.getState());
    thread2.start();

    System.out.println("4. start 메서드를 실행하고 thread2의 상태:" + thread2.getState());

    try {
      Thread.sleep(200);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("5. sleep 호출 이후 thread2의 상태:" + thread2.getState());

    try {
      // thread2가 종료될 때 까지 대기
      thread2.join();
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
    System.out.println("7. thread2가 종료되었을 때의 상태: " + thread2.getState());
    System.out.println("8. thread2가 종료되었을 때의 thread1의 상태: " + thread1.getState());
  }
}

class SimpleThread implements Runnable {

  @Override
  public void run() {
    try {
      Thread.sleep(1500);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }

    System.out.println("6. join 했을 때, thread1의 상태" + Test.thread1.getState());

    try {
      Thread.sleep(200);
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }
}
```

출력

```text
1. thread1을 생성하고 나서의 상태: NEW
2. start 메서드를 실행하고 thread1의 상태: RUNNABLE
3. thread2를 생성하고 나서의 상태: NEW
4. start 메서드를 실행하고 thread2의 상태:RUNNABLE
5. sleep 호출 이후 thread2의 상태:TIMED_WAITING
6. join 했을 때, thread1의 상태WAITING
7. thread2가 종료되었을 때의 상태: TERMINATED
8. thread2가 종료되었을 때의 thread1의 상태: RUNNABLE
```

### 참고

[Geeks for Geeks-자바에서의 스레드의 생명주기와 상태](https://www.geeksforgeeks.org/lifecycle-and-states-of-a-thread-in-java/)

## 스레드의 우선순위

멀티 스레딩 환경에서 스레드 스케쥴러는 스레드의 우선순위에 따라 스레드에 프로세서를 할당합니다. 우리가 Java에서 thread를 만들 때마다, 항상 우선순위가 지정됩니다. 우선순위는 스레드를 만드는 동안 JVM에 의해 제공되거나, 프로그래머가 명시적으로 제공할 수 있습니다.  
스레드의 우선순위는 1에서 10 사이의 값만 허용됩니다. `Thread` 클래스에는 세 개의 static 변수가 우선순위를 위해 정의되어 있습니다.

- `MIN_PRIORITY`: 스레드가 가질 수 있는 우선순위의 최솟값입니다. 이 값은 1입니다.
- `NORM_PRIORITY`: 우선순위를 명시적으로 지정하지 않으면 스레드가 가지는 기본 우선순위입니다. 이 값은 5입니다.
- `MAX_PRIORITY`: 스레드의 가질 수 있는 우선순위의 최댓값입니다. 이 값은 10입니다.

### 스레드 우선순위 가져오거나 설정하기

- `public final int getPriority()`: `java.lang.Thread.getPriority()` 메서드는 해당 스레드의 우선 순위를 반환합니다.
- `public final void setPriority(int newPriority)`: `java.lang.Thread.setPriority()` 메서드는 새로운 우선순위 값으로 스레드의 우선순위를 바꿉니다. 이 메서드는 새로운 우선순위 값이 1~10 사이에 있지 않을 경우 `IllegalArgumentException`을 던질 수 있습니다.

### 예제 코드

```java
public class PriorityThread extends Thread {

  public static void main(String[] args) {
    PriorityThread t1 = new PriorityThread();
    PriorityThread t2 = new PriorityThread();
    PriorityThread t3 = new PriorityThread();

    t1.setName("1번 스레드");
    t2.setName("2번 스레드");
    t3.setName("3번 스레드");

    printPriority(t1, t2, t3);

    t1.setPriority(2);
    t2.setPriority(5);
    t3.setPriority(8);

    printPriority(t1, t2, t3);

    System.out.println("현재 실행중인 스레드: " + Thread.currentThread().getName());

    System.out.println("Main 스레드의 우선순위: " + Thread.currentThread().getPriority());

    Thread.currentThread().setPriority(Thread.MAX_PRIORITY);
    System.out.println("Main 스레드의 우선순위: " + Thread.currentThread().getPriority());
    startAll(t1, t2, t3);
  }

  private static void printPriority(PriorityThread... threads) {
    for (PriorityThread thread : threads) {
      System.out.println("[" + thread.getName() + "] 우선순위: " + thread.getPriority());
    }
  }

  private static void startAll(PriorityThread... threads) {
    for (PriorityThread thread : threads) {
      thread.start();
    }
  }

  @Override
  public void run() {
    System.out.println("[" + Thread.currentThread().getName() + "] run 메서드 내부");
    for (int i = 0; i < 5; i++) {
      try {
        Thread.sleep(1000);
        System.out.println("[" + Thread.currentThread().getName() + "] " + (i + 1) + "초");
      } catch (InterruptedException e) {
        return;
      }
    }
  }
}
```

출력(상황에 따라 다를 수 있음[멀티스레드 프로그램이므로])  
밑의 초는 그냥 어떻게 동작하는지 궁금해서 넣었습니다.

```text
[1번 스레드] 우선순위: 5
[2번 스레드] 우선순위: 5
[3번 스레드] 우선순위: 5
[1번 스레드] 우선순위: 2
[2번 스레드] 우선순위: 5
[3번 스레드] 우선순위: 8
현재 실행중인 스레드: main
Main 스레드의 우선순위: 5
Main 스레드의 우선순위: 10
[2번 스레드] run 메서드 내부
[3번 스레드] run 메서드 내부
[1번 스레드] run 메서드 내부
[1번 스레드] 1초
[3번 스레드] 1초
[2번 스레드] 1초
[2번 스레드] 2초
[1번 스레드] 2초
[3번 스레드] 2초
[3번 스레드] 3초
[1번 스레드] 3초
[2번 스레드] 3초
[3번 스레드] 4초
[2번 스레드] 4초
[1번 스레드] 4초
[2번 스레드] 5초
[1번 스레드] 5초
[3번 스레드] 5초
```

### 참고

[Geeks for Geeks-자바 멀티스레딩에서의 스레드 우선순위](https://www.geeksforgeeks.org/java-thread-priority-multithreading/)

## Main 스레드

멀티스레드 실행은 자바 플랫폼의 필수 기능입니다. 모든 애플리케이션에는 적어도 하나의 스레드가 있고 메모리 관리 및 신호 처리와 같은 시스템 스레드를 포함하면 여러개라고 볼 수도 있습니다.

애플리케이션 개발자의 관점에서 보면, 우리는 `main thread`라고 부르는 한 스레드로부터 시작합니다. 이 스레드는 추가적인 스레드를 만들 수 있는 능력이 있습니다.

또한 프로그램이 종료되기 전 마지막으로 종료되는 스레드여야 합니다.

각 자바 프로그램이 실행 될 때 JVM에 의해 Main 스레드가 생성되고, 이 Main 스레드는 `main()` 메서드의 존재를 확인하고 클래스를 초기화 합니다.

### Main Thread만 사용해서 Deadlock 상태 만들기(싱글 스레드에서만 동작)

우리는 Main thread만 사용해서 데드락을 만들 수 있습니다. 이는 싱글 스레드에서만 동작합니다.

```java
public class DeadlockMainThread {

  public static void main(String[] args) {
    try {
      System.out.println("Deadlock 만들기");

      Thread.currentThread().join();

      System.out.println("이 문장은 절대 실행되지 않을겁니다.");
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }
}
```

```text
Deadlock 만들기

```

이후엔 종료되지 않습니다.  
왜냐하면 `Thread.currentThread().join()`은 Main 스레드가 자기 자신이 종료되기 까지 기다린다는 것입니다. 따라서 Main 스레드는 자기 자신이 죽기까지 기다립니다. 이는 교착상태라고 볼 수 있습니다.

### 참고

- [오라클 자바 튜토리얼(프로세스와 스레드)](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)
- [Geeks for Geeks(자바 main 스레드)](https://www.geeksforgeeks.org/main-thread-java/)

## 동기화

동기화가 필요한 이유는 스레드 간의 커뮤니케이션을 할 때, 필드와 객체 참조를 공유하는데, 이러한 형태의 통신은 매우 효율적이지만, 스레드 간섭 및 메모리 일관성의 오류를 만들게 됩니다. 이러한 오류들을 방지하는 데 필요한 도구가 바로 동기화입니다.

그러나, 동기화로 인해서 스레드간의 경합이 발생할 수 있습니다. 스레드 경합은 두 개 이상의 스레드가 동시에 동일한 리소스에 액세스하려고 시도하는 것을 의미합니다. 스레드 경합에는 기아 현상 및 livelock이 있습니다.

### 스레드 간섭

우리가 흔히 작성하는 클래스를 예제로 들어보겠습니다.

```java
class Counter {
  private int c = 0;

  public void increment() {
    c++;
  }

  public void decrement() {
    c--;
  }

  public int value() {
    return c;
  }
}
```

`Counter`는 겉보기에 아무런 문제도 없는 클래스입니다. 싱글 스레드 환경에서는 아무런 문제 없는 클래스가 맞습니다만, 멀티 스레드 환경에서는 스레드 간의 간섭이 일어나게 됩니다. Counter 객체가 여러 스레드에서 사용되는 경우에는 예상대로 코드가 동작하지 않을 가능성이 있습니다.

서로 다른 스레드에서 실행되는 작업이 동일한 데이터를 가지고 동작할 떄 `interleave`되는 경우 간섭이 발생합니다. 이 의미는 두 작업이 여러 단계로 구성되고, 단계의 시퀀스가 겹쳐집니다. 쉽게 말해서 c가 어떤 시점에 다른 작업에 같은 값이 사용되고, 이에 대한 값이 덮어씌워지는 경우입니다.

단순하다고 해서 이런 문제가 발생되지 않는것은 아닙니다. `Counter`가 그 예입니다. 예를 들어서 `c++;`는 사실 다음과 같습니다.

1. `c`의 현재 값을 가져옵니다.
2. 가져온 값을 1 증가 시킵니다.
3. `c`에 증가된 값을 저장합니다.

`c--`도 마찬가지 입니다.

`ThreadA`와 `ThreadB`가 있다고 가정했을 때, 다음과 같은 시나리오를 상상해 볼 수 있습니다.

`c`의 초기 값은 0이라고 가정합니다. 다음과 같은 순서로 코드가 실행됩니다.

1. `ThreadA`: c 조회
2. `ThreadB`: c 조회
3. `ThreadA`: 조회된 값을 1 증가시킴
4. `ThreadB`: 조회된 값을 1 감소시킴
5. `ThreadA`: 증가된 값을 c에 저장: 현재 1
6. `ThreadB`: 감소된 값을 c에 저장: 현재 -1

최종적으로 `ThreadA`의 값은 `ThreadB`의 값에 의해 덮어씌워지고 잃어버리게 됩니다. 그리고 이런 경우는 드물게 나올 수 있기 떄문에 더더욱 찾기 힘들고, 버그를 발견하고 수정하기 어려울 수 있습니다. 특히 예측이 불가능하기 때문에 발견조차 못할 수 있습니다.

### 메모리 일관성 오류

메모리 일관성 오류는 서로 다른 스레드가 동일한 데이터에 대해 일관성이 없는 view를 가질 때 발생합니다. 이는 튜토리얼 수준에서 다루기엔 무거운 주제라고 합니다. 프로그래머는 이를 피하는 전략을 알아야 합니다.

메모리 일관성 오류를 피하는 방법은 `happens-before` 관계를 이해하는 것입니다. 이 관계는 단순히 특정한 명령문에 의한 메모리 쓰기가 다른 특정 명령문에 보인다는 것을 보장합니다.

```java
int counter = 0;

counter++; // A에서 실행합니다.

System.out.println(counter); // B에서 실행합니다.
```

위의 사례를 예로 들면 같은 스레드에서 두 명령문이 실행되었을 때는 "1"이라고 가정하는 것이 안전합니다. 그러나 다른 스레드에서 실행된다면 값은 "0"으로 출력될 수도 있습니다. 왜냐하면 A에서 변경된 `counter`의 값이 `B`에서는 보인다는 것을 (프로그래머가 둘 사이에 사전에 관계를 설정해주지 않는 이상) 보장해주지는 않기 떄문입니다.

이를 보장해주는 작업이 몇 개 있습니다. 그 중 하나는 동기화입니다.

- 명령문에서 `Thread.start`를 호출하면 `happens-before`를 맺게됩니다.
- 스레드가 종료되고 `Thread.join`이 다른 스레드에 반환되면 이 역시 `join` 문 이후의 코드와 `happens-before`를 맺게됩니다.

참고로 `happens-before` 관계를 만드는 방법의 종류가 궁금하다면, [이 곳](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html#MemoryVisibility)을 참고하세요.

### 동기화된 메소드

자바에서는 두가지 동기화 관용구를 제공합니다. `synchronized` 메서드와, `synchronized` 문입니다. 이 중 더 복잡한 것이 문입니다.

메서드를 동기화하려면 메서드 선언 부분에 `synchronized` 키워드를 추가하면 됩니다.

```java
class SynchronizedCounter {
  private int c = 0;

  public synchronized void increment() {
    c++;
  }

  public synchronized  void decrement() {
    c--;
  }

  public synchronized int value() {
    return c;
  }
}
```

`count`가 `SynchronizedCounter`의 인스턴스라면 `synchronized`는 두가지 효과를 가지게 됩니다.

1. 동일한 객체에서 동기화된 메서드를 두 번 호출하면 인터리브 할 수 없습니다. 완료 될 때까지 기다렸다가 다시 실행하게 됩니다.
2. 동기화된 메서드가 종료되면 동일한 객체에 대한 동기화된 메서드의 후속 호출과 함께 `happens-before` 관계를 자동으로 설정합니다. 이는 객체의 변경사항이 모든 스레드에 보이는 것을 보장합니다.

생성자는 동기화 키워드를 사용할 수 있습니다. 생각해보면 말이 안되는 것이 객체를 생성하는 쪽에서만 접근할 수 있기 떄문입니다.

**주의할 점**: 우리가 생성자에서 `this`를 사용하고 싶을 때가 있을겁니다. 이런 참조는 어떻게 보면 다른 스레드에서 아직 미처 생성되지조차 못한 객체를 참조할 수 있다는 의미입니다. 많은 스레드에서 공유되어 사용되는 것이라면, 심사숙고 후에 결정하기를 권장합니다.(그냥 안하는게 나을 수도 있습니다.)

동기화 된 메서드를 사용하면 스레드 간섭이나 메모리 일관성 오류를 방지하기 위한 간단한 전략을 사용할 수 있습니다. 무조건 동기화된 메서드를 거치게 해서 해당 문제를 예방합니다. 참고로 `final` 필드의 경우에는 동기화 되지 않아도 상관 없습니다. 변경 불가기 때문에 안전하기 떄문입니다. 이 전략(동기화된 메서드)은 효과적이지만 문제가 있을 수 있습니다.(데드락과 같은)

### 고유잠금 및 동기화

동기화는 고유 잠금 또는 모니터 잠금이라고 알려진 내부 엔티티를 중심으로 구축됩니다.

모든 객체는 연결된 고유 잠금이 있으며, 관례적으로 객체의 필드에 대한 배타적이고 일관된 액세스가 필요한 스레드는 객체에 액세스하기 전에 객체의 고유 잠금을 획득한 다음 작업이 완료되면 고유 잠금을 해제해야 합니다. 이를 획득한 경우를 소유한다고 말합니다. 한 스레드가 고유 잠금을 소유하고 있는 동안에 다른 스레드는 고유 잠금을 획득할 수 없습니다.

동기화된 메서드는 메서드의 객체에 대해 고유 잠금을 자동으로 획득하고 메서드가 반환되거나 예외가 발생해도 잠금 해제가 됩니다.

동기화된 문(statement)은 고유잠금을 사용하는 객체를 지정해주어야 합니다.

```java
public void addName(String name) {
  synchronized(this) {
    lastName = name;
    nameCount++;
  }
  nameList.add(name);
}
```

이 예제에서는 `addName` 메서드는 `lastName`, `nameCount`에 대해 동기화 하면서, 다른 객체의 메서드 호출은 동기화 하지 않도록 했습니다. 이를 통해서 데드락을 피할 수 있습니다.

재진입 동기화는 같은 스레드의 메서드가 이미 소유하고 있는 잠금을 다시 요청했을 때, 잠금을 그냥 획득하도록 하는 것입니다. 이를 통해서 자체 스레드간의 예방조치는 해주지 않아도 됩니다.

### 참고

- [오라클 자바 튜토리얼(동기화)](https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html)
- [오라클 자바 튜토리얼(스레드 간섭)](https://docs.oracle.com/javase/tutorial/essential/concurrency/interfere.html)
- [오라클 자바 튜토리얼(메모리 일관성 오류)](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)
- [오라클 자바 튜토리얼(동기화된 메서드)](https://docs.oracle.com/javase/tutorial/essential/concurrency/syncmeth.html)
- [오라클 자바 튜토리얼(고유잠금 및 동기화)](https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html)

## 데드락

데드락은 두 개 이상의 스레드가 계속해서 서로를 기다리며 block 되어 있는 상황을 의미합니다.

철수와 영희는 친구입니다. 그리고 아주 예의바른 사람들입니다. 이 예의의 엄격한 규칙은 친구에게 인사를 할 때, 친구가 인사를 할 때 까지 계속 인사를 해야한다는 것입니다. 불행하게도, 이 규칙은 두 명의 친구가 서로에게 동시에 인사하는 경우에 대한 가능성을 설명해주지는 않습니다.

다음 예제 애플리케이션 `Deadlock`은 이 가능성에 대해서 프로그래밍합니다.

```java
public class Deadlock {
  static class Friend {
    private final String name;

    public Friend(String name) {
      this.name = name;
    }

    public String getName() {
      return this.name;
    }

    public synchronized void hello(Friend friend) {
      System.out.format("%s: %s야 안녕!%n", this.name, friend.getName());
      friend.helloBack(this);
    }

    public synchronized void helloBack(Friend friend) {
      System.out.format("%s: %s야 안녕!!!%n", this.name, friend.getName());
    }
  }

  public static void main(String[] args) {
    final Friend 철수 = new Friend("철수");
    final Friend 영희 = new Friend("영희");

    new Thread(() -> 철수.hello(영희)).start();
    new Thread(() -> 영희.hello(철수)).start();
  }
}
```

`Deadlock`을 실행하면, `helloBack`을 호출하려고 할 때, 두 스레드가 모두 block될 가능성이 매우 높습니다. 각 스레드는 다른 스레드의 `hello`가 끝나길 기다리고 있기 때문에 끝나지 않습니다.

### 참고

- [오라클 자바 튜토리얼(데드락)](https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html)

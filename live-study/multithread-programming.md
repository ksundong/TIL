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

## 스레드의 상태

## 스레드의 우선순위

## Main 스레드

멀티스레드 실행은 자바 플랫폼의 필수 기능입니다. 모든 애플리케이션에는 적어도 하나의 스레드가 있고 메모리 관리 및 신호 처리와 같은 시스템 스레드를 포함하면 여러개라고 볼 수도 있습니다.

애플리케이션 개발자의 관점에서 보면, 우리는 `main thread`라고 부르는 한 스레드로부터 시작합니다. 이 스레드는 추가적인 스레드를 만들 수 있는 능력이 있습니다.

### 참고

[오라클 자바 튜토리얼(프로세스와 스레드)](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)

## 동기화

## 데드락

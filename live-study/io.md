# 13주차: I/O

## 학습할 것

- [스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O](#스트림-stream--버퍼-buffer--채널-channel-기반의-io)
- [InputStream과 OutputStream](#inputstream과-outputstream)
- [Byte와 Character 스트림](#byte와-character-스트림)
- [표준 스트림 (System.in, System.out, System.err)](#표준-스트림-systemin-systemout-systemerr)
- [파일 읽고 쓰기](#파일-읽고-쓰기)

## 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O

스트림과 버퍼는 뒤에서 설명할 것입니다.

### 채널 기반의 I/O

채널이라는 이름에서 알 수 있듯이 채널은 한 쪽에서 다른 쪽으로의 데이터 흐름을 의미합니다. 자바의 NIO에서 채널기반의 io를 제공합니다. 이 채널은 버퍼로 데이터를 읽고 버퍼에서 데이터를 쓰는데 사용됩니다.

채널 기반의 IO는 기존의 자바 IO에서 사용되던 스트림과 달리, 읽기 쓰기가 모두 가능한 양방향입니다. Java의 NIO 채널은 블로킹, 넌블로킹 모드에서 모두 데이터의 비동기 흐름을 지원합니다.

#### Channel의 구현체

Java NIO에서의 채널은 다음의 클래스로 구현되었습니다.

- `FileChannel` - 파일에서 데이터를 읽기 위해 파일 채널을 사용합니다. 파일 채널의 객체는 파일 객체를 직접 생성할 수 업습니다. 따라서 파일 객체에서 `getChannel()` 메서드를 호출해서 생성할 수 있습니다.
- `DatagramChannel` - 데이터그램 채널은 UDP(User Datagram Protocol)를 통해 네트워크를 통한 데이터 읽고쓰기를 할 수 있습니다. 데이터그램 채널의 객체는 팩토리 메서드 방식으로 생성할 수 있습니다.
- `SocketChannel` - 소켓 채널은 TCP(Transmission Contorol Protocol)를 통해 네트워크를 통한 데이터 읽고쓰기를 할 수 있습니다. 이 역시 팩토리 메서드를 사용해 새로운 객체를 생성해줄 수 있습니다.
- `ServerSocketChannel` - 서버 소켓 채널은 웹 서버와 동일하게 TCP 연결을 통해 데이터를 읽고 씁니다. 들어오는 모든 연결에 대해 소켓 채널이 생성됩니다.

### 채널과 스트림의 차이

- 채널은 읽고 쓰기가 모두 가능한 양방향이지만, 스트림은 단방향으로만 가능합니다.
- 채널은 비동기를 지원합니다.
- 채널은 항상 버퍼를 이용하지만 스트림은 래핑을 해주어야 합니다.

### 참고

- [tutorials point(Java NIO - Channels)](https://www.tutorialspoint.com/java_nio/java_nio_channels.htm)

## InputStream과 OutputStream

Java에서는 입력 출력을 Stream의 형태로 표현할 수 있습니다. 입력 소스를 담당하는 `InputStream`과 출력을 나타내는 `OutputStream`으로 나눌 수 있습니다.

`java.io` 패키지에 속한 Stream들은 기본형, 객체, 문자등을 지원합니다.

Stream은 데이터의 시퀀스라고 정의할 수 있습니다. 다음의 두 Stream으로 분류할 수 있습니다.

- `InputStream`은 소스로부터 데이터를 읽어들이는 용도로 사용합니다.
- `OutputStream`은 어떤 대상에 데이터를 쓰는 용도로 사용합니다.

Java는 파일 뿐만 아니라 네트워크 I/O에 대해서도 강력하고 유연한 기능을 제공합니다.  
모든 Stream들은 위의 두 Stream을 상속합니다.

### 참고

- [오라클 자바 튜토리얼(바이트 스트림)](https://docs.oracle.com/javase/tutorial/essential/io/bytestreams.html)
- [튜토리얼스 포인트(자바 파일과 I/O)](https://www.tutorialspoint.com/java/java_files_io.htm)

## Byte와 Character 스트림

자바에는 많은 byte stream 클래스들이 있습니다. 튜토리얼에서는 byte stream의 동작원리를 알기 위해 file I/O byte stream에 초점을 맞추겠습니다. 해당 클래스는 `FileInputStream`과 `FileOutputStream`이 있습니다. 다른 종류의 byte stream들은 목적이 다르기 때문에 구성 방식이 다를뿐이고, 사용하는 방식은 동일합니다.

일단 예제를 통해서 한 번 학습해봅시다. 이 예제는 byte stream을 이용해 파일에 작성된 내용을 한 번에 한 바이트씩 복사하는 애플리케이션입니다.

먼저 `input.txt` 파일과 `output.txt` 파일을 준비해주세요. 이 예제는 gradle 프로젝트로 구성되어 프로젝트 구조를 gradle 구조를 따라갑니다.

해당 소스는 [여기](https://github.com/ksundong/java-io-example)에서 보실 수 있고, [vscode](https://github1s.com/ksundong/java-io-example)로도 보실 수 있습니다.

```text
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   ├── java
    │   │   └── dev
    │   │       └── idion
    │   │           └── ioexample
    │   │               └── CopyBytes.java
    │   └── resources
    │       ├── input.txt
    │       └── output.txt
    └── test
        ├── java
        └── resources

```

`input.txt`

```text
hello, nice to meet you!
```

`output.txt`

```text
```

`CopyBytes.java`

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyBytes {

  public static void main(String[] args) throws IOException {
    try (FileInputStream in = new FileInputStream("src/main/resources/input.txt");
        FileOutputStream out = new FileOutputStream("src/main/resources/output.txt")) {
      int c;

      while ((c = in.read()) != -1) { // -1은 EOF에 도달하면 출력됩니다.
        out.write(c);
      }
    }
  }
}
```

해당 코드를 build하거나 실행시켜보면 `input.txt`의 내용이 `output.txt`로 복사됨을 알 수 있습니다.

`CopyBytes`는 입력 스트림을 읽으면서 한 번에 한 바이트씩 출력 스트림에 해당 내용을 쓰는 간단한 루프에서 대부분의 시간을 사용합니다.

그리고, 해당코드에는 try-with-resources를 사용하여 항상 스트림이 닫히도록 해주었습니다. **이런 I/O를 사용하는 등의 자원을 사용할 때에는 항상 닫아주는 습관을 들여야 합니다. 왜냐하면 이렇게 해주지 않을 경우 심각한 리소스 누수를 발생시킬 수 있기 때문입니다.**

오류가 발생하는 포인트는 두 가지로 `CopyBytes`가 `input.txt` 또는 `output.txt` 파일을 열 수 없을 때 발생합니다.

### Byte Stream을 사용하면 안되는 경우

`CopyBytes`는 일반적인 프로그램처럼 보이지만, 실제로는 우리가 피해야하는 저수준의 I/O의 한 종류입니다. 이런 문자열 타입을 처리할 때에는 뒤에서 설명할 `CharacterSteram`를 사용하는 것이 좋습니다. 또한 이보다 더 복잡한 데이터 타입에 대한 stream도 있습니다. Byte stream은 가장 기본적인 I/O에만 사용해야 합니다.

### Character Stream

자바는 유니코드를 사용하여 문자 값을 저장합니다. Character stream I/O는 자동으로 이 내부 포맷을 로컬 문자 집합으로 자동으로 변환합니다. 서양 locale에서는 로컬 문자셋은 일반적으로 ASCII의 8비트 슈퍼셋입니다.

대부분의 애플리케이션에서 character stream의 I/O는 byte steram의 I/O보다 복잡하지 않습니다. 스트림 클래스로 수행된 입출력은 로컬 문자 집합으로 자동으로 변환됩니다. byte stream 대신 charater stream을 사용하는 프로그램은 프로그래머의 추가적인 노력없이 로컬 문자 집합에 자동으로 연결되고, 국제화될 준비까지 되어있습니다.

국제화가 우선 순위가 아닌경우 문자 집합 문제에는 많은 관심을 주지 않고 그냥 chrater stream을 사용하면 됩니다. 나중에 국제화가 우선시 될 때 광범위한 레코딩 없이 프로그램은 연결될 수 있습니다.

모든 charater stream 클래스는 `Reader`와 `Writer` 를 상속합니다. byte stream과 마찬가지로 파일 I/O에 특화된 chracter stream 클래스인 `FileReader`와 `FileWirter`가 있습니다.

위의 `CopyBytes`를 조금 바꾼 `CopyCharaters`를 보고 알아봅시다.

```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class CopyCharacters {

  public static void main(String[] args) throws IOException {
    try (FileReader in = new FileReader("src/main/resources/input.txt");
        FileWriter out = new FileWriter("src/main/resources/output.txt")) {
      int c;

      while ((c = in.read()) != -1) { // -1은 EOF에 도달하면 출력됩니다.
        out.write(c);
      }
    }
  }
}
```

위의 코드는 거의 바뀌지 않았습니다. 단순히 클래스만 바뀌었죠. 하지만 동작은 다릅니다. `CopyCharacters`에서 `int` 변수는 마지막 16비트 문자 값을 보유하지만, `CopyBytes`에서 `int` 변수는 마지막 8비트 바이트 값을 가집니다.

#### character stream은 byte stream을 사용합니다.

character stream은 종종 byte stream의 Wrapper로 사용됩니다. character stream이 물리적 I/O를 수행하기 위해 byte stream을 사용합니다. character stream은 문자와 바이트의 변환을 처리합니다. `FileReader`는 `FileInputStream`을 사용하고, `FileWriter`는 `FileOutputStream`을 사용합니다.

범용으로 사용하는 byte-to-character 브릿지 스트림은 `InputStreamReader`와 `OutputStreamWriter`가 있습니다.  
요구 사항을 충족하는 미리 제공되는 character stream 클래스가 없다면 이를 사용해서 만들면 됩니다.

#### 라인 기반 I/O

character I/O는 일반적으로 단일 문자보다 더 큰 단위로 발생합니다. 보통 하나의 공통 단위는 라인입니다. 라인 끝에 라인 종료문자가 있는 문자열입니다. 라인 종료문자는 (`\r\n`, `\r`, `\n`) 일 수 있습니다. 가능하면 모든 라인 종료문자를 지원하면, 널리 사용되는 운영체제에서 생성된 텍스트 파일을 읽을 수 있습니다.

우리가 위에 작성했던 프로그램을 좀 더 개선해봅시다. 라인 기반 I/O로 수정할 것입니다. 이를 위해서 우리는 `BufferedReader`와 `PrintWriter`를 사용할 것입니다.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class CopyLines {

  public static void main(String[] args) throws IOException {
    try (BufferedReader in = new BufferedReader(new FileReader("src/main/resources/input.txt"));
        PrintWriter out = new PrintWriter(new FileWriter("src/main/resources/output.txt"))) {
      String line;

      while ((line = in.readLine()) != null) {
        out.println(line);
      }
    }
  }
}
```

`readLine()`을 호출하면 해당 라인 전체가 반환됩니다. 또, `CopyLines`는 현재 사용하는 운영체제의 라인 종료문자를 추가하는 `println()`을 사용하여 각 라인을 출력합니다. 이는 입력 파일에서 사용된 종료문자와 다른 문자일 수 있습니다.

그리고 이를 뛰어넘는 방법이 텍스트 입력과 출력 자체를 구조화 하는 방법입니다. 이는 Scanning과 Formatting으로 구분할 수 있습니다.

#### 궁금했던 점

`new FileReader()`나 `new FileWriter()`를 선언했는데 `close()`가 되는 것인가? 튜토리얼의 예제코드에서는 `BufferdReader`와 `PrintWriter`만 `close()` 해줬기 때문에 내부에 감싸진 스트림도 `close()` 해줘야 하지 않을까?

`BufferedReader`, `PrintWriter` 구현체의 `close()` 메서드를 자세히 살펴보면, 내부에 감싸고 있는 스트림또한 `close()`를 해주고 있습니다.

```java
public void close() throws IOException {
    synchronized (lock) {
        if (in == null)
            return;
        try {
            in.close();
        } finally {
            in = null;
            cb = null;
        }
    }
}
```

### 참고

- [오라클 자바 튜토리얼(바이트 스트림)](https://docs.oracle.com/javase/tutorial/essential/io/bytestreams)
- [오라클 자바 튜토리얼(캐릭터 스트림)](https://docs.oracle.com/javase/tutorial/essential/io/charstreams.html)
- [스택 오버플로우(둘다 close?](https://stackoverflow.com/questions/1388602/do-i-need-to-close-both-filereader-and-bufferedreader)

## 버퍼 스트림

우리가 지금까지 사용한 대부분의 스트림은 Buffer되지 않은 I/O를 사용합니다. 이 Buffer되지 않았다의 의미는 읽기 및 쓰기 요청이 OS단에 바로 전달되어 처리됨을 의미합니다. 이러한 각 요청은 종종 디스크 액세스, 네트워크 활동, 또는 상대적으로 비용이 많이드는 다른 작업을 트리거 하기 때문에 프로그램의 효율성을 떨어뜨리는 원인이 될 수 있습니다.

위에서 설명한 종류의 오버헤드를 줄이기 위해서 Java에서는 Buffer된 I/O 스트림을 구현했습니다. 버퍼된 입력 스트림은 Buffer로 알려진 메모리 영역에서 데이터를 읽습니다. native 입력 API는 버퍼가 비어있을 때에만 호출됩니다. 이와 비슷하게, Buffer된 출력 스트림은 버퍼에 데이터를 쓰고, 버퍼가 가특찬 경우에만 native 출력 API가 호출됩니다.

프로그램은 버퍼되지 않은 스트림 객체가 버퍼된 스트림 클래스의 생성자에 전달되는 우리가 몇 번 사용한 래핑 관용구를 사용하여 버퍼되지 않은 스트림을 버퍼된 스트림으로 변환할 수 있습니다.

```java
BufferedReader in = new BufferedReader(new FileReader("src/main/resources/input.txt"));
BufferedWriter out = new BufferedWriter(new FileWriter("src/main/resources/output.txt"));
```

이러한 버퍼된 스트림 클래스들은 네가지가 있습니다. `BufferedInputStream`, `BufferedOutputStream`은 버퍼된 바이트 스트림을 만들고, `BufferedReader`와 `BufferedWriter`는 버퍼된 문자 스트림을 만듭니다.

### 버퍼된 스트림 플러시(flush)

우리는 버퍼가 채워질 때까지 기다리지 않고, 중요한 지점에서 버퍼에 저장된 데이터를 쓰는 것이 좋습니다. 이를 버퍼 플러시라고 합니다.

일부 버퍼된 출력 클래스는 `autoflush`를 지원합니다. 이는 생성자의 매개변수를 통해 선택적으로 정의될 수 있습니다. `autoflush`가 활성화되면, 특정 키 이벤트는 버퍼가 플러시되도록 합니다. 예를 들어, `PrintWriter` 객체는 자동 플러시가 활성화 되었을 때, `println` 또는 `format`을 호출할 때마다 버퍼를 플러시합니다.

스트림을 수동으로 플러시하기 위해서는 `flush` 메서드를 호출하세요. `flush` 메서드는 모든 출력 스트림에서 사용가능하지만, 버퍼되지 않은 스트림에서는 아무런 효과가 없습니다.

### 참고

- [오라클 튜토리얼(버퍼된 스트림)](https://docs.oracle.com/javase/tutorial/essential/io/buffers.html)

## 표준 스트림 (System.in, System.out, System.err)


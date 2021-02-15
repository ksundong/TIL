# 13주차: I/O

## 학습할 것

- [스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O](#스트림-stream--버퍼-buffer--채널-channel-기반의-io)
- [InputStream과 OutputStream](#inputstream과-outputstream)
- [Byte와 Character 스트림](#byte와-character-스트림)
- [표준 스트림 (System.in, System.out, System.err)](#표준-스트림-systemin-systemout-systemerr)
- [파일 읽고 쓰기](#파일-읽고-쓰기)

## 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O

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

### 참고

- [오라클 자바 튜토리얼(바이트 스트림)](https://docs.oracle.com/javase/tutorial/essential/io/bytestreams)

## 표준 스트림 (System.in, System.out, System.err)

## 파일 읽고 쓰기

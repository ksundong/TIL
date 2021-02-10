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
-`OutputStream`은 어떤 대상에 데이터를 쓰는 용도로 사용합니다.

Java는 파일 뿐만 아니라 네트워크 I/O에 대해서도 강력하고 유연한 기능을 제공합니다.  
모든 Stream들은 위의 두 Stream을 상속합니다.

## Byte와 Character 스트림

## 표준 스트림 (System.in, System.out, System.err)

## 파일 읽고 쓰기

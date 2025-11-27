# JVM 기본 GC방식 확인하는 방법

1. jcmd를 사용하는 방법
    ```shell
    $ jcmd (PID) VM.flags
    (PID):
    -XX:CICompilerCount=2 -XX:InitialHeapSize=16777216 -XX:MaxHeapSize=257949696 -XX:MaxNewSize=85983232 -XX:MinHeapDeltaBytes=196608 -XX:NewSize=5570560 -XX:OldSize=11206656 -XX:+UseCompressedClassPointers -XX:+UseCompressedOops
    ```

    `-XX:Use*GC` 라는 플래그가 없다면 SerialGC를 사용합니다.

2. jmap을 사용하는 방법
    ```shell
    $ jmap -heap (PID)
    ```
    
    시도해봤는데 오류가나서 당황했습니다.

## References

https://stackoverflow.com/questions/54862460/how-to-determine-which-gc-i-use

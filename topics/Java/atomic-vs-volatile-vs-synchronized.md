# atomic vs volatile vs synchronized 차이

동기화를 할 때 사용한다는 공통점

- 경쟁 상태(race condition): 여러 스레드에서 같은 시점에 변수를 읽는 상태
- 변수의 가시성(visibility): 변수들이 사용될 수 있는 영역의 범위

volatile은 변수에 대한 동기화 처리를 해줌  
읽고 쓰는 연산을 메인 메모리에서만 처리한다. 따라서 가시성 문제가 해결된다.  
하지만 경쟁상태를 해결할 수는 없다. (동시에 변수를 읽을 수는 있다.)

syncronized는 해당 블록에 대한 동기화 처리를 해줌.  
여러 스레드에서 읽기 쓰기 모두 이용할 수 있다.  
하나의 스레드가 lock을 얻어 수행이 끝날때까지 다른 스레드들은 lock이 반환될 때까지 대기한다.

Atomic은 CAS(compare-and-swap) 기반으로 되어있다.  
CAS란 특정 메모리 위치의 값이 주어진 값을 비교하여 같으면 새로운 값으로 대체된다.  
여러 스레드에서 읽기 쓰기 모두 이용할 수 있다.

## References

[마이구미님 블로그](https://mygumi.tistory.com/112)

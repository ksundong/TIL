# Java Random 클래스와 SecureRandom 클래스의 차이

검색 키워드: java difference with Random and secureRandom

일단 Java Random 클래스는 java.util 패키지에 속하는 클래스이고, SecureRandom 클래스는 java.security 패키지에 속하는 클래스입니다.

## Java Random 클래스

암호학적으로 강력하지않은 난수를 정의합니다. 그리고, 난수는 완전한 난수가 아닙니다. 왜냐하면 수학적인 알고리즘을 사용하기 때문입니다.  
여기서 사용된 알고리즘은 Donald E. Knuth의 subtractive random number generator 알고리즘입니다.

따라서, 높은 정도의 보안을 필요로 하는 경우 이 클래스를 사용해서는 안됩니다. (예: 랜덤 비밀번호 생성)

## Java SecureRandom 클래스

암호학적으로 강력한 난수를 생성해서 제공합니다. SecureRandom 클래스는 non-deterministic한 출력을 반드시 제공해야합니다.  
따라서 SecureRandom 객체에 전달되는 시드는 예측 불가능하고, 모든 SecureRandom 출력 시퀀스는 암호학적으로 강력해야합니다.

## Random vs SecureRandom

1. 크기: Random 클래스는 48비트를 사용합니다. 하지만 SecureRandom 클래스는 최대 128 비트를 사용할 수 있습니다.
   이로인해 SecureRandom 클래스는 겹치지 않는 난수를 생성할 확률이 높아지게 됩니다.
2. 시드 생성: Random 클래스는 시스템 시계를 시드로 사용하거나, 시드를 생성하기 위해 사용합니다.
   그래서 공격자가 시드가 생성된 시간을 안다면, 재생성되기 쉽습니다.
   SecureRandom은 OS에서 랜덤 데이터(키 입력의 간격 같은 데이터, 대부분의 OS는 이를 수집하여 파일로 저장합니다.)를 시드로 사용합니다.
3. 코드를 크래킹할 경우: Random의 경우 2^48 번의 시도가 필요합니다. 이는 오늘날의 CPU로 충분히 깨는 것이 가능합니다.
   그러나, SecureRandom은 최대 2^128 번의 시도가 필요하며, 이는 오늘날의 CPU로도 수 년이 걸릴 것임을 짐작할 수 있습니다.
4. 구현: Oracle JDK 7의 구현에서는 Random 클래스에 Linear Congruential Generator라는 것을 사용합니다.
   반면 Secure Random에서는 SHA1을 사용하여 의사난수를 생성하는 SHA1PRNG 알고리즘으로 구현되었습니다.
5. 보안: 결과적으로 Random 클래스는 보안이 중요한 애플리케이션이나 민감데이터를 보호하는데에는 절대로 사용해선 안됩니다.

## References

- [GeeksforGeeks](https://www.geeksforgeeks.org/random-vs-secure-random-numbers-java)

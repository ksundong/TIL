# 코드리뷰를 통해 배우는 파이썬

1. inline if else의 사용  
   파이썬에서는 `condition ? a : b` 와같은 삼항 연산자 대신에 `a if condition else b` 와 같은 형태로 쓸 수 있다.
2. list의 `pop(0)` 연산은 O(n)  
   파이썬 List는 다양한 연산을 지원하지만 기본적으로 가변 배열 기반이므로 `pop(0)` 연산이 O(n)의 시간복잡도를 가진다.  
   따라서 큐의 형태로 쓰인다면 `deque` 모듈을 사용하여 `popleft()` 연산을 사용하는 편이 O(1)의 시간복잡도를 가져 더 좋은 성능을 낼 수 있다.
3. 무한 혹은 경계 값이 필요할 경우  
   `sys` 모듈의 `maxsize`를 사용하거나, 코딩테스트에서 `sys` 모듈을 사용할 수 없다면, `float('inf')` 를 활용할 수 있다.
4. PEP8 규칙을 지키자  
   https://www.python.org/dev/peps/pep-0008
5. 파이썬의 문자열 슬라이싱은 범위를 벗어나면 빈 문자열을 반환한다.

   ```python3
   >>> s = 'hello world! this is python3'
   >>> print(s[len(s):999])

   >>> type(s[len(s):999])
   <class 'str'>
   >>> len(s[len(s):999])
   0
   ```

6. 마지막 인덱스에 접근할 때에는 `len() - 1` 보다는 `-1`로 접근한다.
7. `zip()` 을 활용해서 iterable한 것을 묶어서 사용할 수 있다.
8. 전역변수를 사용하기 보단 nested function(중첩 함수)를 사용하자(코드가 지저분해지지 않는 선에서)
9. 파이썬에서 재귀 호출은 매우 느리고 꼬리재귀도 지원이 되지 않기 때문에 stack을 활용해서 재귀 구조를 풀어놓는게 좋다.
10. `extend()`의 활용  
    `extend()`는 내부의 iterable을 값만 추가해주는 함수다.
    ```python3
    dx = [0, 1, 0, -1]
    dy = [1, 0, -1, 0]
    ...
    q.extend([(a + x, b + y) for x, y in zip(dx, dy)])
    ```
    위와 같은 형태로 활용할 수 있다.
    https://m.blog.naver.com/PostView.nhn?blogId=wideeyed&logNo=221541104629&categoryNo=50
11. 파이썬은 `try ~ except` 를 통해 예외처리를 한다.  
    Java처럼 사전에 예외조건을 캐치하고 미리 방지하는 방어적인 방식이 아니다.
    https://suwoni-codelab.com/python%20%EA%B8%B0%EB%B3%B8/2018/03/06/Python-Basic-EAFP/
12. `heappop()` 후 `heappush()` 를 한다면 `heapreplace()` 사용을 고려해보자.  
    로직도 간단해지고 보기 좋아진다.
13. 좀 더 파이써닉하게 코드 짜는 법  
    `for ~ else` 문법, `list comprehension` 같은 파이썬에서만 존재하는 문법을 가능하면 적극적으로 사용하도록 노력한다.
14. python `for ~ else` 문법  
    https://itholic.github.io/python-for-else/  
    `else` 아래는 for 문이 끊김 없이 다 돌았을 때 동작함
    > for ~ else문은 “for문에서 break가 발생하지 않았을 경우”의 동작을 else문에 적어주는 것이다.
15. N x M 크기의 2차원 리스트 초기화 방법
    ```python3
    n = 3
    m = 3
    array = [[0] * m for _ in range(n)]
    ```
16. 문자열 padding 하는 법(0말고 다른 문자도 가능한 방법도 존재)
    1. `str.zfill(size)` 함수 사용(0으로 크기 벗어난 값 채움)
    2. `str.rjust(size, fill_char)` 함수 사용(`fill_char`로 채움)
       `fill_char` 의 길이는 1글자로 제한됨
17. `format()`의 활용
    - 이진수 변환: `format(number, 'b')`
    - zero padding 이진수 변환: `format(number, '0' + str(size) + 'b')`
    - `0b` prefix 이진수 변환: `format(number, '#0' + str(size) + 'b')`
      주의: 0b도 자릿수에 포함됨.
18. 진법 변환시 주의할 점
    진법 변환 후 prefix가 붙어서 보통 `bin(number)[2:]` 와 같은 형태로 prefix를 제거함

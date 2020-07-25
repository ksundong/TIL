# 파이썬 리스트 메소드들

```python
a = [1, 2, 3, 3]

print(len(a))  # 전체 요소의 개수를 리턴한다. O(1)
print(a[0])  # 인덱스 i의 요소를 가져온다. O(1)
print(a[1:2])  # i부터 j까지 슬라이스 길이 k만큼의 요소를 가져온다. O(k)R
print(2 in a)  # elem요소가 존재하는지 확인한다. 순차탐색이므로 O(n)
print(a.count(2))  # elem 요소의 개수를 리턴한다 O(n)
print(a.index(3))  # elem 요소의 인덱스를 리턴한다. O(n)
a.append(4)  # 리스트 마지막에 elem 요소를 추가한다. O(1)
print(a.pop())  # 리스트 마지막 요소를 추출한다. O(1)
print(a.pop(0))  # 리스트 첫번째 요소를 추출한다. 큐의 연산이고, 자주 사용한다면 데크 자료구조를 사용하자.
del a[0]  # 인덱스 i의 요소를 제거한다. O(n)
a.sort()  # 정렬한다. 팀소트를 사용한다. O(nlogn)
print(min(a))  # 최솟값 계산 O(n) 선형 탐색
print(max(a))  # 최댓값 계산 O(n) 선형 탐색
a.reverse()  # 뒤집는다. O(n)
a.insert(3, 5)  # i위치에 x 값을 삽입한다. 이 때, 리스트의 크기보다 큰 index의 경우 마지막에 넣음
print(a)
print(a[2])  # a[i]의 형식으로 값을 꺼내올 수 있다.
print(a[:2])  # 시작 인덱스는 생략 가능하다.
print(a[1:])  # 종료 인덱스도 생략 가능하다.

a = [1, 2, 3, 5, 4, '안녕', True]
print(a[1:4:2])  # 맨 마지막은 단계(step)로 몇 칸씩 건너뛸지 지정함

# 존재하지 않는 인덱스를 조회하려고 할 경우 IndexError 발생
try:
    print(a[9])
except IndexError:
    print('존재하지 않는 인덱스')
# 위와 같이 에러처리 가능

# del은 인덱스의 위치에 있는 요소 삭제
# remove는 값에 해당하는 요소 삭제
```


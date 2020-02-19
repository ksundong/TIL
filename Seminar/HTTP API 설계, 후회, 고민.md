# HTTP API 설계, 후회, 고민

이경환 님

1. 설계의 시작
2. v1 에서 v2 로
3. 후회와 고민

## 1. 설계의 시작

Rest API Design Rule book(목차가 좋음, 4~**5**장이 중요)

POST GET PUT DELETE를 제 용도로 쓰자

Task가 Project의 하위항목이라고 생각했는데, 아니었음

Call을 한 번 하면 모든 데이터가 다나오는 상황(화면 그리긴 좋음)

지금이라면 link를 내보낼 것 같음

URL은 하나 Representation은 둘

Content Negotiation Header를 사용하는 것이 표준을 따르는 방식

Accept: ~~~

REST API를 정의할 때, 여러개 resource를 한 번에 변경하는 Case는 논의가 필요함.

move를 정의해서 사용(없는 경우 생성해서 사용가능)

수정시: URL 변경정보를 내부에 저장 or URL 이 변경되지 않도록 하기

메일 읽음표시: 표준상 GET(safe-method) 후 PUT을 권장

PUT(partial update X), PATCH(partial update O)

## 2. v1에서 v2 로



## 3. 후회와 고민

Dynamic Content는 Cache를 안하는데, 자체가 느린경우가 존재(때문에 HTTP Cache를 고민함)

필드별로 권한이 다른 경우

여러 리소스가 한 번에 만들어져야 하는 경우

### 미리 잘 정해두면 좋은 것

리소스에 어울리는 단어들: 잘 정의되지 않으면 혼용이 되는 경우가 있음

flag성 이름: 규칙을 정하자 is, has, can, ...

날짜, 날짜 시간 필드명(At, On, DateTime, Date, 미래형), 날짜 포맷(ISO8601, UNIX Epoch Time)

Query Param도 미리 정의해두는게 좋음.

## 4. Q&A

Q. 예제를 보니 GET은 몰라도 POST나 PUT으로 일괄처리하는경우가 있던데, API가 중간에 실패할 수 있는데 상태 관리를 어떻게 하는지?

A. API를 제공해야하는데, 이렇게했다~라고 예제를 제공한 것 실제로는 그렇게 처리하지 않았음.
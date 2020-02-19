# Spring JPA의 사실과 오해

신동민 님

1. Spring JPA의 사실과 오해
2. 연관 관계 맵핑에 대한 모든 것
3. Spring Data JPA Repository의 숨겨진(?) 기능

Data Access Layer를 사용할 때 유의해야 할 것

## 1. Spring JPA의 사실과 오해

소프트웨어 공학의 사실과 오해(책)

잘못알려진 오해와 잘 알려지지 않은 사실에 대해 다룸

## 2. 연관 관계 맵핑에 대한 모든 것

JPA는 Java ORM 기술의 표준

연관관계 매핑

Entity들은 대부분 다른 Entity와 연관관계

Class 기반인 Entity는 객체 참조를 이용하여 연관관계 형성

다중성(1:1, 1:n, n:1, n:n) Annotation을 이용하여 설정가능

방향성(단방향, 양방향) 설정 가능

사실상 단방향 매핑으로 연관관계 매핑은 이미 완료(내부적으로 Foreign Key는 하나이기 때문에)

양방향 매핑은 복잡하고 객체에서 양쪽 방향을 관리해주어야함

1. 단방향 매핑
2. 반대방향으로 객체 그래프 탐색기능이 필요할 때 양방향 매핑

영속성 전이  연관된 Entity에도 영속성 상태 변화를 함께 적용하는 것

**다대일 단방향 연관관계 매핑의 cascade**

의도한 대로 동작함

**일대다 단방향 연관관계 매핑의 cascade**

추가적으로 update 쿼리가 두개 수행 됨

**오해 - 양방향 매핑보다 단방향 매핑이 좋다**

1:N 관계의 외래 키 지정을 위해 추가적인 update 쿼리가 발생하는 문제

==> 1:N 양방향 연관관계 매핑을 하면 update 쿼리가 줄어듬

복합키를 사용할 때 MapsId를 사용해서 외래키로 사용가능

외래키를 가지고 있는 쪽이 연관관계의 주인이 됨.(MapsId)

가지고 있지 않은 쪽은 Annotation의 mappedby 속성을 이용하여 양방향 연관관계 매핑

*-ToOne: FetchType.EAGER

*-ToMany: FetchType.LAZY

N+1 문제 Entity에 대해 하나의 쿼리로 조회를 했을 때 연관관계 조회를 위해 N+1번 조회

**N+1 문제에 대한 오해 - EAGER Fetch 전략때문에 발생한다?!**

발생 시점의 차이일 뿐이지, LAZY Fetch 전략을 사용한다고 하더라도 발생함(참조가 발생하지 않으면 추가적인 조회가 발생하지 않긴 함)

**findAll() 메서드는 N+1 문제를 발생시키지 않는다?**

단일 레코드에 대해서만 적용

단일 레코드 조회가 아닌 경우 반환된 레코드 하나 하나에 대해 Entity에 설정된 Fetch 전략을 사용하기 때문에 발생할 수 있음

흔히하는 실수

Pagination + Fetch JOIN을 적용하면 실제로는 모든 레코드를 조회하는 쿼리가 실행된다.
분리해서 수행해줘야 함

둘 이상의 컬렉션을 Fetch JOIN - MultipleBagFetchException이 발생할 수 있음

Bag은 Hibernate에서 중복 요소를 허용하는 비순차 컬렉션

해결방법 - List를 Set으로 변경, @OrderColumn을 사용해서 중복을 제거하거나, 순서 적용

## 3. Spring Data JPA Repository의 숨겨진(?) 기능

Repository란 추상화된 레이어를 제공

Spring Data JPA Repository Data Access layer 구현을 위해 반복적으로 작성했던 유사한 코드를 줄이기 위해 추상화 제공

Paging, Sorting, CRUD 제공, 메서드 이름 규칙을 통한 쿼리 생성기능 제공

단일 Entity에 대한 쿼리는 OK

JOIN쿼리를 실행할 때는?? JPARepository Method로는 JOIN 쿼리를 실행할 수 있을까??

1. JPA는 데이터베이스 테이블 간의 관계를 Entity 클래스의 속성으로 모델링
2. JPA Repository 메서드에서는 "_"를 통해 내부속성에 접근할 수 있다.

**잘 알려지지 않은 사실 - Page vs Slice**

두 메서드의 차이점은? get, read

return Type이 다름

Page는 Slice 상속

전체 페이지 개수, 전체 요소 조회

Page는 count 쿼리가 수행됨, Slice는 limit를 보고 다음 페이지 next여부 판단

Slice가 성능상 이점이 있음

Page에서 count 쿼리가 안나가는 경우(limit 보다 개수가 적으면 안나감)

**JPA Repository Method로는 결국 반환되는 타입은 Entity(DTO Projection 불가????)**

Class 기반 Projection

@Value(생성자 제공) => Projection

Interface 기반 Projection

interface에서 get method 선언하면 Projection

중첩구조를 지원함(@Value + SpEL (target 변수))

```java
interface MemberDetailDto {
	@Value("#{target.pk.type}")
	String getType();
	
	String getDescription();
}
```

Dynamic Projection Generic으로 제공

## 4. Summary

사실상 단방향 매핑으로 연관관계 매핑은 이미 완료.

대개의 경우 충분함

일대다(1:N)에서 단방향 연관관계 매핑에서 영속성 전이 사용시 양방향으로 변경

**Spring Data Jpa Repository**

CRUD, Paging, Sorting 제공

메서드 이름 규칙을 이용한 쿼리 생성 - 이름 규칙에 따라 interface 생성하면 쿼리 생성

JPA Repository 메서드로도 JOIN 쿼리 수행 가능 - 이름 규칙에 따라 _를 이용하여 Entity 탐색

JPA Repository 메서드로도 Projection이 가능하다

소프트웨어 분야에서 많은 실수를 했는데 오해와 실수를 알아서 실수를 줄여나갔으면 좋겠다.

## 4. Q&A


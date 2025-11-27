# DDD-Lite@Spring

정명주 님

인지 과부하를 줄여주겠다.

1. 복잡성과 위기
2. 지식 탐구: 위키
3. 구현: Model-Driven VS Data-Driven
4. 아키텍처와 모듈
5. 결론

## 1. 복잡성과 위기

Motivation: 기술은 항상 발전하는데

- 진짜 중요한 게 뭐지?
- 복잡

문제 자체의 복잡성

우리가 사용하는 기술, 도구

**빠르게 개발(일정 driven)** => 새로운 요구사항 => 복잡성 증가 => 비용 증가, 생산성 감소 =>> 위기 => 차세대 개편 + 레거시 의존

복잡성과 비용 그래프(제곱)

DDD로 위기를 극복하자(은탄환)

해결해야 할 문제(문제공간 to 해결공간) <= Application

해결공간 뽑아내기 => DDD의 전략적 패턴

- 일반(서브 도메인): 대부분의 도메인에서 필요한 서브 도메인(계정, 메일) => 상용 구매
- 지원(서브 도메인): 지원하는 서브 도메인(제품 조회, 제품 검색)
- 핵심: 해결해야 할 핵심 서브 도메인(결제 등) << Model-Driven Design(DDD전략적 패턴을 적용)

전략: 국지적인 전투에서 사용하는 전술

전술: 큰그림

### DDD의 전술적 패턴들(책에 나옴)

Domain Event

DDD-Lite는 DDD 전술적 패턴의 일부를 적용하는 것(여기서는 그냥 이 일부를 지칭하는 것으로 용어 사용)

## 2. 지식 탐구: 위키

지식, 알아가는 과정, 지식 탐구 과정

계정 (일반도메인): 테넌트, 조직, 회원이 존재

프로젝트 (지원 도메인)

**위키 (핵심 도메인): 위키 페이지들의 컨테이너**

Entity(식별할 수 있다, 생명주기가 있다., 행위가 우선한다, 응집력이 중요하다)

Data-Driven(Attribute에 집중)

**DDD는 행위에 집중 Attribute는 그 다음(행위에 필요한 속성을 붙인다.)**

연관관계(필요할 때만 맺어준다.) - 최소한으로, 단방향으로, 도메인을 기반으로

먼저 관계를 그려보는 것도 나쁘진 않음.

값 객체는 불변성, 자체에 행위를 추가 할 수 있어서 활용가능(적극적으로 사용하면 좋을 것 같음): 수량, 주소 등
값 객체는 접미어가 같으면 만들 수 있음(단순화)

값 객체 내부에 값 객체를 조합하거나 entity가 존재해도 됨.

**Aggregate(불변식을 유지하는 것, 데이터 변경의 단위, 트랜잭션의 단위, 락의 범위, 하나에 한 Repository)** 가장 중요함(정제를 제대로 해야함)

AggregateRoot는 Gateway < 이것을 통해서만 조정가능

Domain Event(발행-구독 모델, 트랜잭션 미포함, 낮은 결합도, 다른 Aggregate에 의존하는 것을 처리하는것에 좋음) - divide and  conquare

use case 하나가 application method하나다.

Pub-sub 할 때 관심사별로 발행하는게 좋은 모델임.

Spring-Data는 Trigger가 있어야 하는 단점(DDD에서 권고하는 것은 Trigger를 사용하지 않는 것)

직접구현(단점 보완)

## 3. 구현: Model-Driven VS Data-Driven

Domain Model이 풍부함

도메인 서비스(행위만 가지고 있는것, stateless)

발행 구독의 단점(추적이 안되기 때문에) - javaDoc을 담

POJO vs Spring 의존 (비슷하면 POJO를 사용해야하지 않을까? Trade off)

## 4. 아키텍처와 모듈

레이어드 아키텍처 - 치명적인 단점(도메인이 인프라에 의존하게 됨(복잡성이 폭발적으로 증가))

헥사고날 아키텍처 - 도메인 => Application(Application Service) => Adapter(Primary(init), Secondary(bind) Adapter => 기술적 처리를 담당하는 통로)

모든 의존성이 바깥에서 안으로 향한다

모듈은 java의 package로 표현 가능

adapater

 presentation

  Cli, web

 infrastructure

  기술(엘라스틱 서치, jpa, spring, rabbitmq(primary, secondary))

## 5. 결론

기술보다 도메인이 먼저다

DDD는 학습곡선이 있어서 Trade-off가 있다.

일반, 지원(기존대로 )

핵심(Domain Driven, 전술적 패턴, **헥사고날**)

DDD-Lite 라는 small step으로 복잡성 정복이라는 Big Difference를 만들자.

## 6. Q&A

Q. Mybatis를 쓰는 회사는 어떻게 적용?

A. MSA(가 각광받음), ORM(대중화 됨)

ORM은 훨씬 편함
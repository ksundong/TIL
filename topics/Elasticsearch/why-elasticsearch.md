# 왜 Elasticsearch인가?

## Elasticsearch에 대해서

Elasticsearh는 Apache Lucene 기반의 오픈소스 검색 엔진입니다.

공식 문서를 참고하자면, 분산된 검색 및 분석 엔진이라고 표현하고 있습니다.

또한 실시간 검색 및 분석이 모든 타입의 데이터에 대해 가능하다고 하고, 이 때문에 Elasticsearch의 사용 방안은 매우 다양합니다.

### Elasticsearch의 다양한 사용 사례

- 앱 또는 웹에 검색 창 추가
- 로그, 메트릭, 보안 이벤트 데이터를 분석하고 저장
- 머신러닝
- 비즈니스 워크플로우 자동화(저장 엔진으로 사용)
- GIS(Geographic Information System)으로써의 사용
- 생물 정보학 연구 도구(유전자 데이터 저장 및 처리)

Elasticsearch는 사용 목적이 매우 다양해서 Kibana에 최초 접속할 경우 보여주는 샘플 데이터도 매우 다양합니다.

이러한 특성때문에 학습할 때 도움이 될 수도 있고, 오히려 더 어렵게 느껴지는 원인이 될 것 같네요.

## 그래서 왜 Elasticsearch를 사용하는 것인가?

첫번째로, Java입니다. Java는 어느 OS에서나 실행하기 쉽다는 장점을 가지고 있고, Apache 재단의 힘으로 오픈소스 파워가 상당한 언어 중 하나죠.

두번째로, [오픈 소스](https://github.com/elastic/elasticsearc) 입니다. 일단 오픈 소스의 장점이 바로 내 입맛대로 커스텀이 가능할 수도 있다는 점입니다.(라이선스에 따라 다릅니다.) Elasticsearch는 [Apache2.0 라이선스](https://www.olis.or.kr/license/Detailselect.do?lId=1002)를 따르는 오픈 소스입니다.

세번째로, REST API를 사용합니다. MSA(Micro Service Architecture)가 보편화된 요즘 REST API를 사용하는 이유는 공통 규격의 인터페이스를 사용할 수 있기 때문이라고 생각하는데, Elasticsearch는 자체적으로 REST API를 제공합니다.

네번째로, 분산 환경을 지원해 데이터의 안정성을 보장합니다.

제가 생각하는 이점은 위와 같지만, 또 다른 이점들이 굉장히 많기에 많이 쓰이지 않을까 싶습니다.

## References

[Elasticsearch Official Document](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)

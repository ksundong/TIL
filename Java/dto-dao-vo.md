# DTO, DAO, VO의 구분

이 세 용어는 많이 혼동되어 쓰이고 있습니다. 이는 한국 뿐만이 아니라 해외에서도 공통된 특징입니다.

최근 용어를 어느정도 커뮤니티에서 정립해가는 분위기여서 커뮤니티의 의견을 따른 내용을 정리합니다.

## DTO

DataTransferObject의 줄임말입니다. 주로 레이어(View-Controller-Service-Model)간 데이터를 전송하기 위한 목적으로 사용됩니다. 크게 제약사항을 두지 않고, 단순 데이터 전달목적이기에 변하기 쉬운 특성을 가지고 있습니다. 따라서 객체지향의 원리를 녹이기 보다는, 보다 변화하기 쉬운 특성에 맞춰 개발하는 것이 더 낫습니다.

## DAO

DataAccessObject의 줄임말입니다. Database에 직접적으로 연결되는 계층으로 커넥션을 관리하고, 쿼리를 전송하는 등의 역할을 수행해줍니다. 이를 분리하는 이유는, 변할 수 있는 Database 관련 로직들을 비즈니스 로직과 분리하기 위함입니다.

## VO

ValueObject의 줄임말입니다. 값 객체는 하나 이상의 속성들을 모아서 특정 값을 나타내는 객체를 의미합니다.

VO는 `equals`와 `hashCode` 메서드를 재정의 해야합니다. (자바에서)

이는 동등성 때문인데, 값 객체는 모든 속성이 같은 경우 같은 객체라고 생각해야하기 때문입니다. 또한, hashCode를 재정의하는 이유는 값 객체를 HashMap 같은 hash 값을 사용하는 컬렉션에서 비교용도로 사용할 때, 유용하게 됩니다.

또한, 불변객체여야 합니다. 따라서 `final` 키워드를 통해 불변성을 보장해줄 수 있습니다.

VO를 사용하면 그냥 속성들을 나열했을 때보다 장점이 데이터의 제약사항을 정의해줄 수 있다는 것입니다. 또한, 컬렉션을 VO로 만드는 방법은 일급 컬렉션이라는 방법이 있습니다.
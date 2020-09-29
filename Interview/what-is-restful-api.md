# RESTful API란 무엇인가

RESTful API는 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미합니다.

이해하기 쉽고 사용하기 쉬운 API를 만드는 것을 목적으로 합니다.

- 자원의 표현에 의한 상태 전달
  - 자원: 해당 프로그램이 관리하는 정보
  - 자원의 표현: 해당 자원을 표현하기 위한 이름
  - 상태(정보) 전달: 요청되는 시점에서의 자원의 상태(정보)를 전달
    - xml, json 등으로 정보를 전달합니다.

## HTTP로 REST API를 구현하는 방법

- HTTP URI로 자원을 명시하고, HTTP Method를 통해 해당 자원에 대한 CRUD Operation을 적용합니다.
  - 자원을 가리키는 것은 명사로 표현되어야 합니다.
  - 행위는 Http Method로 표현되며 분명한 목적으로 사용해야 합니다.
- Message는 Header와 Body를 명확하게 분리해서 사용합니다.
  - Entity에 대한 내용은 Body에 담습니다.
- API의 버전을 관리해야 합니다.
  - API의 signature가 변경될 수 있음을 항상 유의합니다.
  - API 변경시에는 하위 호환성을 보장해야 합니다.
- 서버와 클라이언트가 같은 포맷으로 요청과 응답을 하도록 합니다.

- 각각의 HTTP Method에 해당하는 CRUD Operation
  - POST: Create
  - GET: Read
  - PUT, PATCH: Update, Partial Update
  - DELETE: Delete

참고할 만한 글로 [HTTP 메소드의 멱등성](../Network/http-method-idempotent.md)를 추천합니다.

## RESTful API의 장점

- 기존의 웹 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 추가적인 비용(인프라 구축)이 들지 않습니다.
- HTTP 프로토콜이 가진 장점을 사용할 수 있습니다.
- HTTP 프로토콜이 가진 대중성에서 범용성을 확보할 수 있습니다.
- REST API는 사람이 읽을 수 있는 스타일로 작성되어 있기 때문에 의도한 바를 쉽게 알 수 있습니다.
- 서버와 클라이언트의 역할을 명확하게 분리할 수 있습니다.

## RESTful API의 단점

- 명확한 표준이 존재하지 않습니다.
- HTTP Method가 제한적입니다.
- HTTP에 대해서만 지원할 수 있습니다.

## REST가 필요한 이유

- 최근의 서버는 여러 종류의 클라이언트에게 공통된 규격의 API를 제공할 필요성이 대두되었기 때문입니다.
- 또한 MSA에서도 REST API를 사용하면서 그 중요성은 더 커졌습니다.

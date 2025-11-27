# NginX Cors 설정

**주의: 당연히 개발환경임을 염두에 둬야한다. 왜 교차 출처 제한을 하는지 이해하고 해당 설정을 적용해야합니다.**

예전에 했던 프로젝트를 프론트엔드 개발자가 사용하는 과정에서 CORS 에러가 발생했습니다.

왜 맨날 날까요? 정말 CORS로 스트레스 받는게 하루 이틀이 아닌 것 같습니다.

`Access to fetch at 'http://domain/api/feature' from origin 'http://localhost:8080' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.`

- 이 문제는 왜 발생하는가?  
  에러를 읽어보면 'Access-Control-Allow-Origin' 헤더가 없기 때문에 발생함을 알 수 있습니다.  
  아래와 같은 설정을 nginx에 해줘야합니다.
  ```
  add_header "Access-Control-Allow-Origin" *;
  add_header "Access-Control-Allow-Methods" "GET, POST, OPTIONS, HEAD, PUT, DELETE";
  add_header "Access-Control-Allow-Headers" "Authorization, Origin, X-Requested-With, Content-Type, Accept";
  ```

참고로 'no-cors' 옵션을 주면 fetch의 type은 opaque가 되는데, 우리가 의도한 API 요청과 응답용으로는 못써먹으니 알아두고 갑시다.

그럼 다 해결된 것일까요? 제가 개발한 API의 경우 Cookie에 있는 JWT값으로 인증처리를 거칩니다. 따라서 요청은 실패합니다. 왜냐면 쿠키 값이 전달이 되지 않거든요!

- 이 문제는 어떻게 해결할까요?
  fetch 요청을 할 때, 해당 옵션을 주면 됩니다. `credentials: 'include'` 그러면 쿠키에 담긴 값이 전달이 됩니다.
  ```javascript
  fetch('http://domain/api/feature', {credentials: 'include'})
  ```

이렇게 하면 바로 될까요? 우리는 다음과 같은 응답을 받을 수 있습니다.

`Access to fetch at 'http://domain/api/feature' from origin 'http://localhost:8080' has been blocked by CORS policy: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'.`

네 'include' 모드일 때에는 wildcard가 안된다고 합니다. 그럼 어떻게 해야할까요?

제가 처음한 방법입니다.

```
add_header "Access-Control-Allow-Origin" "http://localhost:3000, http://localhost:8080";
add_header "Access-Control-Allow-Methods" "GET, POST, OPTIONS, HEAD, PUT, DELETE";
add_header "Access-Control-Allow-Headers" "Authorization, Origin, X-Requested-With, Content-Type, Accept";
```

이렇게 하면 될까요?

`Access to fetch at 'http://domain/api/feature' from origin 'http://localhost:8080' has been blocked by CORS policy: The 'Access-Control-Allow-Origin' header contains multiple values 'http://localhost:3000, http://localhost:8080', but only one is allowed. Have the server send the header with a valid value, or, if an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.`

문제는 저렇게 여러개를 넣으면 안된다고 하는 것입니다. 제가 테스트하는 환경은 8080포트고 프론트엔드 개발자가 테스트하는 환경은 3000포트기 때문에 무조건 둘 다 만족해야합니다. 그럼 이렇게 해보는건 어떨까요?

```
add_header "Access-Control-Allow-Origin" $http_Origin;
add_header "Access-Control-Allow-Methods" "GET, POST, OPTIONS, HEAD, PUT, DELETE";
add_header "Access-Control-Allow-Headers" "Authorization, Origin, X-Requested-With, Content-Type, Accept";
```

오, 응답이 조금 달라집니다.

`Access to fetch at 'http://domain/api/feature' from origin 'http://localhost:8080' has been blocked by CORS policy: The value of the 'Access-Control-Allow-Credentials' header in the response is '' which must be 'true' when the request's credentials mode is 'include'.`

네 요청 Credentials 모드가 'include'면, Allow-Credentials 헤더도 있어야하고, 그 값은 true여야 한다고 합니다. 그럼 설정해줍시다.

```
add_header "Access-Control-Allow-Origin" $http_Origin;
add_header "Access-Control-Allow-Methods" "GET, POST, OPTIONS, HEAD, PUT, DELETE";
add_header "Access-Control-Allow-Headers" "Authorization, Origin, X-Requested-With, Content-Type, Accept";
add_header "Access-Control-Allow-Credentials" "true";
```

이렇게 설정해줬더니 응답이 잘 옵니다. 정말 귀찮네요!

저 `$http_Origin`은 필요시에만 써야겠죠? nginx의 if 문법을 살펴봅시다. [참고](https://stackoverflow.com/a/33613114/9748089)

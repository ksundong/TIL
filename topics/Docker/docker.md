# Docker 정리

## Docker란 무엇인가?

가장 많은 사람들이 오해하는 것이 바로 `Docker`가 **가상 머신(Virtual Machine, VM)**이라는 것이다.

그러나 Docker는 가상 머신이 아니다. **컨테이너 플랫폼** 이자 **가상화 플랫폼**이다.

왜 **플랫폼(Flatform)**이라는 용어를 썼을까? 왜 **컨테이너(Container)**라는 용어를 썼을까?

Docker의 구글 이미지 검색을 하면 나오는 이미지가 이를 잘 설명해준다고 생각한다. (이미지 출처: [Docker 공식 홈페이지](https://www.docker.com/sites/default/files/social/docker_facebook_share.png))

![Image result for Docker"](https://www.docker.com/sites/default/files/social/docker_facebook_share.png)

저 **고래형태**가 바로 Docker라고 볼 수 있고, Dock은 영어로 부두를 의미하기도 한다.

부두엔 배도 있지만 **컨테이너**도 있다.(이미지 출처: [영문 위키백과 Container Port](https://en.wikipedia.org/wiki/Container_port))

![port](https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Port_of_Singapore_Keppel_Terminal.jpg/1920px-Port_of_Singapore_Keppel_Terminal.jpg)

즉 고래 위의 **네모 모양**이 바로 컨테이너인 것이다.

요약하자면 Docker는 컨테이너를 적어도 보관하는 정도까지는 해준다고 이미지를 통해 유추할 수 있다.

왜 플랫폼이란 용어를 썼을까? [네이버 영한사전](https://en.dict.naver.com/#/entry/enko/293f2220859843409c7bfce7af553495)을 참고해보면 플랫폼은(사용 기반이 되는 시스템·소프트웨어)를 의미한다.

즉, Docker는 컨테이너의 사용 기반이 되는 소프트웨어라고 볼 수 있다.

왜 컨테이너라는 용어를 썼을까? 레드햇의 [한국어 설명](https://redhat.com/ko/topics/containers/whats-a-linux-container)을 한 번 참고해보자.

> Linux 컨테이너는 시스템의 나머지 부분과 격리된 하나 이상의 프로세스 세트입니다.
>
> 이러한 프로세스를 실행하는 데 필요한 모든 파일은 고유한 이미지에서 제공되므로, Linux 컨테이너는 개발 단계에서 테스트, 프로덕션에 이르기까지 이식성과 일관성을 유지할 수 있습니다.

다른 부분도 중요하지만 컨테이너가 무엇인지, 컨테이너라는 용어는 또 무엇인지를 아는 것 부터가 우선이라 볼 수 있다.

컨테이너는 프로세스 세트다. 즉, 프로세스의 정의에 의해서 컨테이너는 동작하고 있는 무엇인가라고 볼 수 있다.

그렇다면 이미지는 파일을 제공해 준다는데, 저장소라고 볼 수 있을까? 반은 맞고 반은 틀리다.

이미지란 가상화된 환경을 저장하고 있는 스냅샷(Snapshot)에 가깝다고 볼 수 있다. 즉, 이미지를 이용해 컨테이너를 만든다.

요약하자면 Docker는 컨테이너 가상화 플랫폼으로, 이미지를 이용해 컨테이너를 실행시키는 소프트웨어이다.

그리고, 이렇게 Docker를 이용하면 Container는 외부 환경과 분리되어 독립적으로 실행될 수 있습니다.

## Docker를 왜 쓰는가?

위의 레드햇 문서에서도 볼 수 있듯이 Docker는 Application이 Guest OS에 종속되지 않는다. 이 부분이 제일 핵심적인 부분이다.

개발단계의 환경이 곧 프로덕션 환경과 동일하다는 것은, 기존의 Develop-Develop Server-Test Server-Production Server의 단계를 획기적으로 줄일 수 있다는 것이고, Develop 단계에선 발생하지 않던 오류가 Production Server에서 터지는 경우가 매우 줄어들 수 있다는 것 입니다.

즉, 제가 1년 전에 설치한 VM의 설치과정과 현재의 VM의 설치과정은 다를 수 있습니다. 이를 Snowflake Server(눈송이 서버)라고 칭합니다. 왜냐하면, 눈송이의 모양은 비슷해 보이지만 자세히 보면 모양이 제각기 다릅니다. 설치과정에서 이런일은 비일비재하게 일어나기도 합니다. 저번 주 금요일에 수행했던 서버 설치 작업이 이번 주 월요일에 출근해서 보니 달라지는 경우도 있습니다.

하지만, Docker는 그러한 시행착오를 한 번만 겪으면 됩니다. 그것도 Develop과정에서요. 단지 서버에는 Docker만 있으면 됩니다. 이를 Phoenix Server(불사조 서버)라고 합니다. 불사조는 죽어도 다시 태어나는데, 이에 빗댄 표현이죠. 때론 일부러 불살라버려서 새 불사조로 바꾸어주는 편이 좋다고 합니다. 그리고 1년 전이나 현재나 변함이 없습니다.

즉, OS부터 다시 설치하는건 시간도 걸리고 너무 복잡합니다. 또 실패할 가능성도 있습니다.

하지만 Docker Image 부터 만드는 과정은 딱 한 번만 시행착오를 겪으면 되고, Production Level에서는 실패할 확률이 VM보다 현저히 줄어듭니다. 또한 외부환경으로부터 자유롭기 때문에 어느 플랫폼에서든 동일한 동작을 보장할 수 있습니다.

또한 VM에 비해서 경량화 되어있기 때문에 MSA(MicroService Architecture)로의 발전도 이끌었다고 볼 수 있습니다.

~~적다보니 지식의 부족을... 느낍니다...~~

## References

예전에 들었던 아샬님 강의 중 일부

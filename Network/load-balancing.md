# 로드 밸런싱

쉽게 말해서 여러대의 컴퓨터가 있고, 이 여러대의 컴퓨터에 각각 작업을 분산시켜서 나눠주는 것을 의미합니다.

## 로드 밸런싱이 왜 필요하고, 없으면 어떻게 될까요?

일반적인 WAS(Web Application Server)를 예로 들어봅시다. 특정 서버 하나의 크기를 키워서 대응하는 수직확장 보다는 수평 확장을 통해 대응하는 것이 더 효과적입니다. 왜냐하면 서버의 크기를 늘리는 것은 물리적인 한계가 존재하기 때문입니다.

이 수평 확장을 적용하게 되면, 신경써야 할 부분이 수직 확장에 비해 많아지게 되는데 그 중 하나가 부하분산입니다. 여러 서버들을 사용할 때, 특정 서버에만 트래픽이 몰리지 않도록 골고루 처리하도록 하는 것입니다.

로드밸런싱이 없다면 어떻게 될까요? 최악의 경우 특정 서버에는 요청이 몰리지만, 요청이 아예 없다시피 한 서버도 존재하게 될 것입니다. 그렇다면 우리가 굳이 수평확장을 선택한 이유가 없어져버립니다. 요청이 몰려서 생기는 장애를 방지하고자 분산환경을 구성했는데, 장애가 발생하게 되는 것이죠.

또 하나의 장점으로는 로드밸런서를 사용함으로써 안정성이 올라간다는 점입니다. 위의 사례를 들더라도 전체 사용자에게 서비스의 장애가 발생하진 않습니다. 특정 사용자군에 한해서만 장애가 발생할겁니다.

즉, 로드 밸런서를 사용하면 사용자 경험을 크게 개선할 수도 있고, 애플리케이션 안정성에도 크게 기여하게 됩니다.

## 로드 밸런싱 알고리즘

### 구간 합(prefix sum) 알고리즘

알고리즘 문제풀이시 종종 등장하는 구간 합 알고리즘입니다.

대략의 원리는 미리 해당지점까지의 총합을 저장한 배열(SUM)이 있고, 구간(S, E)이 주어지면, SUM(E) - SUM(S - 1)이 구간의 합이 됩니다.

이는 매우 제한적인 상황에서 사용되는 알고리즘이고, 로드 밸런싱을 할 때도 마찬가지입니다.

각각의 작업은 독립적이어야 하고, 각각의 실행 시간과 작업을 세분화 할 수 있어야 합니다.

보통의 웹 서비스는 불규칙한 시간과 작업을 다루므로, 이 알고리즘이 있다는 것만 알아두고 넘어가도 무방할 것 같습니다.

[Prefix sum](https://en.wikipedia.org/wiki/Prefix_sum)

### 라운드 로빈(Round Robin)

가장 우리가 이해하기 쉬운 로드 밸런싱 알고리즘 입니다. (로드 밸런싱 이외에도 컴퓨팅의 여러 분야에서 두루두루 쓰이는 알고리즘입니다.)

서버를 순환하면서 요청을 분배해줍니다. 이 방법의 문제점이라면, 요청이 얼마나 걸리는지 신경쓰지 않고 순서대로 분배하기 때문에 한 서버에 부하가 많이 걸릴 수 있다는 점입니다.

알고리즘 자체는 매우 간단하기 때문에, 알고리즘 상으로는 가장 빠른 알고리즘입니다.

이 방법은 모든 애플리케이션 서버가 동일한 가용성, 컴퓨팅 및 처리 능력을 가지고 있다고 가정합니다.

### 가중 라운드 로빈(Weighted Round Robin)

가중 라운드 로빈은 각 서버에 가중치를 부여해서 특정 서버에 요청을 더 분배하고, 덜 분배하는 방식으로 동작합니다.

예를 들어서 서버 1의 가중치가 2고, 서버 2, 3의 가중치가 1이라면, 요청이 5개가 들어왔을 때, 1, 2 번 요청은 서버 1에 전달되고, 3, 4번 요청은 서버 2, 3에 전달되며, 5번 요청은 다시 서버 1로 전달됩니다.

라운드 로빈과 같은 문제점을 가지고 있지만, 서버 별로 성능상의 차이가 있을 경우 유용합니다.

### 무작위 로드 밸런싱(Random Load Balancing)

요청을 무작위로 할당합니다. 무작위로 분배하기 때문에 요청이 많아질 경우 복잡한 로드 밸런싱 알고리즘 보다 빠른 성능을 보여줄 수도 있습니다.

흥미로운 자료를 발견해서 아래에 붙여두었습니다. 자세한 내용은 아래의 Nginx 항목을 보는 편이 더 이해가 잘 되실겁니다.

[The power of two random choices](https://brooker.co.za/blog/2012/01/17/two-random.html)

### 가중 무작위 로드 밸런싱(Weighted Random Load Balancing)

사용자가 지정한 가중치에 따라 확률적으로 무작위로 선택된 서버에 요청을 분배합니다.

해당 내용은 자료가 충분치 않아 판단 기준을 세우기 어렵다고 판단했습니다.

[Weighted Random Load Balancing property](https://learn.akamai.com/en-us/webhelp/global-traffic-management/global-traffic-management-user-guide/GUID-F37B1B6A-307E-42C3-B648-C8845FC572DC.html)

### 최소 연결(Least Connection)

### 가중 최소 연결(Weighted Least Connection)

### 최소 시간(Least Time), 빠른 응답(Fastest Response)

### 'Power of Two Choices' 알고리즘

임의의 서버 두 개를 고르고 그 중에서 연결 수가 더 적은 서버를 선택합니다. 이는 위에서 설명한 최소 연결 알고리즘과 동일한 선택 기준을 갖습니다. 혹은 최소 시간 알고리즘을 사용해도 됩니다.

모든 서버를 확인하는 비용을 절약하고, 완전 무작위보다는 나은 결정을 하겠다라는 것이 그 아이디어입니다.

완전 랜덤보다 더 공정한 결과를 반환할 수 있습니다.

아래의 글은 편향적인 내용이 일부 포함될 수 있습니다. 벤치마크는 실제환경과 거리가 있을 수 있습니다.

[Test Driving "Power of Two Random Choices" Load Balancing - HAProxy Technologies](https://www.haproxy.com/blog/power-of-two-load-balancing/)

### 최소 대기(Adaptive)

### 고정(Fixed)

### 해시 테이블(Hash Table)

### 마스터-워커(Master Worker)

### 참고 자료

[Load balancing (computing)](<https://en.wikipedia.org/wiki/Load_balancing_(computing)#Approaches>)

[Load Balancing Algorithms and Techniques](https://kemptechnologies.com/load-balancer/load-balancing-algorithms-techniques/)

[NGINX and the "Power of Two Choices" Load-Balancing Algorithm - NGINX](https://www.nginx.com/blog/nginx-power-of-two-choices-load-balancing-algorithm/)

[로드 밸런싱 알고리즘 종류](https://weicomes.tistory.com/7)

## 하드웨어 로드 밸런서 vs 소프트웨어 로드 밸런서

## 로드 밸런서 vs 리버스 프록시

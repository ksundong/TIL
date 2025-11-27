# S.O.L.I.D 원칙

- SRP(Single Responsibility Principle, 단일 책임 원칙)
  - 모든 모듈 또는 클래스는 프로그램의 기능 중 하나에 대한 책임만을 가져야 한다.
  - 변경이 필요할 때, 수정할 대상은 하나여야 하고 변경대상이 여러개여서는 안된다.
- OCP(Open-Closed Principle, 개방-폐쇄 원칙)
  - 모든 소프트웨어 구성요소들은 확장에 있어선 열려있어야 하고, 수정에 있어선 닫혀있어야 한다.
- LSP(Liskov Substitution Principle, 리스코프 치환 원칙)
  - 부모클래스는 자식클래스로 대체 될 수 있어야 하고, 정상적으로 동작해야한다.
- ISP(Interface Segregation Principle, 인터페이스 분리 원칙)
  - 클라이언트는 사용하지 않는 메서드에 의존해선 안되고, 필요한 경우 인터페이스를 분리해야 한다.
- DIP(Dependency Inversion Principle, 의존 역전 원칙)
  - 높은 수준의 모듈은 낮은 수준의 모듈에 의존해선 안된다.
  - 추상화는 세부 사항에 의존해선 안되고, 세부 사항이 추상화에 따라 달라져야 한다.
  - 보통 DI(Dependency Injection)로 해결한다.

목적: 재사용 가능하고, 유지보수 가능하며, 테스트 가능한 코드를 만들기 위함.

# Today I Learned

- 좋은 개발자가 되기 위해 하루동안 학습한 내용이나 개발관련 경험들을 간단한 기록으로 남긴다.
- 기본기를 중심으로 학습하고, 나아가 기본기를 바탕으로 발전하는 개발자가 된다.
- 학습의 연속성을 유지하고, 반드시 제대로 숙독한 내용만 기록에 남긴다.
- wiki처럼 운용이 가능하도록 지향한다.

## 작성 규칙

- 해당 문서를 다시 봤을 때, 추가적인 검색의 비용이 들지 않도록 자세히 기록한다.
- reference를 명시하고, 원작자가 참고를 허용하는 자료만 사용한다.
- 뭐든 잘 쓴다.

## 디렉토리 구조

```
TIL/
├── daily/              # 일별 TIL
│   ├── 2025/
│   │   └── 11/
│   │       └── 2025-11-27.md
│   └── archive/        # 과거 기록
├── topics/             # 주제별 심화 학습
│   ├── Java/
│   ├── Spring/
│   ├── Docker/
│   └── ...
├── books/              # 독서 노트
├── interview/          # 면접 준비
└── README.md
```

### 분류

- [세미나&컨퍼런스](#Seminar--Conference)
- [HTML/CSS](#HTML--CSS)
- [자바](#Java)
- [백기선님 라이브 스터디](#Live-Study)
- [코틀린](#Kotlin)
- [자바스크립트](#JavaScript)
- [디자인패턴](#Design-Pattern)
- [스프링](#Spring)
- [스프링부트](#Spring-Boot)
- [ORM](#ORM)
- [깃](#Git)
- [SQL](#SQL)
- [MySQL](#MySQL)
- [데이터베이스](#Database)
- [엘라스틱 서치](#Elasticsearch)
- [리눅스](#Linux)
- [클라우드](#Cloud)
- [파이썬](#Python)
- [도구](#Tools)
- [도커](#Docker)
- [운영체제](#OS)
- [네트워크](#Network)
- [도서](#Books)
- [면접](#Interview)
- [생산성](#Productivity)
- [DevOps](#DevOps)

---

### Seminar & Conference

- [개발자 이력서 비하인드 스토리](topics/Seminar/개발자-이력서-비하인드-스토리.md)

### HTML & CSS

- [link태그와 @import문의 차이](topics/HTML&CSS/difference-with-link-tag-and-import-statement.md)

### Java

- [Java 배경지식](topics/Java/java-background.md)
- [Java의 정석 변수 파트 내용 정리](topics/Java/java-the-variable-part.md)
- [Java의 정석 연산자 파트 내용 정리](topics/Java/java--the-operator-part.md)
- [Java의 정석 조건문과 반복문 파트 내용 정리](topics/Java/java-the-control-statement-part.md)
- [Java의 정석 배열 내용 정리](topics/Java/java-the-array-part.md)
- [Java의 정석 객체지향 프로그래밍 1 내용 정리](topics/Java/java-the-oop-part-1.md)
- [Java의 정석 객체지향 프로그래밍 2 내용 정리](topics/Java/java-the-oop-part-2.md)
- [멤버변수와 로컬변수의 차이점](topics/Java/The-Difference-Between-Member-Variable-And-Local-Variable.md)
- [Java의 날짜와 시간 API](topics/Java/Date-Time-API.md)
- [SecureRandom과 Random의 차이점](topics/Java/Difference-with-SecureRandom-and-Random.md)
- [String join](topics/Java/String-join.md)
- [map과 flatMap의 차이](topics/Java/Difference-with-map-and-flatMap.md)
- [자바 메모리 구조](topics/Java/Memory-Architecture.md)
- [JVM 기본 GC 확인하는 방법](topics/Java/JVM-default-GC.md)
- [동등성과 동일성](topics/Java/equality-and-identity.md)
- [JUnit5 문서 기초부분 번역](topics/Java/junit5-basic-document.md)
- [atomic vs volatile vs synchronized 차이](topics/Java/atomic-vs-volatile-vs-synchronized.md)
- [Java8 이후 추가된 유용한 기능들](topics/Java/java8-after-useful-features.md)
- [DTO, DAO, VO](topics/Java/dto-dao-vo.md)
- [JDK, JVM, 자바 메모리 구조](topics/Java/jdk-jvm-java-memory-structure.md)
- [GC](topics/Java/gc.md)

### Live-Study

- [1주차: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가](topics/live-study/what-is-jvm-and-how-to-execute-java-code.md)
- [2주차: 자바 데이터 타입, 변수 그리고 배열](topics/live-study/java-data-types-variables-and-array.md)
- [3주차: 연산자](topics/live-study/operator.md)
- [4주차: 제어문](topics/live-study/control-statement.md)
- [5주차: 클래스](topics/live-study/class.md)
- [6주차: 상속](topics/live-study/inheritance.md)
- [7주차: 패키지](topics/live-study/package.md)
- [8주차: 인터페이스](topics/live-study/interface.md)
- [9주차: 예외 처리](topics/live-study/exception-handling.md)
- [10주차: 멀티스레드 프로그래밍](topics/live-study/multithread-programming.md)
- [11주차: 열거형](topics/live-study/enum.md)
- [12주차: 애노테이션](topics/live-study/annotation.md)
- [13주차: I/O](topics/live-study/io.md)
- [14주차: 제네릭](topics/live-study/generics.md)
- [15주차: 람다식](topics/live-study/lambda.md)

### Kotlin

- [Kotlin extensions를 분석하면서 공부하는 Kotlin](topics/Kotlin/learn-kotlin-while-analyzing-kotlin-extensions.md)

### JavaScript

- [호이스팅, 클로저, this(이해가 잘 안간다)](topics/JavaScript/hoisting-closure-and-this.md)
- [비동기 자바스크립트, 프로미스](topics/JavaScript/asynchronous-javascript-and-promise.md)
- [SSR과 CSR](topics/JavaScript/ssr-and-csr.md)

### Design Pattern

- [싱글톤 패턴](topics/DesignPattern/singleton-pattern.md)

### Spring

- [스프링 삼각형](topics/Spring/spring-triangle.md)

### Spring Boot

-

### ORM

- [JpaRepository의 getOne 메소드와 CurdRepository의 findById 메소드의 차이](topics/ORM/difference-with-getone-and-findbyid.md)

### Git

- [빈 커밋 만드는 방법](topics/Git/make-empty-git-commit.md)
- [ProGit 정리](topics/Git/progit-summary.md)

### SQL

- [스칼라값을 특별 취급 하는 이유](topics/SQL/why-scala-is-special.md)
- [테이블의 데이터를 전부 지울땐 DELETE를 사용하지 말자](topics/SQL/don-t-use-delete-when-delete-all-data.md)

### MySQL

- [DB 테이블에서 변경된 데이터 추출](topics/MySQL/Extracting-different-data-from-DB-table.md)
- [MySQL UTF-8 Collations](topics/MySQL/Mysql-UTF-8-Collations.md)
- [MySQL Database와 Table Character set확인](topics/MySQL/MySQL-database-table-character-set-check.md)
- [MySQL 공간데이터 저장](topics/MySQL/mysql-store-spatial-data-type.md)

### Database

-

### Elasticsearch

- [왜 Elasticsearch인가?](topics/Elasticsearch/why-elasticsearch.md)
- [Elasticsearch REST API들](topics/Elasticsearch/elasticsearch-rest-apis.md)

### Linux

- [Chsh zsh non-standard shell](topics/Linux/chsh-zsh-non-standard-shell.md)
- [포트 사용중인 프로세스 찾기/죽이기](topics/Linux/find-and-delete-process-using-port.md)
- [Byobu 사용법](topics/Linux/how-to-use-byobu.md)

### Cloud

-

### Python

- [파이썬 리스트 메소드들](topics/Python/python-list-method.md)
- [코드리뷰를 통해 배우는 파이썬 정리](topics/Python/learn-python-with-code-reviews.md)

### Tools

- [IntelliJ IDEA 추천 플러그인](topics/Tools/Intellij-recommended-plugins.md)
- [IntelliJ IDEA Plain Java 프로젝트를 Gradle 프로젝트로 바꾸는 방법](topics/Tools/intellij-plain-java-to-gradle.md)
- [IntelliJ 저장시에 마지막에 빈라인 추가하기](topics/Tools/intellij-save-with-new-line.md)
- [IntelliJ IDEA Oracle DB Connection](topics/Tools/IntelliJ-Oracle-DB-Connection.md)
- [Docker로 ELK 스택 설치하기](topics/Tools/docker-elk-stack-install-guide.md)
- [IntelliJ IDEA Gradle Lombok Dependency Setting](topics/Tools/gradle-lombok-dependency.md)
- [Mac에서 Cmd+M 단축키 변경시키기(Minimize Window)](topics/Tools/change-cmd-m-shortcut.md)

### Docker

- [Docker 호스트와 컨테이너 간 파일 전송하기](topics/Docker/transfer-files-between-container-and-host.md)
- [Docker란 무엇이고, 왜 쓰는가](topics/Docker/docker.md)

### OS

- [프로그램의 실행 순서와 메모리 공간](topics/OS/program-execution-order-and-memory-space.md)
- [프로세스와 스레드](topics/OS/process-and-thread.md)

### Network

- [네트워킹이란 무엇인가?](topics/Network/what-is-networking.md)
- [Nginx Cors Setting](topics/Network/nginx-cors-setting.md)
- [HTTP Method의 멱등성](topics/Network/http-method-idempotent.md)
- [브라우저에서 DNS Lookup은 어떤과정으로 진행되는가](topics/Network/browser-dnslookup-process.md)
- [TCP 프로토콜](topics/Network/tcp-protocol.md)
- [HTTPS의 동작 원리](topics/Network/https-principle-of-operation.md)
- [로드 밸런싱](topics/Network/load-balancing.md)
- [WebServer와 WAS](topics/Network/webserver-and-was.md)

### Books

- [토비의 스프링](books/토비의-스프링.md)
- [오브젝트](books/오브젝트.md)

### Interview

- [객체지향 프로그래밍이란 무엇인가?](interview/what-is-oop.md)
- [S.O.L.I.D 원칙](interview/solid-principles.md)
- [RESTful API란 무엇인가?](interview/what-is-restful-api.md)
- [브라우저에서 서버 통신과정](interview/browser-to-server.md)
- [TCP와 UDP의 차이점](interview/difference-between-tcp-and-udp.md)
- [GET과 POST 방식의 차이점](interview/difference-between-get-and-post.md)
- [프로세스와 스레드의 차이](interview/difference-between-process-and-thread.md)
- [멀티 스레드](interview/multi-thread.md)
- [동시성](interview/concurrency.md)
- [동기와 비동기](interview/sync-async.md)
- [교착상태와 기아상태](interview/deadlock-and-starvation.md)

### Productivity

### DevOps

- [CI/CD란 무엇이며, 왜 필요한가?](topics/DevOps/ci-cd.md)

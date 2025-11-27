# '깃'깔나는 Git Workflow 알아보기

신승엽 님

9년차 개발자

1. 주요 Git 워크플로 살펴보기
    - Gitflow
    - GitHub flow
    - GitLab flow
2. 우리는 이렇게 해요!
    - 브랜치 전략
    - 개발 플로

## 1. 주요 Git Workflow 살펴보기

### - Gitflow

많은 도구에서 공식으로 지원하는 branch model

Main branch: master(공식)/develop(다음 버전 개발)

Supporting branch: 병렬업무가 가능, 필요시 생성, 완료시 삭제

	- feature branch: merge commit을 통해 기록 유지
	- release branch: 배포 전 소소한 버그 수정 develop과 master에 둘다 merge
	- hotfix branch: 기본적인 개념은 release branch와 동일하나 master에서 생성 develop과 master에 둘다 merge

규칙만 지켜주면 많은 사람들이 충돌없이 진행 가능

### - GitHub flow

Git flow보다 훨씬 간단한 workflow

본인들의 개발철학을 설명(이해하기 쉬워 실수가 없게 됨)

master는 stable한 상태여야 함.(유일한 강제사항)

master에 test를 수행하지 않고 push하면 깨지게 되므로 주의하는데 강제함

Topic branch(기능을 설명하는 명확한 이름): commit을 서버로 꾸준히 push를 해야함(하드웨어 고장 예방), 커뮤니케이션

기능 개발 중 pull request를 개설하여 변경 사항, 코딩 가이드라인 등 의견 공유가 가능

배포 락: 특정 topic branch가 반영중이라는 것을 설명하는 락

배포 후 15분간 오류가 없는지 테스트, rollback이 필요하면 다시 master를 배포

이상이 없다면 pull request를 이용해서 merge

### - GitLab flow

Git flow는 너무 복잡(너무 지나친 규칙)

GitHub flow는 너무 간단

- 지속적인 배포가 어려울 때, merge가 불가능함(GitHub flow의 경우)
  GitLab flow에서는 production branch를 두어 배포를 하도록 함(자동 배포 Script를 사용하면 정확한 시간 파악 가능)

- 환경별 배포가 필요할 때, 사내 환경, 실제 환경
  test를 통과한 commit만 merge 가능 hotfix도 pre-production branch에 merge 후 production에 merge
- 릴리즈 소프트웨어일 때, Bug-fix할 때, cherry-pick으로 수행
- 개발이 완료되지 않았더라도 중간 겨로가를 팀 내에 공유하라
- 항상 이슈 트래커 시스템에 이슈를 생성하라
  - 개발은 항상 코드 변경
  - Pull Request를 받고싶은 사람에게 요청하라
- Commit을 자주하고 Push를 자주하라
  - 코드를 봤을 때 Context이해에 도움
  - Pull Request를 상세히
- Merge 전에 테스트 하라
  - CI 빌드를 돌려주는 기능을 수행하면 검증되지 않은 코드가 배포되는 것을 막을 수 있음

## 2. 우리는 이렇게 해요!

### - 브랜치 전략

다양한 서비스가 존재, 각 서비스 간에도 다양한 요구사항이 존재(단기간, 장기간 프로젝트가 공존)

- 코드네임을 부여하여 관리
- 개발을 동시에 수행함
- 개발 브랜치를 여러개 둠(일정, 코드네임 별로)
- Feature 브랜치를 각자 만들어서 개발(기능명)
- Pull Request를 생성하여 Merge함
- 개발 완료 후 QA 진행(release branch를 따로 생성하지 않음)
- merge commit을 유지하기 위해 -p 옵션을 이용하여 rebase를 수행 후 각 개발자들이 feature branch를 rebase(충돌 발생시 다시 생성 후 cherry pick)
- hotfix branch도 pull request 생성, merge는 master와 develop에 추가
- 장점: 각 개발 일정별 branch가 유지됨, commit history를 유지할 수 있음
- 단점: 복잡함, rebase에 대한 이해 필요

### - 개발 플로

Dooray! JIRA와 비슷한 Issue Tracker(이슈번호 tracking)

Pull Request는 몇 가지 조건을 통과하지 않으면 Merge 불가하도록 설정(GitHub에 branch protection기능)

1. 코드 리뷰
2. 테스트를 통과해야함(base 브랜치의 최신 변경사항이 포함되어야 하는 옵션 선택 추천)
   1. 젠킨스를 통한 자동 테스트(pull request 생성시 자동 테스트)
   2. 소나큐브를 이용한 정적 분석(실수나 취약점을 분석)

GitHub PR + Jenkins + SonarQube

- 기계적인 테스트, 코드리뷰를 덜어내어 비즈니스 로직에 집중 가능
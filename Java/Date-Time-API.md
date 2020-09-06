# Java 날짜와 시간 API

Java8 이전엔 Date, Calendar 클래스로 API를 제공했었습니다.  
하지만 뭔가 사용이 불편하고, 잘못된 점이 많아서 Joda-Time 등의 라이브러리가 만들어져서 사용되었습니다.

Java8 이후엔 개선된 날짜와 시간 API를 제공하게 되었습니다.

**날짜와 시간계산은 사회제도나 과학에 복잡하게 얽혀있습니다.**  
때문에, 우리는 날짜관련 로직을 직접 구현하기 보다는 구현된 Library 등을 이용하여 하는 편이 오류를 줄일 수 있습니다.

1. 1582년 10월 4일의 다음날은? (10월 15일)
   율리우스력의 누적 오차를 그레고리력을 적용하면서 교정하기위해 10일을 건너뛰었습니다.
   `Calendar.getInstance()` 는 역사와 천문학이 복잡하게 얽혀있습니다.
   Locale 정보에 따라 다른 `Calendar`를 반환합니다.
2. 우리나라 1988년 5월 7일 23시의 1시간 후는 5월 8일 1시입니다.
   이 시기에 시행된 일광시간 절약제 때문입니다. 일광시간 절약제의 확인은 `TimeZone.inDaylightTime()`으로 합니다.
   그러나 이는 실제와 달랐었습니다. [참고](http://blog.benelog.net/3120317)
   Java는 **시간대 데이터베이스**에 있는 데이터를 기반으로 계산을 합니다.
3. 우리나라의 표준시 관련 문제도 있었습니다.
4. 윤초문제도 있습니다.
   윤초를 계산하지 않아서, 그로 인한 Linux + Java 기반 시스템의 문제가 있었습니다.
   Google의 경우에는 'leap smear' 기법으로 해당 문제를 미리 방지하였습니다.

Date 클래스가 UTC를 정확히 반영하는지의 여부는 JVM의 실행환경에 의존적입니다.(대부분의 컴퓨터는 되지 않습니다.)

윤초때문에 OS, Java, Middle Ware, Application의 Malfunction이 예상됩니다.  
그래서 윤초를 없애자는 얘기도 나오고 있습니다.

**결론: 날짜와 시간은 어렵고, 복잡한 영역이고 배경지식또한 필요로 합니다.**

## Calendar와 Date 클래스의 문제

1. 불변객체가 아닙니다.
   내부 속성 변경이 가능하여, 다른 곳에서 변경하면 결과에 영향이 있을 수 있습니다.
2. int 상수필드를 남용하였습니다.
   엉뚱한 값을 넣어도 Compile Error, Exception등의 오류가 발생하지 않아 잘못된 값을 넣었는지 알 수 없습니다.
3. 일관성 없는 요일 상수
   넣을 때와 받을 때의 값이 서로 다릅니다.
4. Date와 Calendar에서 불편한 부분
   복잡한 날짜계산은 Calendar를 이용하게 되는데, 이는 불필요한 중간 객체일 수 있습니다.
   또한, Date는 날짜와 시간 모두를 저장하는 클래스인데, 이름이 Date입니다.
5. 시간대 지정에 둔감합니다.
6. 하위 클래스의 문제

- `java.sql.Date` 이름이 겹치고, Comparable을 클래스에서 선언하지 않았습니다.
- `java.sql.Timestamp` equals의 대칭성을 어겼습니다.(Effective Java. Item #10)

**결론: 이제는 Date, Calendar를 사용하지말고, Java8의 날짜 시간 API를 사용하자.**

## Reference

[Naver D2 Java의 날짜와 시간 API](https://d2.naver.com/helloworld/645609)
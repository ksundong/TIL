# IntelliJ IDEA Oracle DB Connection

DB Version: Oracle 11g Express Edition

ojdbc Version: oracle8(In sql_developer)

Develop Environment: Windows 10 Pro, IntelliJ IDEA Ultimate 2019.2, Gradle, Zulu JDK 8_202, Git

먼저, 가장 먼저 발생한 오류는 Class Not Found Exception이었다.
이 문제를 해결하기 위해 시도한 방법은,

1. sql_developer에 있는 파일을 Project Structure에서 Library 에 추가한 후 테스트 수행: 실패
   Class Not Found Exception

2. 직접 프로젝트 경로에 libs 폴더에 넣은 다음 Project Structure에서 Library 에 추가한 후 테스트 수행: 실패
   Class Not Found Exception

3. 먼저 IntelliJ의 Database Menu에서 접속시도(IntelliJ에서 제공하는 ojdbc 드라이버 사용): 실패
   해당 버전은 상위버전을 지원하는 jdbc인 것 같음

4. Database Menu에서 Driver의 Oracle을 누른 후 기존의 ojdbc 드라이버 제거 후 sql_developer의 ojdbc8.jar파일 Import, 접속시도: 실패(성공<Listener Process가 살아있는 경우>)

   > 접속이 가능할 수도 있다, 나의 경우 Listener Process가 죽어서
   > TNS:listener does not currently know of SID given in connect descriptor. 라는 오류가 발생하였다.
   > 재부팅 후 성공하였음.

5. gradle로 build를 해주기 때문에 gradle에서 local에 있는 jar파일을 dependencies에 추가하는 방법 검색

   ```groovy
   // build.gradle
   repositories {
       flatDir { 
           dirs 'libs'
       }
   }
   
   dependencies {
       implementation name: 'ojdbc8'
       testImplementation name: 'ojdbc8'
       testCompile name: 'ojdbc8'
   }
   ```

   해당 내용 추가 후 혹시 몰라서 Project Structure에서 Modules에 체크박스를 체크해주었다.

   이렇게 까지 했는데, 실패하면 나처럼 connection을 맺는 소스를 오타를 치거나 잘못 적었을 수도 있다.

6. 그래도 실패한 경우,

   ```java
   Connection con = DriverManager.getConnection(
   				 "jdbc:oracle:thin:@localhost:1521:XE" // url
   				,"book"   // user
   				,"admin") // password
   ```

   위의 내용중 url 부분을 `jdbc:oracle.thin:@localhost:1521:XE` 으로 적었다. `oracle.thin` 으로...

해당 내용까지 적용하면 어지간해선 실패하지 않으리라 예상한다.

sql_developer 및 IntelliJ Database Menu에서 접속시도를 해보고 접속이 되는 경우면, 잘 빌드하고 잘 입력하면 될 것이다.
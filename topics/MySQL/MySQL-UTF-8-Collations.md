# MySQL UTF-8 Collations

Mysql이 설정할 수 있는 UTF-8 Unicode관련 charset은 `utf8`, `utf8mb4`, `utf8mb4_0900` 이 있다.

각각의 유의미한 차이는 속도이며, `utf8` 은 표현에 제한이 있으므로 `utf8mb4` 를 사용할 수 있다면 `utf8` 은 지양해야한다. 특히 이모지 표현이 불가능하다.

`5.5.3` 이전은 `utf8` 을 `5.5.3` 부터 `8.0` 이전까지는 `utf8mb4` 를 `8.0` 이후부터는 `utf8mb4_0900` 을 선택해주면된다.

MariaDB는 `utf8mb4` 까지만 지원하고 있으므로 이것을 사용하면 된다.

collation은 대표적으로 `general_ci` 와 `unicode_ci` 등이 있는데, `general_ci` 는 속도를 위해서 일부를 포기한 것으로 정렬도 일반적이지 않고, 현재는 하드웨어 성능이 올라가서 둘의 차이가 유의미한 차이가 나지 않기 떄문에 `unicode_ci` 를 사용해주면 된다.

`_unicode_ci` 또한 변하였는데, Unicode 버전이 올라감에 따라 `unicode_ci` (Unicode 4.0) `_unicode_520_ci` (Unicode 5.20), `_0900_ci_ai` (Unicode 9.0)으로 변하였다.

`ci` 는 대소문자를 구분하지 않는다는 의미이고

`ai` 는 악센트를 구분하지 않는다는 의미이다.



##### 참조

https://stackoverflow.com/questions/43644218/why-is-table-charset-set-to-utf8mb4-and-collation-to-utf8mb4-unicode-520-ci/43692337

https://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci

https://www.monolune.com/what-is-the-utf8mb4_0900_ai_ci-collation/

https://dev.mysql.com/doc/refman/5.7/en/charset-connection.html
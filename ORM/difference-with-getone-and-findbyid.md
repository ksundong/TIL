# JpaRepository의 getOne 메소드와 CrudRepository의 findById 메소드의 차이

|              getOne              |                 findById                  |
| :------------------------------: | :---------------------------------------: |
|      반환 타입이 일반 타입       |           옵셔널로 래핑된 타입            |
|           Lazy Loding            |               Eager Loading               |
| EntityNotFoundException 발생가능 | orElse~를 이용한 처리를 따로 해주어야 함. |

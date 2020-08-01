# MySQL 공간데이터 저장하기

위도 경도를 어떻게 저장하는지 DECIMAL로 저장할 수 있지만, 수업내용을 살리고 싶었다.

`how to store longitude and latitude in mysql`

로 검색을 했더니, StackOverFlow에서 Spatial Data Type으로 저장하라고 했고, 이에 대해서 Honux에게 여쭤봤다.

Honux도 한 번 공부해 볼 만한 주제라고 하셨고, 그래서 `POINT` Type으로 저장을 하도록 하였다.

POINT Type은 공간 좌표에서 점을 저장하고, Real MySQL에도 굉장히 적은 정보만 있다.

MySQL에서는 공간 데이터를 R-Tree Index를 생성해서 저장할 수 있다. 인덱스를 지정하지 않으면 B-Tree인덱스로 작성된다.
공간값의 B-Tree Index는 정확한 값 조회에는 유용하지만, 범위 검색에는 유용하지 않다고 공식 매뉴얼에 적혀있다.
또한 인덱스를 생성하기 위해서는 NOT NULL 제약조건이 필수적으로 있어야 한다.

[참고](https://dev.mysql.com/doc/refman/5.7/en/creating-spatial-indexes.html)

```mysql
# Table 생성시 인덱스를 주는 방법
CREATE TABLE geom (g GEOMETRY NOT NULL, SPATIAL INDEX(g));
# Table 변경시 인덱스를 주는 방법
CREATE TABLE geom (g GEOMETRY NOT NULL);
ALTER TABLE geom ADD SPATIAL INDEX(g);
# 인덱스를 생성하는 방법
CREATE TABLE geom (g GEOMETRY NOT NULL);
CREATE SPATIAL INDEX g ON geom (g);
```

MySQL에서는 POINT형 데이터를 만드는 FUNCTION을 제공한다. `ST_GeomFromText('POINT (1 1)')` 과 같은 형식으로 공간데이터를 생성할 수 있다.

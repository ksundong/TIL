# MySQL Database와 Table Character set확인

database 명이 mydb이고 table 명이 mytable일 때, database와 table의 character set 확인하는 방법

## Schema 자체를 조회하는 방법

### database 확인 방법

```sql
SELECT default_character_set_name, DEFAULT_COLLATION_NAME
  FROM information_schema.SCHEMATA
 WHERE schema_name = "mydb";
```

### table 확인 방법

```sql
SELECT CCSA.character_set_name
  FROM information_schema.`TABLES` T,
       information_schema.`COLLATION_CHARACTER_SET_APPLICABILITY` CCSA
 WHERE CCSA.collation_name = T.table_collation
   AND T.table_schema = "mydb"
   AND T.table_name = "mytable";
```

### column 확인 방법

```sql
SELECT character_set_name
  FROM information_schema.`COLUMNS` C
 WHERE table_schema = "mydb"
   AND table_name = "mytable"
   AND column_name = "mycolumn";
```

## Show 명령어를 사용하는 방법(간단)

```sql
SHOW FULL COLUMNS FROM mytable;
```

## DB와 Table 생성문 조회

show create 구문으로 db와 table을 생성한 DDL을 확인할 수 있고, DDL에 기술된 encoding을 확인할 수 있다.

```sql
SHOW CREATE DATABASE mydb;
```

```sql
show create table mytable;
```

## 참고

- [MySQL database 와 table 의 character set 확인하는 법](https://www.lesstif.com/dbms/mysql-database-table-character-set-17105743.html)

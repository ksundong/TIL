# DB 테이블에서 변경된 데이터 추출

1. DB에서 변경된 데이터만 가져오고 싶다.

2. 쿼리를 작성해본다.

   ```SQL
   SELECT *
     FROM A
         ,B
    WHERE A.C <> B.C;
   ```

   시간이 너무 오래걸린다. INNER JOIN을 걸어야 할 것 같다.
   4,077,165개의 데이터가 나온다...

3. 다시 쿼리를 작성했다.

   ```SQL
   SELECT *
     FROM A
    INNER JOIN B
       ON A.C = B.C
      AND A.D = B.D
      AND A.E = B.E
    WHERE A.F <> B.F;
   ```

   1400개의 데이터가 나왔다.
   추가적인 조건은 이제 WHERE 절에 AND문으로 넣어주면 될 것 같다.

4. 차집합 작성

   ```SQL
   SELECT A.*
     FROM A
     LEFT JOIN B
       ON A.C = B.C
    WHERE B.C IS NULL;
   ```

   26개의 데이터가 나온다. 이전 테이블에서 추가된 내용.

5. 4번이 뭔가 오류가 있어서 다시 작성했다.

   ```SQL
   SELECT A.*
     FROM A
     LEFT JOIN B
       ON A.C = B.C
      AND A.D = B.D
      AND A.E = B.E
    WHERE B.D IS NULL;
   ```

6. 각 C별로 추가된 데이터를 가져오는 쿼리

   ```SQL
   SELECT COUNT(*)
     FROM A
     LEFT JOIN B
       ON A.C = B.C
      AND A.D = B.D
      AND A.E = B.E
    WHERE B.D IS NULL
      AND A.C = "SEARCH";
   ```

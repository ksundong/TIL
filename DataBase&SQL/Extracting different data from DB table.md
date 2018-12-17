# DB 테이블에서 변경된 데이터 추출

1. DB에서 변경된 데이터만 가져오고 싶다.

2. 쿼리를 작성해본다.

   ```SQL
   SELECT *
     FROM COMM_GRID_DET AS A
         ,COMM_GRID_DET_EXCELDOWN AS B
    WHERE A.UPD_DT <> B.UPD_DT;
   ```

   시간이 너무 오래걸린다. INNER JOIN을 걸어야 할 것 같다.
   4,077,165개의 데이터가 나온당...

3. 다시 쿼리를 작성했다.

   ```SQL
   SELECT *
     FROM COMM_GRID_DET AS A
    INNER JOIN COMM_GRID_DET_EXCELDOWN AS B
       ON A.GRID_ID = B.GRID_ID
      AND A.COL_NAME = B.COL_NAME
      AND A.PAGE_ID = B.PAGE_ID
    WHERE A.UPD_DT <> B.UPD_DT;
   ```

   1400개의 데이터가 나왔다.
   추가적인 조건은 이제 WHERE 절에 AND문으로 넣어주면 될 것 같다.

4. 차집합 작성

   ```SQL
   SELECT A.*
     FROM COMM_GRID_DET AS A
     LEFT JOIN COMM_GRID_DET_EXCELDOWN AS B
       ON A.GRID_ID = B.GRID_ID
    WHERE B.GRID_ID IS NULL;
   ```

   26개의 데이터가 나온다. 이전 그리드에서 추가된 내용.

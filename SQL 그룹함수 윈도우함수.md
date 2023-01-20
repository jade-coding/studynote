# SQL 그룹함수 윈도우함수

### 데이터 분석을 위한 함수

**윈도우 함수**

**집계 함수**

**그룹 함수**

### 윈도우 함수

순위, 집계 등 행과 행 사이의 관계를 정의하는 함수 OVER구문을 필수로 한다

SELECT WINDOW_FUNCTION(ARGUMENTS) OVER([PARTITION BY 칼럼][ORDER BY 절][WINDOWING 절] ) FROM 테이블 명;

- ARGUMENTS : 윈도우 함수에 따라서 필요한 인수

- PARTITION BY : 전체 집합에 대해 소그룹으로 나누는 기준

- ORDER BY : 소그룹에 대한 정렬 기준

- WINDOWING : 행에 대한 범위 기준

WINDOWING절에 들어가는 키워드(명령어)

   윈도우는 일종의 그룹내 범위를 정하는 프레임 정도로 이해하면 되겠습니다.

- ROWS : 물리적 단위로 행의 집합을 지정

- UNBOUNDED PRECEDING : 윈도우의 시작 위치가 첫 번째 행

- UNBOUNDED FOLLOWING : 윈도우의 마지막 위치가 마지막 행

- CURRENT ROW : 윈도우의 시작 위치가 현재 행

##### 순위함수

RANK() OVER([PARTITION BY 컬럼][ORDER BY 컬럼][WINDOWING 절]) 

- RANK : 동일한 값에는 동일한 순위를 부여

- DENSE_RANK : RANK와 같이 같은 값에는 같은 순위를 부여하나 한 건으로 취급

- ROW_NUMBER : 동일한 값이라도 고유한 순위를 부여

```sql
SELECT ID, NAME, SALARY, 
RANK() OVER (ORDER BY SALARY DESC) RANK,
DENSE_RANK() OVER (ORDER BY SALARY DESC) DENSED_RANKING,
ROW_NUMBER() OVER (ORDER BY SALARY DESC) ROW_NUMBER
FROM EMPLOYEE;
```

<img src="file:///C:/Users/user/Desktop/Coding/studynote/SQL순위함수.PNG" title="" alt="" width="571">

```sql
--- EXAMPLE ---

SELECT 
  MEMBER_ID,
  SQUAT,
  BENCH_PRESS,
  DEADLIFT,
 (SQUAT+BENCH_PRESS+DEADLIFT) AS WEIGHT_SUM,
 RANK() OVER(ORDER BY WEIGHT_SUM DESC) RANK
FROM GYM_MEMBER;
```

##### 일반 집계 함수

일반 집계 함수(SUM, AVG, MAX, MIN, ...)를 GROUP BY 구문없이 사용할 수 있음

```sql
SELECT 
    ID, NAME, SALARY, DEPARTMENT_ID,
    (SELECT AVG(SALARY
    FROM EMPLOYEE B
    WHERE B.DEPARTMENT_ID = A.DEPARTMENT_ID) AS DEPARTMENT_AVG
FROM EMPLOYEE A
ORDER BY A.DEPARTMENT_ID;


SELECT ID,NAME,SALARY,AVG(SALARY) 
    OVER (PARTITION BY DEPARTMENT_ID) DEPARTMENT_AVG
FROM EMPLOYEE;
```

<img src="file:///C:/Users/user/Desktop/Coding/studynote/SQL일반집계함수.PNG" title="" alt="" width="567">

```sql
--- EXAMPLE ---
SELECT SELL_ID 판매,SELLER_NAME '판매자 이름',PRODUCT_NAME '상품 이름', QUANTITY '수량', SUM(PRICE*QUANTITY) OVER(PARTITION BY SELLER_NAME,SELL.PRODUCT_ID) '판매 금액'
FROM SELL 
INNER JOIN PRODUCT
ON SELL.PRODUCT_ID = PRODUCT.PRODUCT_ID
ORDER BY SELL_ID ASC;
```

##### 그룹 내 행 순서 함수

- FIRST_VALUE : 가장 먼저 나온 값을 구한다

- LAST_VALUE : 가장 나중에 나온 값을 구한다

- LAG : 이전 X 번째 행을 가져온다

- LEAD : 이후 X 번째 행을 가져온다

```sql
--- 각 부서에서 급여 많이 받는 사람의 급여과 적게 받는 사람의 급여를 출력
SELECT
    ID, DEPARTMENT_ID, NAME, SALARY,
    FIRST_VALUE(SALARY) OVER(PARTITION BY DEPARTMENT_ID 
    ORDER BY  SALARY
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) 
    DEPARTMENT_MINS_SALARY,
    LAST_VALUE(SALARY) OVER(PARTITION BY DEPARTMENT_ID 
    ORDER BY SALARY
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
    DEPARTMENT_MAX_SALARY
FROM EMPLOYEE
ORDER BY ID;
```

##### LAG,LEAD

```sql
--- 본인 사원 번호 앞과 뒤에 해당하는 직원의 이름을 함께 출력---
SELECT 
    ID,
    NAME,
    SALARY,
    LAG(NAME,1) OVER(ORDER BY ID) PREV_EMPLOYEE_NAME,
    LEAD(NAME,1) OVER(ORDER BY ID) AFTER_EMPLOYEE_NAME
FROM EMPLOYEE;
```

##### 그룹 내 비율함수

- RATIO_TO_REPORT : 파티션 내 전체 SUM에 대한 비율을 구한다

- PERCENT_RANK : 파티션 내 순위를 백분율로 구한다

- CUME_DIST : 파티션 내 현재 행보다 작거나 같은 건들의 수 누적 백분율로 구한다

- NTILE : 파티션 내 행들을 N등분한 결과를 구한다



```sql
--- 직원 전체 급여의 합 중 각 행이 차지하는 비율을 출력 ---

SELECT ID, NAME, SALARY,
    SUM(SALARY) OVER() TOTAL_SALARY,
    RATIO_TO_REPORT(SALARY) OVER() RATIO_TO_REPORT
FROM EMPLOYEE;
```

```sql

SELECT ID,USE_CODE, EXPENSE,    
 ROUND((EXPENSE / SUM(EXPENSE) OVER())*100,4) AS RAT
FROM BUDGET_USE
ORDER BY ID ASC;
```



MARIA DB에서는 제공되지 않기에 명시적으로 구현가능합니다

> **(SALARY/ SUM(SALARY) OVER()) RATIO_TO_REPORT**



##### PERCENT_RANK, CUME_DIST

PERCENT_RANK는 순위를 백문율로 나타내며 제일 높은 순위 행은 0, 가장 낮은 행은 1을 가진다 - 0이상 1이하의 값 반환

CUME_DIST는 현재 행보다 같거나 낮은 값들을 가지는 행들의 누적 백분율 값을 나타낸다 - 0초과 1이하의 값 반환



둘다 Argument가 따로 오지는 않습니다!



```sql
SELECT ID,NAME,SALARY,
PERCENT_RANK() OVER(ORDER BY SALARY DESC)
AS PERCENT_RANK,
ROUND(CUME_DIST() OVER(ORDER BY SALARY DESC),4)
AS CUME_DIST
FROM EMPLOYEE;
```

##### NTILE

```SQL
--- 급여에 따라 직원들을 세 그룹으로 분류 ---
SELECT ID, NAME, SALARY, NTILE(3) OVER(ORDER BY SALARY DESC) 
AS NTILE
FROM EMPLOYEE;
```

(N)개의 그룹으로 나눕니다. 

추가로 데이터들이 들어오면 NTILE은 몇번으로 넣어줘야할까?

-> 들어가는 순서의 순열에 맞춰서 들어갑니다 



EX)

```sql
SELECT ID, MATH,PHYSICS,CHEMISTRY, 
(MATH+PHYSICS+CHEMISTRY) AS SCORE_SUM,
PERCENT_RANK() OVER (ORDER BY (MATH+PHYSICS+CHEMISTRY) DESC)
 AS PERCENT_RANK,
CUME_DIST() OVER(ORDER BY (MATH+PHYSICS+CHEMISTRY) DESC)
 AS CUME_DIST
FROM STUDENT
ORDER BY SCORE_SUM DESC;

SELECT ID, MATH,PHYSICS, CHEMISTRY,
 (MATH+PHYSICS+CHEMISTRY) AS SCORE_SUM,
 NTILE(3) OVER(ORDER BY SCORE_SUM DESC) NTILE
FROM STUDENT
ORDER BY ID ASC;


```

### 그룹함수

데이터를 통계 내기 위해서는, 전체 데이터에 대한 통계는 물론이고 데이터 일부에 대한 소계 중계 또한 필요하다.

각 레벨 별 SQL을 UNION문으로 묶어 작성할수도 있으나 ORACLE DB에서는 이러한 통계 데이터를 위한 몇가지 함수를 제공한다

##### 그룹함수 - 기반 테이블

##### GROUP BY

```sql
SELECT 
D.NAME AS DEPARTMENT_NAME,
J.NAME AS JOB_NAME,
AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN DEPARTMENT D
ON E.DEPARTMENT_ID = D.ID
JOIN JOB J
ON F.JON_ID
GROUP BY
D.NAME,J.NAME
ORDER BY D.NAME, J.NAME;



```

EX)

```sql
SELECT KIND,CATEGORY,SUM(SELL_COUNT) AS SL
FROM BOOK_HISTORY
GROUP BY KIND,CATEGORY
ORDER BY SL ASC;
```

##### ROLL UP

그룹화 하는 컬럼에 대한 부분적인 통계를 제공해준다

그룹화 된 칼럼에 대한 TOTAL 결과를 받는 것



```sql
SELECT 
    D.NAME AS DEPARTMENT_NAME
    J.NAME AS JOB_NAME,
    AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN DEPARTMENT D
ON E.DEPARTMENT_ID = D.ID
JOIN JOB J
ON E.JON_ID = J.ID
GROUP BY ROLLUP(D.NAME, J.NAME);
```

**위는 ORACLE 방식**



**아래는 MARIA DB에서는 다른 방식을 사용**

```sql
SELECT 
    D.NAME AS DEPARTMENT_NAME
    J.NAME AS JOB_NAME,
    AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN DEPARTMENT D
ON E.DEPARTMENT_ID = D.ID
JOIN JOB J
ON E.JON_ID = J.ID
GROUP BY D.NAME, J.NAME WITH ROLLUP;
```

##### CUBE

ROLLUP 함수에서 제공하는 결과를 포함해서 CUBE 함수에서는 그룹화 하는 컬럼에 대해 결합 가능한 모든 경우의 수에 대해 다차원 집계를 생성한다

-> ROLLUP 함수들의 합으로 보면 됩니다

> **순서만 다르게 했을 때의 ROLL UP(A.NAME, B.NAME) ROLL UP(B,NAME, A.NAME)의 결과가 달랐는데 이것을 합쳐주는 것이 CUBE 입니다.**



```sql
SELECT 
D.NAME AS DEPARTMENT_NAME,
J.NAME AS JOB_NAME,
AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.DEPARTMENT_ID = D.ID
JOIN JOB J ON E.JOB_ID = J.ID
GROUP BY CUBE(D.NAME, J.NAME);
```

<img src="file:///C:/Users/user/Desktop/Coding/studynote/CUBE%20함수.PNG" title="" alt="" width="572">



Maria DB에서는 WITH CUBE 지원을 안하기에

**WITH ROLLUP UNION을 이용해서 적용합니다.**





EX)

```sql
SELECT KIND, CATEGORY, SUM(SELL_COUNT)
FROM BOOK_HISTORY
GROUP BY KIND ,CATEGORY WITH ROLLUP;
```



#### GROUPING SET

명시된 컬럼에 대해 개별 통계를 생성한다

각 컬럼에 대해 GROUP BY로 생성한 통계를 모두 UNION ALL한 결과와 동일하다

```sql
SELECT 
D.NAME AS DEPARTMENT_NAME,
J.NAME AS JOB_NAME,
AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.DEPARTMENT_ID = D.ID
JOIN JOB J ON E.JOB_ID = J.ID
GROUP BY GROUPING SETS(D.NAME, J.NAME);
```

![](C:\Users\user\Desktop\Coding\studynote\GROUPINGSETS함수.PNG)



Maria DB에서는 UNION ALL로 합쳐줍니다. 

사실 중복되는 값은 없을 거기에 UNION으로 큰 상관은 없을 겁니다.

그래도 cube와의 구분을 위해서 UNION ALL을 사용합니다.



```sql
SELECT 
D.NAME AS DEPARTMENT_NAME,
NULL,
AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN DEPARTMENT D ON D.ID = E.DEPARTMENT_ID
GROUP BY D.NAME
UNION ALL
SELECT
NULL, J.NAME AS JOB_NAME,
AVG(E.SALARY) AS AVG_SALARY
FROM EMPLOYEE E
JOIN JOB J ON J.ID = E.JOB_ID
GROUP BY J.NAME;
```



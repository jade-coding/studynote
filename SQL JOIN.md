# SQL JOIN

### 

### JOIN

두개 이상의 테이블들을 **연결** 또는 **결합**하여 데이터를 출력하는 것

- 연산자에 따라 JOIN 방식 분류시
  
  - **EQUI JOIN** VS **Non Equi JOIN**

#### EQUI JOIN(등가 교집합)

두 개의 테이블 간에 서로 정확하게 일치하는 경우를 활용하는 조인. 즉, 등가 연산자(=)를 사용한 조인을 의미 / 대부분 기본키-외래키 관계를 기반으로 발생하나, 모든 조인이 그런 것은 아님 / 실무에서 많이 사용함

#### Non EQUI JOIN(비등가 교집합)

두 개의 테이블 간에 서로 정확하게 일치하지 않는 경우를 활용하는 조인. 즉, 등가 연산자 이외의 연산자(>, >=, <=, BETWEEN)연산자들을 사용하는 조인을 의미

#### INNER JOIN

내부 JOIN이라고도 하며 JOIN 조건에서 동일한 값이 있는 행만 반환

INNER JOIN은 JOIN의 기본값으로 'INNER' 생략 가능

```sql
SELECT * FROM 테이블1 [INNER] JOIN 테이블2

ON 테이블1.컬럼명 = 테이블2.컬럼명;
```

* WITH RECURSIVE CTE에 대해서도 인지할 것!

##### USING 조건절

같은 이름을 가진 컬럼들 중 원하는 칼럼에 대해서만 선택적으로 등가 조인 가능 / SQL Server에서는 지원X / innerJoin보다 좀 더 간편함

```sql
SELECT * FROM 테이블1 JOIN 테이블2
USING(기준칼)
```

**!! USING 조건절 사용시에는 칼럼이나 테이블에 별칭을 붙일 수 없음**

##### NATURAL JOIN

두 테이블 간의 동일한 이름을 갖는 모든 칼럼들에 대해 등가조인을 수행 / Using 절보다 더 간편함

```sql
SELECT * FROM 테이블1 NATURAL JOIN 테이블2;
```

**!! 추가로 ON 조건절이나 USING 조건절, WHERE절에서 JOIN 조건 정의 불가!!**




!! 신기한 것

```sql
SELECT REQUEST_ID,STUDENT_NAME,CLASS_NAME

FROM STUDENT

JOIN (

    SELECT *

    FROM CLASS_REQUEST

    JOIN CLASS

    USING(CLASS_ID)

) as a

--- 서브쿼리여서 앨리어스 지정해야함. ---

USING(STUDENT_ID);

--- 위와 아래가 같음 ---

SELECT REQUEST_ID, STUDENT_NAME, CLASS_NAME

FROM CLASS_REQUEST

INNER JOIN STUDENT USING(STUDENT_ID)

INNER JOIN CLASS USING(CLASS_ID);
```



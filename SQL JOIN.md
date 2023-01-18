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
- WHERE를 추가할 수 있음 JOIN이 다 되어서 나온 테이블에 WHERE를 적용
  
  > ```sql
  > SELECT * FROM 테이블1 [INNER] JOIN 테이블2
  > ON 테이블1.컬럼명 = 테이블2.컬럼명;
  > WHERE [조건];
  > ```

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



#### CROSS JOIN(Cartesian Product)

JOIN 조건이 없는 경우 생길 수 있는 **모든 데이터의 조합**을 조회

```sql
SELECT * FROM PERSON

(CROSS) JOIN PUBLIC_TRANSPORT;

--- INNER도 CROSS처럼 생략이 가능하다만 ON이 있어야 하므로 ---
--- 그게 없으면 CROSS JOIN으로 보는 것 ---
```



#### OUTER JOIN

두 개의 테이블 간에 교집합을 조회하고 한 쪽 테이블에만 있는 데이터도 포함시켜 조회

-> 빈 곳은 NULL 값으로 출력

-> WHERE 절에서 한쪽에만 있는 데이터를 포함시킬 테이블 반대쪽으로(+)를 위치 - (Oracle DB 에서 쓰는 방법) 

> ```sql
> > SELECT * FROM USER, CLASS
> > WHERE USER.CLASS_ID = CLASS.CLASS_ID(+);
> ```
> 
> 집어넣을 테이블을 +로 표시한다고 생각하면 쉽습니다.



```sql
SELECT * FROM USER LEFT [OUTER] JOIN CLASS
ON USER.CLASS_ID = CLASS.CLASS_ID;


SELECT * FROM USER RIGHT [OUTER] JOIN CLASS
ON USER.CLASS_ID = CLASS.CLASS_ID;
```

<mark>OUTER를 생략할 수 있지만 가시적으로 보이도록 써주는게 일반적입니다</mark>

> ##### FULL OUTER JOIN이라는 것도 존재함
> 
> ORACLE DB에서는 가능하나 Maria DB에는 존재하지 않음
> 
> >  **LEFT OUTER JOIN과 RIGHT OUTER JOIN 을 UNION을 하는 방법으로 해결을 할 수 있음**
> 
> ```sql
> SELECT * FROM CLASS FULL OUTER JOIN USER
> ON USER.CLASS_ID = CLASS.CLASS_ID;
> ```



EX)

```sql
SELECT *
FROM 
(
SELECT FRONT_VERSION_HIST.version_id as version_id , version_content_front, version_content_back
FROM FRONT_VERSION_HIST LEFT JOIN BACK_VERSION_HIST
ON FRONT_VERSION_HIST.version_id = BACK_VERSION_HIST.version_id
UNION
SELECT BACK_VERSION_HIST.version_id as version_id , version_content_front, version_content_back
FROM FRONT_VERSION_HIST RIGHT JOIN BACK_VERSION_HIST
ON FRONT_VERSION_HIST.version_id = BACK_VERSION_HIST.version_id
)as UNI
ORDER BY version_id ASC;

--- 위와 아래 동일
SELECT FRONT_VERSION_HIST.version_id, version_content_front, version_content_back
FROM FRONT_VERSION_HIST LEFT JOIN BACK_VERSION_HIST
ON FRONT_VERSION_HIST.version_id = BACK_VERSION_HIST.version_id
UNION
SELECT BACK_VERSION_HIST.version_id , version_content_front, version_content_back
FROM FRONT_VERSION_HIST RIGHT JOIN BACK_VERSION_HIST
ON FRONT_VERSION_HIST.version_id = BACK_VERSION_HIST.version_id
ORDER BY version_id ASC;
```



#### 셀프조인

**동일 테이블 사이의 조인**

동일 테이블 사이의 조인을 수행하면 테이블과 칼럼이름이 모두 동일하므로 식별하려면 별칭(얼리어스) 지정이 필수적이다



```sql
SELECT ALPHA.칼럼명, BETA.칼럼명
FROM 테이블1 ALPHA, 테이블1 BETA
WHERE ALPHA.컬렴명2 = BETA.컬럼
```

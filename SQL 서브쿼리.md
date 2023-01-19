# 서브쿼리 심화

### 동작하는 방식에 따른 서브쿼리 분류

서브쿼리에 메인쿼리의 컬럼이 포함되는지로 구분합니다

##### 연관 서브쿼리(Correlated Subquery)

메인쿼리의 컬럼이 서브쿼리에 포함되며, 메인 쿼리의 컬럼은 서브쿼리에 특정 조건으로 사용된다.

>  SELECT *
> 
> FROM A
> 
> WHERE A.0 > (SUB QUERY)

EX

```sql
SELECT ID, DEPARTMENT_ID, NAME, SALARY
FROM EMPLOYEE A
WHERE SALARY > (SELECT
 AVG(SALARY)
FROM EMPLOYEE B
WHERE B.DEPARTMENT_ID = A.DEPARTMENT_ID);
```

##### 비연관 서브 쿼리(Un-Correlated Subquery)

메인쿼리 컬럼이 서브쿼리에 포함되지 않으며, 주로 메인 쿼리에 특정한 값을 제공할 때 사용된다.

EX

```sql
SELECT AVG(SALARY)
FROM EMPLOYEE
WHERE DEPARTMENT_ID = (SELECT 
DEPARTMENT_ID
FROM EMPLOYEE
WHERE NAME = 'ELICE');
```

### 반환되는 데이터 형태에 따른 서브쿼리 분류

##### 단일 행 서브쿼리

서브쿼리의 결과가 한 개의 행을 반환하며 단일 행 비교 연산자(= , > , >= ...)와 같이 사용된다.

a.salary = 1 처럼 단일의 값이 들어가야 한다.

```sql
SELECT AVG(SALARY)
FROM EMPLOYEE
WHERE DEPARTMENT_ID = (SELECT ---단일값 ID가 들어 ---
DEPARTMENT_ID
FROM EMPLOYEE
WHERE NAME = 'ELICE');
```

##### 다중 행 서브쿼리

서브쿼리의 겨로가가 두개 이상 행을 반환할 수 있으며,

다중 행 비교 연산자(IN, ALL, ANY, EXISTS)와 같이 사용된다.

> IN : 서브쿼리 결과에 존재하는 값들 중 하나와 일치해야 한다.
> 
> EXISTS : 서브쿼리 결과 값이 존재하는지(NULL OR NOT NULL) 여부를 확인한다. 
> 
> ALL : 서브쿼리 결과에 존재하는 모든 값들에 대해 조건을 만족해야 한다.
> 
> ANY : 서브쿼리 결과에 존재하는 값들 중 조건을 만족하는 것이 존재해야 한다.

- IN 연산자

```sql
--- 영업 또는 개발팀에 속한 직원들을 출력 ---
SELECT NAME
FROM EMPLOYEE
WHERE DEPARTMENT_ID IN(
    SELECT ID
    FROM DEPARTMENT
    WHERE NAME='품질' OR NAME = '영업'
);
```

- EXISTS 연산자
  
  서브쿼리의 결과가 존재하는가 아닌가를 따짐

```sql
--- 급여가 10000을 넘는 직원이 존재하는 부서에 소속된 모든 직원들을 출력---
SELECT NAME
FROM EMPLOYEE A
WHERE EXISTS (
SELECT ID --- ID를 쓰든 쓰레기값을 쓰든 상관없음 EXIST 한지만 따짐 ---
FROM EMPLOYEE B
WHERE B.SALARY >= 10000 AND
A.DEPARTMENT_ID = B.DEPARTMENT_ID
);
```

- ALL 연산자
  
  조건에 모두 만족해야 함.

```sql
--- 개발 팀 소속 모든 직원들 급여보다 급여가 큰 직원들을 출력 ---
SELECT NAME
FROM EMPLOYEE
WHERE SALARY >= ALL(SELECT SALARY
FROM EMPLOYEE
WHERE DEPARTMENT_ID = 1);
```

- ANY 연산자

조건에 한 개라도 충족하는지를 판단

```sql
--- 개발 팀 소속 임의의 직원들 급여보다 급여가 큰 직원들을 출력 ---
SELECT NAME
FROM EMPLOYEE
WHERE
SALARY >= ANY(SELECT SALARY
FORM EMPLOYEE
WHERE DEPARTMENT_ID = 1)
```

##### 다중 컬럼 서브쿼리

서브쿼리의 결과가 여러 개의 컬럼을 반환하며 메인 쿼리의 조건과 동시에 비교된다

```sql
---각부서에서가장높은급여를받는직원의이름과급여를출력---
SELECT NAME, SALARY
FROM EMPLOYEE
WHERE(DEPARTMENT_ID, SALARY) 
IN(
SELECT DEPARTMENT_ID, MAX(SALARY) 
FROM EMPLOYEE 
GROUP BY DEPARTMENT_ID);
```

##### 스칼라 서브쿼리

스칼라서브쿼리는하나의속성을가지면서,하나의행만을반환하는쿼리이다.그리고이는SELECT, WHERE, HAVING 절등에서사용할수있다.

```sql
---부서명과부서의구성원수를출력한다---
SELECTNAME, (
SELECT COUNT(*) 
FROM EMPLOYEE E 
WHERE E.DEPARTMENT_ID = D.ID)
FROM DEPARTMENT D;
```

스칼라서브쿼리예시-DUAL

FROM을 사용한 특정 테이블 참조없이 쿼리를 작성하는경우도있다 

```sql
--- 전체임직원중에팀장직급이차지하는비율을출력한다 ---
SELECT(SELECTCOUNT(*) 
FROM EMPLOYEE 
WHERE DEPARTMENT_ID = 1) / 
(SELECT COUNT(*) FROM EMPLOYEE)
AS DEVELOPER_RATIO
FROM DUAL;
```



### 뷰

뷰는 다른 테이블에서 파생된 테이블이다.물리적으로데이터가 저장되는 것이아니라, 논리적으로만 존재하며 뷰를 사용한 질의시에는 DBMS에서 뷰 정의에 따라 질의를 재작성하여 수행한다

- 독립성 : 테이블 구조가 변경되어도 뷰를 사용하고 있는 응용 프로그램은 변경하지 않아도 된다

- 편리성 : 자주 사용되는복잡한 쿼리를 미리 뷰로 정의해놓으면, 추후 쿼리는 간단한 형태로 표현 할 수있다

- 보안성 : 사용자의권한에따라 열람가능한 데이터를 다르게 할 수 있다 권한에 따라 확인가능한 컬럼을 정의하여 뷰를 생성하면, 기본 테이블 노출 없이 접근 제어를할 수 있다

##### View의 특징

- 생성된뷰는 또 다른뷰를 생성하는데 사용될 수 있다

- 뷰의정의는 변경할 수 없으며, 삭제 후 재 생성이 필요하다

- 뷰를 통한 갱신에는 제약이따른다. 갱신을 위해서는기본적으로 원천 테이블의 기본 키가포함 되어야 한다

- 원천이 되는 테이블이나 뷰가 삭제되면 이를 기반으로 하는 뷰도 함께 삭제된다

```sql
CREATE [OR REPLACE] VIEW view_name AS
SELECT col1, col2, ...
FROM table_name
WHERE condition;


EX)
CREATE VIEW EMPLOYEE_FULL AS
(SELECTE.ID AS EMPLOYEE_ID, 
E.NAME ASEMPLOYEE_NAME, 
E.SALARY, 
E.DEPARTMENT_ID,
D.NAME ASDEPARTMENT_NAME
FROM EMPLOYEEE LEFT OUTER JOIN DEPARTMENT D 
ON E.DEPARTMENT_ID = D.ID);
```

REPLACE : 같은 이름의 뷰가 존재하면 기존 뷰를 무시하고 대체



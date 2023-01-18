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





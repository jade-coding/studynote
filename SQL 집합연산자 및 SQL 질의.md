# SQL : 집합연산자 및 SQL 질의



### Standard SQL

 관계형 대수라고도 불리는 Standard SQL은 관계형 데이터베이스에서 원하는 정보를 유도하기 위한 기본 연산 집합을 말합니다. 이는 **일반 집합 연산**과 **순수 관계 연산**을 포함합니다.

#### 일반 집합 연산

- 합집합 : UNION

- 교집합 : INTERSECT

- 차집합 : EXCEPT

- 카디션 프로덕트 : JOIN



#### 순수 관계 연산

- 셀렉션 : WHERE 절

- 프로젝션 : SELECT절

- 조인 : 다양한 JOIN

- 디비전 : 실제 구현된 것은 없음. 추상적인 개념임.



#### 집합연산자

UNION, UNION ALL, INTERSECT, EXCEPT

- UNION : 합집합 / 중복제거, 정렬

- UNION : 합집합 / 중복제거X, 정렬 X

- INTERSECT : 교집합 / Oracle, Maria DB는 가능 MySql은 불가능

- EXCEPT : 차집합 / Oracle은 Minus 키워드로 가능, MariaDB는 10.3 ver부터 가능 MySql은 불가능

    **MySql에서 지원되지 않는 것들은 Join연산을 활용해야 함**



### 계층형 질의

Oracle, SQL server, MariaDB, MySql 다 구현에는 차이가 있음

<img title="" src="file:///C:/Users/user/Desktop/Coding/studynote/계층형질의.PNG" alt="" width="593" data-align="center">

SQL Server version.2000이전

-> 저장 프로시저를 재귀 호출 / While 루프 문에서 임시테이블 사용

SQL Server version.2005이후

-> CTE(Common Table Expression)을 이용하여 재귀 호출 / Maria와 거의 동일



MySQL, MariaDB는 특별버전 이후 CTE를 이용한 재귀 호출

실무에서 엄청 응용되는 것은 아니지만 어떠한 개념이고 사용하는지를 아는 것이 중요









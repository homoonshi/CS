# JOIN
- 한 데이터베이스 내의 여러 테이블의 레코드를 조합해 하나의 열로 표현
- 각 테이블에 저장된 데이터를 효과적으로 검색하기 위해 필요
### 필요성
- 관계형 데이터베이스의 구조적 특징으로 정규화를 수행하면 의미 있는 데이터의 집합으로 테이블이 구성되고, 각 테이블 끼리는 관계를 갖게 됨
- 이와 같은 특징으로 관계형 데이터베이스는 저장 공간의 효율성과 확장성이 향상
- 다른 한편으론 서로 관계 있는 데이터가 여러 테이블로 나뉘어 저장되므로, 각 테이블에 저장된 데이터를 효과적으로 검색하기 위해 필요
### 종류
#### 내부 조인
- 가장 기본적인 조인
- 두 테이블에서 "서로 일치하는 값"이 있는 행만 보여주는 조인
- 명시적 조인 표현(explicit), 암시적 조인 표현(implicit) 2개의 다른 조인식 구문이 있음
- 명시적 조인 표현
    - 테이블에 조인을 하라는 것을 지정하기 위해 JOIN 키워드를 사용
    - 그 후 ON 키워드를 조인에 대한 구문을 지정하는데 사용함
    ```SQL
    SELECT *
    FROM employee INNER JOIN department
    ON employee.DepartmentID = department.DepartmentID;
    ```
- 암시적 조인 표현
    - SELECT 구문의 FROM 절에서 분리하는 컴마(,) 를 사용해 단순히 조인을 위한 여러 테이블을 나열만 함
    ```SQL
    SELECT *
    FROM employee, department
    WHERE employee.DepartmentID = department.DepartmentID;
    ```
##### (1) 동등 조인 (EQUI JOIN)
- 두 테이블의 공통된 컬럼을 기준으로 `ON`이나 `WHERE`절을 써서 조인
- 조건은 = 로만 비교
```SQL
SELECT *
FROM 직원
JOIN 부서 ON 직원.부서ID = 부서.부서ID;
```
##### (2) 자연 조인 (NATURAL JOIN)
- 공통된 컬럼이 있으면 자동으로 그걸 기준으로 조인해주는 방식
- `ON` 조건을 안써도 됨
- 같은 이름 컬럼이 있을 때 그걸 기준으로 동등 조인 해줌
- 중복된 컬럼은 한 번만 나옴
```SQL
SELECT *
FROM 직원
NATURAL JOIN 부서;
```
##### (3) 교차 조인 (CROSS JOIN)
- 모든 조합을 다 보여주는 조인
- 두 테이블의 모든 행을 곱집합으로 조합
- 모든 행 X 모든 행이 나옴
```SQL
SELECT *
FROM 직원
CROSS JOIN 부서;
```
### 외부 조인 
- 두 테이블 중 하나라도 일치하는 값이 있으면 포함
- 일치하지 않으면 NULL 값으로 채워서 보여줌
##### (1) LEFT OUTER JOIN
- 왼쪽 테이블 기준, 오른쪽에 없으면 NULL
```SQL
SELECT *
FROM 직원
LEFT JOIN 부서 ON 직원.부서ID = 부서.부서ID;
```
##### (2) RIGHT OUTER JOIN
- 오른쪽 테이블 기준, 왼쪽에 없으면 NULL
```SQL
SELECT *
FROM 직원
RIGHT JOIN 부서 ON 직원.부서ID = 부서.부서ID;
```
##### (3) FULL OUTER JOIN
- 양쪽 다 포함, 어느 한 쪽이라도 없으면 NULL로 채움
```SQL
SELECT *
FROM 직원
FULL OUTER JOIN 부서 ON 직원.부서ID = 부서.부서ID;
```
### 셀프 조인 (SELF JOIN)
- 자기 자신과 조인하는 것
- 테이블을 두 개처럼 별칭(alias)를 붙여서 사용
- 같은 테이블 안에서의 관계를 표현할 때 사용
```SQL
SELECT A.이름 AS 직원이름, B.이름 AS 상사이름
FROM 직원 A
LEFT JOIN 직원 B ON A.상사ID = B.직원ID;
```
##### 예시 1 댓글 - 대댓글 구조
```SQL
SELECT 자식.내용 AS 대댓글, 부모.내용 AS 원댓글
FROM 댓글 자식
LEFT JOIN 댓글 부모 ON 자식.부모댓글ID = 부모.댓글ID;
```
##### 예시 2 카테고리 - 상위 카테고리
```SQL
SELECT 하위.이름 AS 하위카테고리, 상위.이름 AS 상위카테고리
FROM 카테고리 하위
LEFT JOIN 카테고리 상위 ON 하위.상위카테고리ID = 상위.카테고리ID;
```
##### 예시 3 조직도 (직원-상사)
```SQL
SELECT 부하.이름 AS 직원, 상사.이름 AS 상사
FROM 직원 부하
LEFT JOIN 직원 상사 ON 부하.상사ID = 상사.직원ID;
```
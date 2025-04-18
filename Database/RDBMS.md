# RDBMS
- 관계형 데이터베이스
- 데이터를 테이블 형식으로 저장
### 역할
- 데이터 저장
- 데이터 조회
- 데이터 수정/삭제
- 무결성 제약 유지
- 트랜잭션 처리
- 성능 최적화
- 동시 접속 처리
- 백업 & 복구
### 구성 요소
```SQL
┌────────────────────────────┐
│         RDBMS              │
├────────────────────────────┤
│  SQL 파서 (Parser)         │ ← SQL 해석
│  옵티마이저 (Optimizer)     │ ← 쿼리 실행계획 최적화
│  쿼리 실행기 (Executor)     │ ← 실제 실행
│  버퍼/캐시 매니저           │ ← 데이터 읽기/쓰기 관리
│  트랜잭션/락 매니저          │ ← 동시성 제어
│  저장 엔진 (Storage Engine) │ ← 파일로 저장
└────────────────────────────┘

```
### 용어 설명
##### (1) 테이블
- DB에서 데이터를 저장하는 기본 단위
- 엑셀의 시트(sheet)처럼 생김
##### (2) 열 (Column, 속성)
- 테이블의 세로 방향, 하나의 속성
- 컬럼마다 자료형이 정해져 있음
    - INT
    - VARCHAR
    - DATE
##### (3) 행 (Row, 튜플, 레코드)
- 테이블의 가로 한 줄
- 실제 데이터 한 개
- 하나의 행은 하나의 엔티티(개체)를 나타냄
##### (4) 기본키
- 각 행을 유일하게 식별하는 컬럼
- NULL 안됨, 중복 X
##### (5) 외래키
- 다른 테이블의 기본키를 참조하는 컬럼
- 테이블 간의 관계를 맺어줌
##### (6) 제약조건
- 데이터 무결성을 지키기 위한 제한 조건
##### (7) 스키마
- 데이터베이스 안에서 테이블 구조, 관계, 제약조건 등을 정의한 것
- DBMS에서 사용자/이름공간 단위로 스키마를 나누는 경우도 있음
##### (8) 인덱스
- 빠른 검색을 위한 목차
- WHERE, JOIN, ORDER BY 등에 성능 향상
### RDBMS 종류
|RDBMS|특징|
|--|--|
|MySQL|오픈소스, 빠르고 가볍고 인기 많음, 웹 백엔드에 많이 사용|
|PostgreSQL|오픈소스, 기능이 풍부하고 안정성 높음, 복잡한 쿼리에 강함|
|Oracle| 대규모 기업용, 안정성/보안/트랜잭션에 강함|
|SQL Server| 마이크로소프트 제품, .NET 환경에서 많이 사용|
|MariaDB|MySQL에서 파생된 오픈소스, 호환성 높음|
|SQLite|임베디드용, 앱/모바일에 내장 가능|
|MSSQL|대기업 ERP 시스템, Azure 클라우드 환경|

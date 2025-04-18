# Transaction (트랜잭션)
- 데이터베이스의 상태를 변경시키는 작업의 단위
### 트랜잭션의 성질 (ACID)
#### 1. Atomicity (원자성)
- 트랜잭션의 연산은 모두 반영되어야하며, 하나라도 실패하면 모두 취소되어야함
#### 2. Consistency (일관성)
- 트랜잭션을 성공하면 언제나 일관성 있는 데이터베이스 상태로 변화함
#### 3. Isolation (독립성, 격리성)
- 둘 이상의 트랜잭션이 동시에 수행되는 경우 다른 트랜잭션 연산에 끼어들 수 없음
#### 4. Durability (지속성)
- 완료된 트랜잭션은 영구적으로 반영되어야함
### 트랜잭션의 상태
- 활동 (Active)
    - 트랜잭션이 실행 중에 있는 상태
    - 연산들이 정상적으로 실행 중
- 장애 (Failed)
    - 트랜잭션이 실행에 오류가 발생해 중단
- 철회 (Aborted)
    - 트랜잭션이 비정상적으로 종료되어 Rollback 연산을 수행한 상태
- 부분 완료 (Partially Committed)
    - 트랜잭션이 마지막 연산까지 실행했지만, Commit 연산이 실행되기 직전 상태
- 완료 (Committed)
    - 트랜잭션이 성공적으로 종료되어 Commit 연산을 실행한 후의 상태
### 트랜잭션 격리 수준
- 일관성이 없는 데이터를 허용하도록 하는 수준
- 동시성 제어를 위해 사용되는 개념
#### 1. 필요성
- 데이터베이스는 ACID 같이 트랜잭션이 원자적이면서도 독립적인 수행을 하도록 함
- Locking 이라는 개념 등장
    - 트랜잭션이 DB를 다루는 동안 다른 트랜잭션이 관여하지 못하게 막는 것
- 무조건적인 Locking 으로 동시에 수행되는 많은 트랜잭션들을 순서대로 처리하는 방식으로 구현되면 DB 성능이 떨어짐
- 응답성을 높이기 위해 Locking 범위를 줄이면 잘못된 값이 처리될 여지가 있음
#### 2. Level 종류
##### (1) Read Uncommitted (커밋되지 않은 읽기)
- 다른 트랜잭션이 아직 커밋하지 않은 변경 내용을 읽을 수 있음
- 가장 낮은 수준의 격리
- 성능은 좋으나 데이터 정합성 보장 X
- 발생 가능한 문제
    - Dirty Read : 커밋되지 않은 데이터를 읽음
    - Non-Repeatable Read
    - Phantom Read
##### (2) Read Committed (커밋된 읽기)
- 커밋된 데이터만 읽을 수 있음
- 대부분의 DBMS 기본 값
- Dirty Read는 방지하지만 같은 트랜잭션 안에서 같은 조건으로 조회해도 결과가 바뀔 수 있음
- 발생 가능한 문제
    - Non-Repeatable Read
    - Phantom Read
##### (3) Repeatable Read (반복 가능한 읽기)
- 트랜잭션 내에서 같은 조건으로 SELECT 하면 항상 같은 결과 반환
- 읽은 데이터가 다른 트랜잭션에 의해 수정되는 것을 막음
- InnoDB는 Phantom Read도 방지
- 발생 가능한 문제
    - Phantom Read
##### (4) Serializable (직렬화 가능)
- 모든 트랜잭션을 순차적으로 실행하는 것처럼 동작
- 가장 높은 수준의 격리
- 성능은 낮으나 모든 문제 완벽 방지
- 테이블 잠금을 사용하기도 함
##### 요약
|상황|추천 격리 수준|
|--|--|
|성능 우선, 캐시/로그 등 정합성 중요 X| Read Uncommitted|
|일반 업무 시스템, 트랜잭션 간 최소 충돌|Read Committed|
|금융/결제 등 데이터 정합성 중요| Repeatable Read|
|아주 높은 정확성 필요|Serializable|

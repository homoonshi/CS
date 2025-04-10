# Deadlock
- 두 개 이상의 트랜잭션이 서로가 가진 자원을 기다리며 무한 대기하는 상태
### 예시
```SQL
-- 트랜잭션 A
BEGIN;
UPDATE 상품 SET 재고 = 재고 - 1 WHERE 상품ID = 1;
-- 잠시 뒤...
UPDATE 상품 SET 재고 = 재고 - 1 WHERE 상품ID = 2;

-- 트랜잭션 B
BEGIN;
UPDATE 상품 SET 재고 = 재고 - 1 WHERE 상품ID = 2;
-- 잠시 뒤...
UPDATE 상품 SET 재고 = 재고 - 1 WHERE 상품ID = 1;
```
- 두 번째 UPDATE에서 서로의 락이 풀릴 때까지 대기함 -> 무한 대기
### 발생 조건
|조건|설명|
|--|--|
|상호 배제 (Mutual Exclusion)|자원은 한 트랜잭션에만 할당 가능|
|점유 대기 (Hold and Wait)|자원 하나를 점유한 채 다른 자원을 기다림|
|비선점 (No Preemption)|자원을 강제로 뺏을 수 없음|
|순환 대기 (Circular Wait)|트랜잭션이 서로 자원을 기다림 (A->B->A)|
### 발생 시 처리 방법
- 트랜잭션 중 하나를 강제 종료 (ROLLBACK)
- 오류 발생
### 데드락 방지/예방 방법
|전략|설명|
|--|--|
|항상 같은 순서로 자원 접근|항상 `상품 ID 작은 것부터 UPDATE`|
|짧은 트랜잭션 유지|BEGIN ~ COMMIT 구간을 짧게|
|SELECT 시에도 LOCK 고려|SELECT ..FOR UPDATE 등도 데드락 유발 가능|
|트랜잭션 수동 재시도 로직 구현|데드락 발생 시 자동 재시도 처리|
|필요한 만큼만 락 걸기|불필요한 `SELECT FOR UPDATE` 지양|

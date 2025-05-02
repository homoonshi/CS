# Procedure (프로시저)
- 미리 작성된 SQL 명령어들의 집합
- 반복해서 실행해야하는 복잡한 로직을 캡슐화해 쉽게 호출할 수 있게 도와줌
- 저장 프로시저는 데이터베이스에 저장되어 있는 하나의 서브 프로그램
- `CREATE PROCEDURE` 문을 사용해 생성하고, `CALL 프로시저이름()`으로 실행함
### 장점
|이유|설명|
|--|--|
|재사용성|여러 곳에서 동일한 로직을 재사용|
|보안성|SQL을 숨기고 인터페이스만 제공|
|성능|미리 컴파일되어 빠름|
|복잡한 작업 처리|반복적이고 조건적인 로직 처리에 유리|
### 프로시저를 사용하는 이유
- 성능 최적화가 필요한 대량 데이터 처리
    - 수천~수만 건의 데이터를 처리할 때 애플리케이션과 DB 통신을 반복하기보다, DB 내부에서 직접 반복 처리를 하게 함
    - 월말 정산, 대량 포인트 적립, 정기 통계 계산
- 복잡한 로직을 DB에 위임하고 싶을 때
    - 조건문, 반복문, 트랜잭션이 포함된 로직을 DB에서 처리
    - 여러 테이블을 조작하는 로직에 유용
- 보안 또는 제약 때문에 DB에서만 처리해야 할 때
    - DB 접근 권한이 제한된 상황에서, 프로시저만 실행 권한을 주는 방식
    - 민감한 로직을 숨기고, `CALL 프로시저명()`만 허용
- 트랜잭션을 DB에서 직접 제어해야 할 때
    - DB 내부에서 세부적으로 커밋/롤백을 처리해야 하는 경우
### 문법
```sql
DELIMITER //

CREATE PROCEDURE GetUserById(IN userId INT)
BEGIN
    SELECT * FROM users WHERE id = userId;
END //

DELIMITER ;

CALL GetUserById(@total);
SELECT @total;
```
### 주의사항
- 내부에서 트랜잭션을 다룰 수도 있으므로, 롤백이나 커밋 처리에 유의
- 비즈니스 로직이 너무 복잡해지면 애플리케이션 코드로 분리하는 것도 고려
### PostgreSQL 에서 사용법
- `CREATE PROCEDURE` 문법 도입
- 트랜잭션 제어(`COMMIT`, `ROLLBACK`) 가능 -> 함수에서 불가능
- `CALL 프로시저명(매개변수)`로 호출
#### 예제
```sql
CREATE PROCEDURE transfer_money(IN from_id INT, IN to_id INT, IN amount NUMERIC)
LANGUAGE plpgsql
AS $$
BEGIN
    -- 출금
    UPDATE accounts SET balance = balance - amount WHERE id = from_id;

    -- 입금
    UPDATE accounts SET balance = balance + amount WHERE id = to_id;

    -- 트랜잭션 커밋 (COMMIT)
END;
$$;
```
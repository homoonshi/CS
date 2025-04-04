# RDBMS vs NoSQL
## RDBMS
- 데이터를 테이블(행과 열)구조로 저장하는 데이터베이스
- 각 테이블은 명확한 스키마(구조 정의)를 갖고, SQL을 사용해 데이터를 조작함
#### 1. 특징
- 정형 데이터에 적합
- 데이터 무결성, ACID 트랜잭션 보장
- 테이블 간 관계를 통한 연관성 유지
- 스키마를 미리 정의해야 함
#### 2. 데이터베이스
- MySQL
- PostgreSQL
- Oracle
- Microsoft SQL Server
## NoSQL
- 비정형 또는 반정형 데이터를 저장하는 데이터베이스
- 테이블 기반이 아닌 다양한 형태의 저장 방식을 지님
- SQL이 아니어도 데이터를 저장/조회 가능
#### 1. 특징
- 유연한 스키마, 빠른 개발 속도
- 대규모 분산 시스템 적합
- 수평적 확장성 강점
- CAP 이론에 따라 가용성과 성능에 초점
#### 2. 데이터베이스
|유형|설명|DB|
|--|--|--|
|key-value 저장소|key에 값만 저장|Redis, DynamoDB|
|Document DB|JSON 또는 BSON 형식의 문서 저장|MongoDB, CouchDB|
|Column DB|컬럼 단위 저장, 분석에 유리|Cassandra, HBase|
|Graph DB|노드와 엣지 기반 관계 저장|Neo4j, ArangoDB|
## 차이점
|항목|RDBMS|NoSQL|
|--|--|--|
|데이터 구조|테이블|Key-Value, Document, Column, Graph 등 다양|
|스키마|고정 스키마|유연한 스키마|
|확장성|수직적(고사양 장비)|수평적(서버 추가 확장)|
|트랜잭션|ACID 보장|BASE 지향|
|성능|복잡한 쿼리 강점, 데이터 정합성 중요|대용량 처리, 읽기/쓰기 성능 우수|
|사용 용도|금융, ERP, 사용자 정보 등|실시간 로그, SNS, IoT, 캐시 등|
## 사용처
- 정형 데이터/복잡한 조인/높은 정합성 필요 -> RDBMS
- 대용량 데이터/빠른 속도/유연한 구조 필요 -> NoSQL

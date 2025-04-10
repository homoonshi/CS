# Index
- 특정 데이터를 빠르게 찾기 위해 사용
- 조회 속도 향상
    - WHERE, JOIN, ORDER BY, GROUP BY에서 빠르게 검색 가능
- 성능 최적화
    - 불필요한 데이터 읽기 줄여 리소스 절약
### 추천 상황
- WHERE 자주 쓰는 컬럼
- JOIN에 자주 쓰는 외래키
- ORDER BY, GROUP BY에 자주 등장
### 비추천 상황
- 자주 수정/삭제되는 컬럼
- 자주 사용되지 않는 컬럼
### 종류
|종류|설명|
|--|--|
|기본 인덱스 (B-tree)| 가장 일반적, 정렬된 구조|
|유니크 인덱스| 중복 허용 X, PK처럼 작동|
|복합 인덱스| 여러 칼럼을 묶어 인덱싱|
|풀텍스트 인덱스| 긴 텍스트 검색에 적합 (블로그 내용)|
|해시 인덱스| 해시 구조 기반, 아주 빠름 (조건 한정적)|
### 인덱스 작동 방식
##### (1) B-Tree 인덱스
- 대부분의 RDBMS의 기본 인덱스
- 정렬된 트리 구조
- 루트 -> 리프 노드로 내려가며 탐색
- 탐색, 삽입, 삭제 시 O(log n) 시간 복잡도
- 접두어 검색만 가능
```SQL
CREATE INDEX idx_name ON 직원(이름);
```
##### (2) Hash 인덱스
- 값을 해싱해서 위치를 바로 찾음
- 정렬 불가능, 범위 검색 불가능
- 완전한 일치 검색에만 빠름
- ID 중복 검색에 쓰일 것 같지만 `UNIQUE INDEX + B-Tree` 조합을 자주 사용
- 메모리 테이블, 임시 테이블, 캐시 같은 특수 상황에 '고려'됨
```SQL
CREATE TABLE 해시예제 (
    id INT,
    이름 VARCHAR(50),
    PRIMARY KEY (id),
    INDEX 이름_인덱스 (이름) USING HASH
) ENGINE=MEMORY;
```
##### (3) Full-Text 인덱스
- 긴 텍스트 필드 (TEXT, VARCHAR)에서 단어 단위로 쪼개 인덱싱
- 단어 매칭, 유사 단어, 관련도 순위 적용 가능
```SQL
CREATE FULLTEXT INDEX idx_content ON 블로그(내용);
```
##### (4) Spatial 인덱스
- `GEOMETRY`, `POINT`, `POLYGON` 등 공간 데이터를 위한 인덱스
- `R-Tree`, `Quadtree` 등 공간 검색 최적화 구조 사용
```SQL
CREATE SPATIAL INDEX idx_location ON 지점(위치);
```
##### (5) 비트맵 인덱스
- 값의 종류가 적은 컬럼에 적합 (성별, 상태값 등)
- 각 값마다 비트맵 배열을 만들어 빠르게 조합 연산
- AND/OR 조합해 효율적인 필터링 가능
- 대량 데이터 + 낮은 유니크 값에 효과적
```SQL
CREATE BITMAP INDEX idx_gender ON 회원(성별);
```
##### (6) 복합 인덱스
- 여러개 컬럼을 하나로 묶어 만든 인덱스
- 왼쪽 컬럼부터 순서대로만 인덱스 사용
```SQL
CREATE INDEX idx_multi ON 회원(이름, 생년월일);
```

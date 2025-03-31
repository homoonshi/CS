# Java 버전별 업데이트 사항
### Java 8 
- 2014년 출시, LTS 버전
- 대규모 릴리즈
- Lambda, Stream API 제공 -> 선언적 프로그래밍 가능
- Optional
- 새로운 날짜,시간 API (LocalDateTime)
### Java 11
- 2018년 출시, LTS 버전
- String, File 기능 향상
    - String : isBlank(), strip()
    - File : writeString(), readString()
- var 키워드 사용 가능
- Open JDK와 Oracle JDK 통합
- HTTP 클라이언트 표준화
- GC 개선 및 성능 향상
### Java 17
- 2021년 출시, LTS 버전
- Spring Boot 3.x.x 버전은 JDK 17 이상부터 지원
- Switch에 대한 패턴 매칭 (Preview)
- recode class 도입
- 텍스트 블록 기능 (""")을 추가해 코드를 간결하고 효율적으로 작성할 수 있도록 도움
### Java 21
- 2023년 출시, LTS 버전
- Spring Boot 3.2 부터 지원
- 가상 스레드 : Java 플랫폼에 경량 가상 스레드 도입
    - 가상 스레드 도입으로 몇 개의 운영체제 스레드만 사용해 수백만개의 가상 스레드 실행이 가능해짐
    - 기존 Java 코드 수정 X
- UTF-8 기본 값으로 사용

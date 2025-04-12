# Thread-per-request 모델
## 기본 구조
```Text
[클라이언트 요청]
      ↓
[TCP 커넥션 생성 (Tomcat) (서블릿 컨테이너)]
      ↓
[Tomcat 스레드 풀에서 스레드 1개 할당]
      ↓
[DispatcherServlet → Controller → Service → Repository → DB]
      ↓
[응답 작성 후 커넥션 종료]
```
## 흐름 설명
1. 요청 수신
    - 클라이언트가 HTTP 요청을 보내면, 내장 Tomcat이 요청을 서블릿 스레드풀로 전달
    - Tomcat은 기본적으로 `maxThreads = 200` 설정이 되어 있고, 동시에 처리 가능
2. 스레드 할당
    - 요청 하나당 스레드 1개 할당
    - 스레드는 응답을 리턴할 때까지 요청에 묶여(blocked) 계속 유지
3. 비즈니스 로직 처리
    - `@RestController` -> `@Service` -> `@Repository` 호출까지 모든 로직을 단일 스레드로 처리
    - DB I/O, 외부 API 호출 등으로 오래 걸리면, 그 스레드가 대기하며 다른 요청 처리를 못함
4. 응답 처리
    - `ResponseEntity` 등으로 응답 객체를 반환해 스레드를 스레드 풀로 반납
## REST API 에서의 스레드 특징
|항목|설명|
|--|--|
|동시성 처리|요청마다 스레드 하나씩 할당|
|처리 방식|동기/블로킹 방식|
|확장성 한계|동시 요청이 많아지면 스레드 부족 -> 지연 발생|
|병렬 처리 필요 시|`@Async`, 비동기 WebClient 등 사용 가능|

# AOP (Aspect-Oriented Programming)
- 관점 지향 프로그래밍
- 핵심 로직과 공통 기능을 분리해서 코드 중복 없이 깔끔하게 관리
### 핵심 용어 정리
|용어|설명|
|--|--|
|Aspect|공통 기능 모음 클래스(로그 찍기, 트랜잭션 처리)|
|JoinPoint|공통 기능이 삽입될 수 있는 지점(메서드 실행 전/후)|
|Advice|언제 실행할 지 정의(Before, After, Around 등)|
|Pointcut|어떤 메서드에 적용할 지 정의하는 필터|
|Weaving|실제 코드에 Aspect를 끼워 넣는 과정|
### 실무 사용 사례
|기능|설명|
|--|--|
|로깅|서비스, API 진입 로그 등|
|인증/인가|로그인 여부 체크, 권한 체크|
|트랜잭션 처리|DB 작업 단위 통합|
|성능 측정|실행 시간 측정|
|예외 처리|공통 예외 처리, 알림 전송|
### 예시
```Java
@Aspect
@Component
public class LoggingAspect {

    // Pointcut: 모든 Service 메서드
    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("👉 실행 전: " + joinPoint.getSignature().toShortString());
    }

    @AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
    public void logAfter(JoinPoint joinPoint, Object result) {
        System.out.println("✅ 성공 후: " + joinPoint.getSignature().toShortString() + ", 결과: " + result);
    }

    @AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "e")
    public void logError(JoinPoint joinPoint, Exception e) {
        System.out.println("❌ 예외 발생: " + joinPoint.getSignature().toShortString() + ", 에러: " + e.getMessage());
    }
}
```
### Spring AOP
|구성요소|설명|
|--|--|
|`@Aspect`|공통 기능 클래스|
|`@Before`, `@After`, `@Around` 등|실제 공통 기능 메서드(Advice)|
|`Pointcut`|공통 기능을 적용할 대상을 지정하는 식|
|`JoinPoint`|Advice가 끼어들 수 있는 지점|
|`Weaving`|Advice를 실제 코드에 적용하는 작업|

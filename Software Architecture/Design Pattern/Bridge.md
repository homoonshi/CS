# Bridge
- 기능과 구현 계층을 분리해 독립적으로 확장할 수 있도록 하는 패턴
- 추상적인 기능과 그 기능을 실제로 수행하는 구현을 따로 만들고 다리로 연결
### 장점
- 기능과 구현 분리
    - 서로 독립적으로 변경 가능
- 클래스 폭발 방지
    - 조합을 클래스로 전부 만들 필요 X
- 유지보수 용이
    - 기능/구현 둘 다 OCP 원칙 적용 가능
### 사용처
- UI 테마, 디바이스별 기능 구현
- 디바이스 종류 + 메세지 타입
- 조합이 많아질 수 있는 상황
### 구현 계층 (Implementor)
```Java
public interface Color {
    String fill(); // 색상을 채우는 방식
}

public class Red implements Color {
    public String fill() {
        return "빨간색";
    }
}

public class Blue implements Color {
    public String fill() {
        return "파란색";
    }
}
```
### 추상 계층 (Abstraction)
```Java
public abstract class Shape {
    protected Color color; // 구현체를 다리처럼 연결

    public Shape(Color color) {
        this.color = color;
    }

    public abstract void draw();
}
```
### 확장된 추상 계층 (Refined Abstraction)
```Java
public class Circle extends Shape {
    public Circle(Color color) {
        super(color);
    }

    public void draw() {
        System.out.println(color.fill() + " 원을 그립니다.");
    }
}

public class Square extends Shape {
    public Square(Color color) {
        super(color);
    }

    public void draw() {
        System.out.println(color.fill() + " 사각형을 그립니다.");
    }
}
```
### 사용
```Java
public class Main {
    public static void main(String[] args) {
        Shape redCircle = new Circle(new Red());
        Shape blueSquare = new Square(new Blue());

        redCircle.draw();     // 빨간색 원을 그립니다.
        blueSquare.draw();    // 파란색 사각형을 그립니다.
    }
}
```
### Spring의 Bridge 패턴 사용 예시
- 인터페이스로 기능을 추상화하고, 다양한 구현체와 조합해서 쓰는 구조 많음
##### 구현 계층
```Java
public interface MessageSender {
    void send(String to, String message);
}

@Component
public class EmailSender implements MessageSender {
    public void send(String to, String message) {
        System.out.println("이메일 전송: " + to + " - " + message);
    }
}

@Component
public class KakaoSender implements MessageSender {
    public void send(String to, String message) {
        System.out.println("카카오톡 전송: " + to + " - " + message);
    }
}
```
##### 추상 계층
```Java
public abstract class NotificationService {
    protected final MessageSender sender;

    public NotificationService(MessageSender sender) {
        this.sender = sender;
    }

    public abstract void notify(String to);
}
```
##### 기능 계층 확장
```Java
public class SignUpNotification extends NotificationService {
    public SignUpNotification(MessageSender sender) {
        super(sender);
    }

    @Override
    public void notify(String to) {
        sender.send(to, "[회원가입] 가입을 환영합니다!");
    }
}

public class ReservationNotification extends NotificationService {
    public ReservationNotification(MessageSender sender) {
        super(sender);
    }

    @Override
    public void notify(String to) {
        sender.send(to, "[예약] 예약이 완료되었습니다.");
    }
}
```
##### 사용 예시
```Java
@RestController
@RequiredArgsConstructor
public class NotificationController {
    private final EmailSender emailSender;
    private final KakaoSender kakaoSender;

    @GetMapping("/test-email")
    public void testEmail() {
        NotificationService signUp = new SignUpNotification(emailSender);
        signUp.notify("user@example.com");
    }

    @GetMapping("/test-kakao")
    public void testKakao() {
        NotificationService reservation = new ReservationNotification(kakaoSender);
        reservation.notify("010-1234-5678");
    }
}
```
### AOP 적용 예시
```Java
@Aspect
@Component
public class NotificationAspect {

    @Around("execution(* *.notify(..))")
    public Object logAndRetry(ProceedingJoinPoint joinPoint) throws Throwable {
        String method = joinPoint.getSignature().toShortString();
        Object[] args = joinPoint.getArgs();
        System.out.println("✅ 알림 시작: " + method + " → 대상: " + args[0]);

        try {
            Object result = joinPoint.proceed();
            System.out.println("✅ 알림 성공: " + method);
            return result;
        } catch (Exception e) {
            System.out.println("❌ 알림 실패: " + e.getMessage());
            throw e;
        }
    }
}
```

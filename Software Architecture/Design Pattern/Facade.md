# Facade
- 복잡한 서브시스템(여러 클래스, 로직 등)을 간단한 인터페이스로 감싸 사용자가 쉽게 쓸 수 있게 해줌
### 장점
- 사용 편의성
- 의존성 감소
- 느슨한 결합
- 코드 가독성 향상
### 실무 사례
- Spring의 `JdbcTemplate`
- JPA `EntityManager`
- `RestTemplate`, `WebClient`
- 라이브러리 래퍼
### 예시
##### 복잡한 서브시스템 클래스
```Java
public class TicketSystem {
    public void buyTicket() {
        System.out.println("🎫 티켓을 구매합니다.");
    }
}

public class SeatSystem {
    public void chooseSeat() {
        System.out.println("💺 좌석을 선택합니다.");
    }
}

public class SnackSystem {
    public void getSnacks() {
        System.out.println("🍿 팝콘을 구매합니다.");
    }
}

public class TheaterSystem {
    public void turnOffLights() {
        System.out.println("💡 조명을 끕니다.");
    }

    public void startMovie() {
        System.out.println("🎬 영화를 시작합니다.");
    }
}
```
##### Facade 클래스
```Java
public class CinemaFacade {
    private TicketSystem ticket;
    private SeatSystem seat;
    private SnackSystem snack;
    private TheaterSystem theater;

    public CinemaFacade() {
        this.ticket = new TicketSystem();
        this.seat = new SeatSystem();
        this.snack = new SnackSystem();
        this.theater = new TheaterSystem();
    }

    public void watchMovie() {
        ticket.buyTicket();
        seat.chooseSeat();
        snack.getSnacks();
        theater.turnOffLights();
        theater.startMovie();
    }
}
```
##### 사용 예시
```Java
public class Main {
    public static void main(String[] args) {
        CinemaFacade cinema = new CinemaFacade();
        cinema.watchMovie(); // 내부 시스템 몰라도 영화 관람 OK!
    }
}
```


# FlyWeight
- 같은 데이터를 공유해서 메모리 낭비를 줄이는 패턴
- 많은 수의 유사 객체를 효율적으로 다루기 위해 공통 상태는 유지하고 변하는 부분만 따로 관리
### 사용 사례
|사용 사례|설명|
|--|--|
|글자 렌더링|글자는 같지만 위치만 다름|
|게임 유닛|유닛 그래프는 같고 좌표/상태만 다름|
|GUI 아이콘|같은 이미지지만 위치, 상태 다름|
|캐싱된 DB 커넥션|설정은 같고 요청만 다름|
### 주의
- Flyweight 객체는 불변이어야 함
- 변하는 상태는 반드시 외부에서 전달
- 너무 많이 나누면 설계가 복잡해짐
### 구성 요소
|구성|설명|
|--|--|
|Flyweight|공유되는 객체(불변)|
|ConcreteFlyweight|실제 객체 구현|
|FlyweightFactory|이미 생성된 객체를 관리 (캐싱)|
|Client|외부 상태(extrinsic state, 위치 등) 전달|
### 예시
##### Flyweight 객체
```Java
public class Character {
    private final char symbol; // 공유 상태

    public Character(char symbol) {
        this.symbol = symbol;
    }

    public void display(int x, int y) {
        System.out.println("[" + symbol + "] 위치 (" + x + ", " + y + ")");
    }
}
```
##### Flyweight Factory
```Java
import java.util.HashMap;
import java.util.Map;

public class CharacterFactory {
    private Map<Character, Character> pool = new HashMap<>();

    public Character getCharacter(char c) {
        if (!pool.containsKey(c)) {
            pool.put(c, new Character(c));
        }
        return pool.get(c);
    }
}
```
##### 사용 예시
```Java
public class Main {
    public static void main(String[] args) {
        CharacterFactory factory = new CharacterFactory();

        Character c1 = factory.getCharacter('a');
        Character c2 = factory.getCharacter('b');
        Character c3 = factory.getCharacter('a'); // 공유됨

        c1.display(1, 1);
        c2.display(2, 1);
        c3.display(3, 1);

        System.out.println("c1 == c3 ? " + (c1 == c3)); // true ✅
    }
}
```
### 스프링에서 사용 사례
##### Spring Bean
- Bean : `Singleton`
- 어떤 클래스든 스프링 컨테이너에 한 번만 생성되고 공유
- 같은 객체를 여러 곳에서 사용하지만 메모리는 단 1개
```Java
@Service
public class MailService {
    public void sendMail(String to, String content) {
        System.out.println("메일 전송 → " + to + ": " + content);
    }
}

@RestController
@RequiredArgsConstructor
public class NotificationController {

    private final MailService mailService;

    @PostMapping("/notify")
    public void notify(@RequestParam String to) {
        mailService.sendMail(to, "안녕하세요!");
    }
}
```
##### 공통 유틸 컴포넌트
- `RestTemplate`, `ObjectMapper`, `Validator`
```Java
@Configuration
public class CommonConfig {
    
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }

    @Bean
    public ObjectMapper objectMapper() {
        return new ObjectMapper();
    }
}
```
##### Thymeleaf, jackson, Hibernate 내부
```Java
ObjectMapper mapper = new ObjectMapper();
// 내부에서 같은 FieldNamingStrategy, Serializer 캐싱 → Flyweight
```
##### Spring Security
- 권한 정보와 같은 객체들
- 시스템 내부에서 공유되는 상수이므로 Flyweight 구조 사용
- 내부에서 Map<String, Role>처럼 캐싱되어 동작

# MVC
- Model-View-Controller의 약자
- 웹 애플리케이션에서 코드를 역할에 따라 분리해서 관리하기 위해 사용
### 쓰는 목적
- 관심사 분리 : 역할에 따라 코드 분리
- 협업 효율 향상
- 테스트 용이성
### Model (모델)
- 데이터와 비즈니스 로직 담당
- 데이터베이스와 직접적으로 소통하며, 데이터를 가져오거나 저장
```Java
// Java 예시 - Spring에서의 모델
@Entity
public class User {
    @Id
    private Long id;
    private String name;
    private String email;
}
```
### View (뷰)
- 사용자에게 보여지는 화면(UI)를 담당
- 모델 데이터를 받아와 사용자에게 보여주는 역할
- 로직 처리 X
- HTML, Thymeleaf, JSP, React 컴포넌트 등
### Controller (컨트롤러)
- 요청을 받아와 적절한 모델을 호출하고 결과를 뷰에 전달
- 사용자와 시스템의 중간다리 역할
```Java
@Controller
public class UserController {
    @GetMapping("/user")
    public String getUser(Model model) {
        User user = userService.getUser();
        model.addAttribute("user", user);
        return "userView"; // View 이름
    }
}
```
### Spring MVC의 전체 구조
```csharp
[Client 브라우저]
       ↓ HTTP 요청
[Controller]  ← 사용자 요청 처리, 서비스 호출
       ↓
[Service]     ← 비즈니스 로직 처리
       ↓
[Repository]  ← DB 접근, JPA/Querydsl 등으로 DB와 통신
       ↓
[Database]
```
### Controller
- HTTP 요청을 받는 진입점
- 클라이언트 요청을 받아 Service 호출, 응답 반환
- View를 리턴하거나 JSON 데이터 반환
```Java
@RestController
@RequestMapping("/users")
public class UserController {

    private final UserService userService;
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserDto> getUser(@PathVariable Long id) {
        UserDto user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}
```
### Service
- 비즈니스 로직 담당 (핵심 로직)
- 여러 Repository를 조합하거나 조건 처리 등 비즈니스 규칙 적용
- 여러 데이터 조합/검증
```Java
@Service
public class UserService {

    private final UserRepository userRepository;
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public UserDto getUserById(Long id) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("사용자 없음"));
        return new UserDto(user.getId(), user.getName(), user.getEmail());
    }
}
```
### Repository
- DB와 직접적으로 통신하는 계층
- Spring Data JPA나 Querydsl 등을 사용해 구현
```Java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    // 기본적으로 findById, save, delete 등 CRUD 제공
}
```
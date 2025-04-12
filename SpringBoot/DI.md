# DI (Dependency Injection)
- 의존관계 주입
## 주입 방법
- 생성자
- 수정자 (Setter)
- 필드
- 일반 메서드
## 생성자 주입
- 생성자를 통해 의존 관계를 주입 받음
- 생성자 호출 시점에 딱 1번만 호출됨
- "불변, 필수" 의존 관계에 사용
- Set을 추가해 변경하지 않음
- final -> 값이 꼭 있어야 함
- 생성자가 딱 1개만 있다면 `@Autowired`를 생략해도 됨
```Java
@Component
public class OrderServiceImpl implements OrderService{

  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;

  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy){
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }
}
```
- 롬복 사용 (권장)
```Java
@Component
@RequiredArgsConstructor // final 붙은 애들을 생성자 만들어줌 -> 생성자 주입 
public class OrderServiceImpl implements OrderService{

  @Autowired private final MemberRepository memberRepository;
  @Autowired private final DiscountPolicy discountPolicy;
}
```
### 생성자 주입을 권장하는 이유
- 불변
    - 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료 시점까지 의존관계를 변경할 일이 없음
    - 수정자 주입을 사용하면 setXxx 메서드를 public으로 열어야 함
    - 누군가가 실수로 변경할 수도 있고 변경하면 안되는 메서드를 열어두는건 좋은 설계가 아님
    - 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출될 일 X
- 누락
    - 프레임워크 없이 순수한 자바 단위 테스트 코드를 작성하는 경우
        - 필드에 final 키워드 넣기 가능
        - 컴파일 시점에서 오류를 캐치함
## 수정자 주입
- setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해 의존관계 주입
- "선택, 변경"
- MemberRepository가 스프링 빈에 등록되지 않아도 사용 가능
- 선택적 -> (required = false)
- 변경 가능성 있는 의존 관계
- `@Autowired`가 없으면 작동하지 않음
```Java
@Component
public class OrderServiceImpl implements OrderService{

  private MemberRepository memberRepository;
  private DiscountPolicy discountPolicy;
  
  @Autowired
  public void setMemberRepository(MemberRepository memberRepository){
    this.memberRepository = memberRepository;
  }
  
  @Autowired
  public void setDiscountPolicy(DiscountPolicy discountPolicy){
    this.discountPolicy = discountPolicy
  }

}
```
## 필드 주입
- 필드에 바로 주입하는 방식
- 코드가 간결하나, 외부에서 변경이 불가능해 테스트하기 힘듬
- DI 프레임워크가 없으면 아무 것도 할 수 없음
- 사용하지 않는 것을 권장
- 스프링 컨테이너가 아닌 순수한 자바로 테스트 할 수 있는 방법 X
```Java
@Autowired private MemberRepository memberRepository;
@Autowired private DiscountPolicy discountPolicy;
```
## 일반 메서드
- 한번에 여러 필드 주입 가능
- 일반적으로 잘 사용 X
```Java
  @Autowired
  public void init(MemberRepository memberRepository,DiscountPolicy discountPolicy){
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }

```
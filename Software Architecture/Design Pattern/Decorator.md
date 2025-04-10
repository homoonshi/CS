# Decorator
- 기존 객체에 기능을 덧붙일 수 있도록 하는 패턴
- 상속 없이도 런타임에 동적으로 기능 추가 가능
### 구조 설명
|구조|설명|
|--|--|
|Component|기본 인터페이스|
|ConcreteComponent|기본 구현체|
|Decorator|Component를 구현하면서 내부에 Component를 품음|
|ConcreteDecorator|기능을 실제로 추가하는 클래스|
### Decorator와 상속 비교
|항목|상속|데코레이터|
|--|--|--|
|기능 확장|컴파일 시 정해짐|런타임에 유연하게 가능|
|클래스 수|조합마다 클래스 생성 필요|조합 자유롭게 가능|
|유연성|낮음|매우 높음|
|대표 예|ArrayList extends AbstractList|BufferedReader(new InputStreamReader())|
### 실무 사용 사례
|사용처|설명|
|--|--|
|Java IO|`BufferedReader`, `InputStreamReader`, `FileReader` 등|
|Spring|`HandlerInterceptor`, `FilterChain`, `HttpServletRequestWrapper`|
|GUI 컴포넌트|버튼 + 스크롤바 + 테두리 등|
### 예시
##### Component 인터페이스
```Java
public interface Burger {
    String getDescription();
    int getCost();
}
```
##### 기본 구현체
```Java
public class BasicBurger implements Burger {
    public String getDescription() {
        return "햄버거";
    }

    public int getCost() {
        return 3000;
    }
}
```
##### 추상 데코레이터
```Java
public abstract class BurgerDecorator implements Burger {
    protected Burger burger;

    public BurgerDecorator(Burger burger) {
        this.burger = burger;
    }

    public String getDescription() {
        return burger.getDescription();
    }

    public int getCost() {
        return burger.getCost();
    }
}
```
##### 구현 데코레이터
```Java
public class CheeseDecorator extends BurgerDecorator {
    public CheeseDecorator(Burger burger) {
        super(burger);
    }

    public String getDescription() {
        return super.getDescription() + ", 치즈";
    }

    public int getCost() {
        return super.getCost() + 500;
    }
}

public class BaconDecorator extends BurgerDecorator {
    public BaconDecorator(Burger burger) {
        super(burger);
    }

    public String getDescription() {
        return super.getDescription() + ", 베이컨";
    }

    public int getCost() {
        return super.getCost() + 700;
    }
}
```
##### 사용
```Java
public class Main {
    public static void main(String[] args) {
        Burger burger = new BasicBurger();
        burger = new CheeseDecorator(burger);
        burger = new BaconDecorator(burger);

        System.out.println("설명: " + burger.getDescription()); // 햄버거, 치즈, 베이컨
        System.out.println("가격: " + burger.getCost());       // 3000 + 500 + 700 = 4200
    }
}
```

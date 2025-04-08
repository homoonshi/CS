# Factory Method
- 객체 생성을 서브 클래스에 위임
- 객체 생성 코드를 별도의 Factory 메서드로 분리
- new 메서드를 사용하지 않음
- 객체를 만들어주는 메서드 정의
- 어떤 구체 클래스의 인스턴스를 만들진 서브 클래스가 결정
### 예시
```Java
abstract class BurgerStore {
    public void orderBurger() {
        Burger burger = createBurger(); // 어떤 버거인지는 서브클래스가 결정
        burger.prepare();
        burger.cook();
        burger.wrap();
    }

    protected abstract Burger createBurger();
}
```
```Java
class BulgogiBurgerStore extends BurgerStore {
    @Override
    protected Burger createBurger() {
        return new BulgogiBurger();
    }
}

class ChickenBurgerStore extends BurgerStore {
    @Override
    protected Burger createBurger() {
        return new ChickenBurger();
    }
}
```
```Java
public class Main {
    public static void main(String[] args) {
        BurgerStore store = new BulgogiBurgerStore();
        store.orderBurger();

        System.out.println();

        store = new ChickenBurgerStore();
        store.orderBurger();
    }
}
```
### 장점
- 객체 생성 코드를 캡슐화
- 확장에 유리함 (새로운 클래스를 추가할 때 기존 코드 변경 X)
- 개방/폐쇄 원칙 (OCP) 준수
### 쓰는 목적
- 객체 생성이 복잡하거나 조건에 따라 달라질 때
- 라이브러리/프레임워크에서 구체적인 구현을 나중에 결정하고 싶을 때
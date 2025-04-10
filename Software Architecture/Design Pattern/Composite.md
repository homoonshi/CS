# Composite
- 단일 객체와 복합 객체를 동일하게 다룰 수 있도록 트리 구조로 구성
- 부분-전체 관계를 갖는 객체들을 일관된 방식으로 처리할 수 있게 해줌
### 장점
- 일관된 처리
- 재귀 구조
- 확장성 우수
### 단점
- 전체 구조가 커지면 순환 참조나 깊은 재귀 관리 필요
- 단일 객체와 복합 객체의 차이가 코드상 명확하지 않을 수도 있음
### 실무 사용 사례
|분야|예시|
|--|--|
|파일 시스템|파일(Leaf), 폴더(Composite)|
|UI 컴포넌트|버튼, 패널, 레이아웃|
|HTML DOM|`div` 안에 `span`, `ul` 등 중첩 가능|
|조직도|직원(Leaf), 부서(Composite)|
### 예시
##### 공통 인터페이스
```Java
public interface MenuComponent {
    void show(); // 메뉴 표시
}
```
##### Leaf : 단일 메뉴
```Java
public class MenuItem implements MenuComponent {
    private String name;

    public MenuItem(String name) {
        this.name = name;
    }

    public void show() {
        System.out.println("📄 " + name);
    }
}
```
##### Composite : 메뉴 그룹
```Java
import java.util.ArrayList;
import java.util.List;

public class MenuGroup implements MenuComponent {
    private String name;
    private List<MenuComponent> items = new ArrayList<>();

    public MenuGroup(String name) {
        this.name = name;
    }

    public void add(MenuComponent item) {
        items.add(item);
    }

    public void show() {
        System.out.println("📁 " + name);
        for (MenuComponent item : items) {
            item.show();
        }
    }
}
```
##### 사용 예시
```Java
public class Main {
    public static void main(String[] args) {
        MenuGroup root = new MenuGroup("전체 메뉴");

        MenuGroup korean = new MenuGroup("한식");
        korean.add(new MenuItem("김치찌개"));
        korean.add(new MenuItem("불고기"));

        MenuGroup western = new MenuGroup("양식");
        western.add(new MenuItem("파스타"));
        western.add(new MenuItem("스테이크"));

        root.add(korean);
        root.add(western);

        root.show(); // 전체 메뉴 구조 출력
    }
}
```
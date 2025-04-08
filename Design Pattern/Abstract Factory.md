# Abstract Factory
- 관련 있는 객체들을 패밀리 단위로 생성하는 팩토리들을 모아놓은 추상 팩토리
- 서로 관련된 객체들을 통일된 방식으로 생성하기 위한 팩토리의 팩토리
### 예시
```Java
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}
```
```Java
public class WinFactory implements GUIFactory {
    public Button createButton() {
        return new WinButton();
    }
    public Checkbox createCheckbox() {
        return new WinCheckbox();
    }
}

public class MacFactory implements GUIFactory {
    public Button createButton() {
        return new MacButton();
    }
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```
```Java
public class Application {
    private Button button;
    private Checkbox checkbox;

    public Application(GUIFactory factory) {
        button = factory.createButton();
        checkbox = factory.createCheckbox();
    }

    public void renderUI() {
        button.render();
        checkbox.render();
    }
}
```
```Java
public class Main {
    public static void main(String[] args) {
        GUIFactory factory;

        String os = "mac"; // 또는 "windows"

        if (os.equals("windows")) {
            factory = new WinFactory();
        } else {
            factory = new MacFactory();
        }

        Application app = new Application(factory);
        app.renderUI();
    }
}
```
### 사용 목적
- 크로스 플랫폼 UI 개발
- JDBC Driver 관리
- Spring Bean 구조
- 테스트 코드에서 Mock 객체 생성
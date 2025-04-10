# Prototype
- 기존 객체를 복사해 새로운 객첼르 만드는 패턴
- new로 새로 만들지 않고 복제(clone) 해서 쓰는 방식
### 쓰는 목적
- 객체 생성 비용이 크거나 복잡할 때
- 비슷한 객체를 여러 개 만들어야 할 때
- new로 매번 생성하는 것이 비효율적일 때
### 예시
```Java
public class Bear implements Cloneable {
    private String name;
    private String color;

    public Bear(String name, String color) {
        this.name = name;
        this.color = color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    // 복제 메서드
    @Override
    public Bear clone() {
        try {
            return (Bear) super.clone(); // 얕은 복사
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public String toString() {
        return "Bear{name='" + name + "', color='" + color + "'}";
    }
}
```
```Java
public class Main {
    public static void main(String[] args) {
        Bear original = new Bear("곰돌이", "갈색");

        Bear copy1 = original.clone();
        Bear copy2 = original.clone();

        copy1.setColor("흰색");

        System.out.println(original); // Bear{name='곰돌이', color='갈색'}
        System.out.println(copy1);    // Bear{name='곰돌이', color='흰색'}
        System.out.println(copy2);    // Bear{name='곰돌이', color='갈색'}
    }
}
```
### Java에서 Prototype을 대신하는 방법
```Java
@Builder(toBuilder = true)
public class User {
    private String name;
    private String email;
}

User user1 = User.builder().name("홍길동").email("hi@gmail.com").build();
User user2 = user1.toBuilder().email("bye@gmail.com").build(); // 복제 + 일부 수정
```
### 실제 사용 사례
- 게임 캐릭터 생성
- 문서 템플릿 복제
- Spring에서 Bean 복제

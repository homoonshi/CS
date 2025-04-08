# Builder
- 복잡한 객체를 단계별로 만들 수 있게 도와주는 설계도와 조립공 같은 패턴
### 장점
- 복잡한 객체 생성 과정을 캡슐화
- 각 단계별로 구성할 수 있어 유연
- 동일한 빌더로도 다양한 조합의 객체를 만들 수 있음
- new로 한번에 생성하는 것보다 가독성이 좋고, 설정이 명확함
### 예시
```Java
public class LunchBox {
    private String rice;
    private String mainDish;
    private String sideDish;

    // 생성자는 private, Builder로만 만들 수 있게 함
    private LunchBox(Builder builder) {
        this.rice = builder.rice;
        this.mainDish = builder.mainDish;
        this.sideDish = builder.sideDish;
    }

    @Override
    public String toString() {
        return "LunchBox{" +
                "rice='" + rice + '\'' +
                ", mainDish='" + mainDish + '\'' +
                ", sideDish='" + sideDish + '\'' +
                '}';
    }

    // 2. Builder 클래스: 조립공
    public static class Builder {
        private String rice;
        private String mainDish;
        private String sideDish;

        public Builder addRice(String rice) {
            this.rice = rice;
            return this;
        }

        public Builder addMainDish(String mainDish) {
            this.mainDish = mainDish;
            return this;
        }

        public Builder addSideDish(String sideDish) {
            this.sideDish = sideDish;
            return this;
        }

        public LunchBox build() {
            return new LunchBox(this);
        }
    }
}
```
```Java
public class Main {
    public static void main(String[] args) {
        LunchBox lunchBox = new LunchBox.Builder()
                .addRice("잡곡밥")
                .addMainDish("치킨")
                .addSideDish("김치")
                .build();

        System.out.println(lunchBox);
    }
}
```
### Lombok의 @Builder
- 복잡한 Builder 클래스를 자동으로 생성해주는 Lombok
```Java
import lombok.Builder;
import lombok.ToString;

@Builder
@ToString
public class LunchBox {
    private String rice;
    private String mainDish;
    private String sideDish;
}
```
```Java
public class Main {
    public static void main(String[] args) {
        LunchBox lunchBox = LunchBox.builder()
                .rice("잡곡밥")
                .mainDish("치킨")
                .sideDish("김치")
                .build();

        System.out.println(lunchBox);
    }
}
```


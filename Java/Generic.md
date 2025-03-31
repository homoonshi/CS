# Generic
- 데이터 타입을 파라미터처럼 다룰 수 있게 해줌
- 클래스나 메소드에 타입을 외부에서 주입할 수 있게 해줌
- 객체를 초기화할 때 타입을 지정해주기 때문에 타입 캐스팅이 필요 없음
```Java
List<String> list = new ArrayList<>();
list.add("hello");
// list.add(123); // 컴파일 에러

String s = list.get(0); // 형변환 불필요
```
### 장점
- 타입 안정성 
    - 컴파일 시 타입 체크
- 코드 재사용성 
    - 하나의 클래스/메서드로 여러 타입 처리
- 가독성 향상
    - 형변환 없이 명확한 타입 코드 작성 가능
### 클래스에 제네릭 사용
```Java
public class Box<T> {
    private T item;

    public void set(T item) { this.item = item; }
    public T get() { return item; }
}

Box<String> stringBox = new Box<>();
stringBox.set("사과");
String item = stringBox.get(); // 형변환 필요 없음
```
### 메서드에 제네릭 사용
```Java
public <T> void print(T item) {
    System.out.println(item);
}

print("문자열");
print(123);
```

### 자주 사용하는 제네릭 타입
|제네릭 표현|의미|
|---|---|
|T|Type (타입)|
|E|Element (컬렉션요소)|
|K, V| Key, Value (Map)|
|?|와일드카드|


# Serialization (직렬화)
- 자바 객체를 파일이나 네트워크로 전송하기 위해 byte 형태로 변환
- 객체 자체를 보낼 수 없기 때문에 바이트로 변환해서 보냄
```Java
import java.io.*;

// 직렬화 대상 클래스
class Person implements Serializable {
    private static final long serialVersionUID = 1L; // 버전 관리용
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

Person p = new Person("홍길동", 30);

try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
    out.writeObject(p); // 직렬화: 객체 -> 바이너리 파일
    System.out.println("직렬화 완료!");
} catch (IOException e) {
    e.printStackTrace();
}
```
- serialVersionUID로 클래스 버전을 관리해 클래스 변경 시 충돌을 방지함
- transient 키워드를 사용하면 직렬화에서 제외 가능

### Deserialization (역직렬화)
- byte 형태의 데이터를 다시 원래의 자바 객체로 복원하는 과정
```Java
try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("person.ser"))) {
    Person restored = (Person) in.readObject(); // 역직렬화: 바이너리 파일 -> 객체
    System.out.println("이름: " + restored.name + ", 나이: " + restored.age);
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

### 직렬화가 쓰이는 예시
- 파일입출력
- 네트워크 통신 (소켓)
- 웹 세션 저장
- 캐싱
- JPA / Hibernate Lazy Loading 시 내부에서 활용
- 멀티 스레드 환경에서 객체 복제

### 실무에서 사용?
- 분산 시스템이나 외부 API 통신에선 직렬화보다 JSON, Protobuf, XML이 쓰임
- 악의적인 바이트 스트림이 와서 역직렬화 했을 때 시스템에서 코드가 실행될 수 있어 위험
- 허용된 클래스만 역직렬화 하도록 필터링 할 수도 있음 (JDK 9부터 제공)
- HTTP 세션 저장, 로컬 파일 저장, JMS/RMI/소켓통신(JVM<->JVM), 객체 깊은 복사, 직렬화 기반 캐시 시스템에서 일부 사용
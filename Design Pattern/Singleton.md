# Singleton
- 프로그램에서 단 하나의 인스턴스만 존재하도록 보장하는 패턴
- 설정값, 로그, DB 연결처럼 하나만 있으면 되는 객체를 누가 언제 어디서 불러도 항상 같은 객체를 반환하고 싶을 때 사용함
### 예시
```Java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
        // 외부에서 new 금지
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // 최초 1회만 생성
        }
        return instance;
    }
}
```
```Java
public class Main {
    public static void main(String[] args) {
        Singleton a = Singleton.getInstance();
        Singleton b = Singleton.getInstance();

        System.out.println(a == b); // true
    }
}
```
### 문제
- 멀티스레드 환경에서 위험함
- 두 스레드가 동시에 `getInstance`를 호출하면 2개의 객체가 생성될 수 있음
#### 해결법
##### (1) synchronized 사용
```Java
public static synchronized Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```
##### (2) Double-Checked Locking
```Java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```
##### (3) 클래스 로더
```Java
public class Singleton {
    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```
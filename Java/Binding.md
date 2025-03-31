# Binding
- 함수를 호출할 때 함수가 위치한 메모리로 연결시켜주는 것
### 정적 바인딩
- 컴파일 타임에 결정
- 컴파일 시간에 정보가 결정되므로 효율이 좋음
- 주로 private, static, final, overloaded 메서드
```Java
class Animal {
    void sound() {
        System.out.println("동물 울음소리");
    }

    static void info() {
        System.out.println("Animal 정보");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("멍멍");
    }

    static void info() {
        System.out.println("Dog 정보");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.info();      // 정적 바인딩 → Animal의 info() 호출됨
    }
}
```
### 동적 바인딩
- 런타임에 결정
- 런타임에 자유롭게 바뀌므로 적응성 좋음
- 타입 체크로 인한 수행 속도 저하
- 오버라이딩 된 메서드
```Java
class Animal {
    void sound() {
        System.out.println("동물 울음소리");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("멍멍");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound();   // 동적 바인딩 → 실제 객체(Dog)의 sound() 호출
    }
}
```
# String (문자열)
- 객체
- String은 불변
- String + String + String
    - 각각의 String 주소 값이 Stack에 쌓임
    - GC가 호출되기 전까지 String 객체들이 Heap에 쌓임
    - StringBuffer나 StringBuilder 사용 권장
### 리터럴과 new의 차이
```Java
String a = "hello";         // 리터럴
String b = new String("hello"); // 객체 생성
```

|구분|리터럴("hello")|new String("hello")|
|---|---|---|
|메모리 위치|String constant pool (메서드 영역)|힙(Heap)|
|중복 제거|동일 문자열 재사용 (interning)|매번 새 객체 생성|
|== 비교| true (같은 참조)| false (다른 객체)|
|equals()|항상 true (내용 비교)|항상 true (내용 비교)|
- JVM이 String Constant Pool (문자열 상수 풀)에 "hello"를 저장함
- 이미 존재하는 리터럴은 공유됨 (중복 제거)
```Java
String a = "hello";
String b = "hello";
System.out.println(a == b); // ✅ true → 같은 참조
```
|상황|권장방식|
|--|--|
|문자열이 고정된 값, 비교 자주 발생|"리터럴" 사용|
|정말 필요한 경우에만 새로운 객체 생성|new String()은 거의 안 씀|
|문자열 동등성 비교 시 항상|equals() 사용|

- intern()을 사용해 상수 풀로 묶을 수 있음
```Java
String b = new String("hello");
String c = b.intern();

System.out.println(c == "hello"); // ✅ true
```
- String Pool은 전역적이기 때문에 많은 문자열이 쌓이면 메모리 부담 증가
- 자주 사용하는 값만 intern 사용 -> "Y", "N", "SUCCESS", "ERROR"
- 반복 문자열은 SpringBuilder나 캐시 사용
### StringBuilder & StringBuffer
- Memory에 append 하는 방식으로 클래스에 대한 객체 직접 생성 X
- StringBuilder
    - 변경 가능한 문자열
    - 비동기 처리
- StringBuffer
    - 변경 가능한 문자열
    - 동기 처리
    - multiple thread 환경에서 안전한 클래스
- Thread와 관련 있으면 StringBuffer, 관련 없으면 StringBuilder 사용

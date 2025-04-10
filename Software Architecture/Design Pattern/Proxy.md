# Proxy
- 객체에 직접 접근하지 않고 객체의 대리인을 통해 접근하게 만듬
- 진짜 객체 대신 중간에서 조정하거나 제어하는 객체를 둠
### 쓰는 목적
- 객체 접근 통제
- 기능 확장
- 원본 객체 늦게 생성
- 실제 객체가 무거울 때 성능 최적화
### 쉬운 예시
- 유튜브
    - 썸네일 이미지와 프록시 객체만 먼저 보여줌
    - 영상은 나중에 로딩
### 패턴 종류
|종류|설명|예시|
|--|--|--|
|Virtual Proxy|무거운 객체를 필요할 때만 생성|이미지, 대용량 DB 연결|
|Protection Proxy|접근 권한 제어|관리자만 가능|
|Remote Proxy|다른 JVM 또는 서버의 객체 대신|RMI, gRPC|
|Caching Proxy|결과를 캐싱해서 성능 향상|API 결과 캐시|
### Spring에서 Proxy 패턴 사용 예
|사용처|설명|
|--|--|
|AOP|메서드 실행 전/후를 프록시 객체로 감싸 처리|
|트랜잭션 처리|`@Transactional`은 실제 대상이 아닌 프록시를 호출해 트랜잭션 관리|
|Security|메서드 접근 전 권한 체크 수행|
|Lazy Bean|`@Lazy`는 프록시를 먼저 등록하고 진짜 객체는 나중에 생성|
### 예시
##### Subject 인터페이스
```Java
public interface Image {
    void display();
}
```
##### RealSubject 실제 가능 클래스
```Java
public class RealImage implements Image {
    private String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk(); // 무거운 작업
    }

    private void loadFromDisk() {
        System.out.println("📂 " + filename + " 로딩 중...");
    }

    public void display() {
        System.out.println("🖼 " + filename + " 표시 중...");
    }
}
```
##### Proxy 클래스
```Java
public class ProxyImage implements Image {
    private RealImage realImage;
    private String filename;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    public void display() {
        if (realImage == null) {
            realImage = new RealImage(filename); // 필요할 때만 생성!
        }
        realImage.display();
    }
}
```
##### 사용 예시
```Java
public class Main {
    public static void main(String[] args) {
        Image image = new ProxyImage("photo.png");

        System.out.println("이미지 객체만 생성됨 (로딩 안 함)");
        image.display(); // 이제 로딩 + 표시
        image.display(); // 두 번째는 바로 표시
    }
}
```
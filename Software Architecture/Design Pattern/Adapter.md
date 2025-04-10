# Adapter
- 호환되지 않는 인터페이스끼리 연결(연동)할 수 있도록 중간에 변환기 역할을 하는 객체를 둠
- 클라이언트는 A 인터페이스만 사용 가능 -> 그러나 B 인터페이스만 갖고 있음 -> Adapter가 B를 감싸서 A처럼 보이게 해줌
### 장점
|장점|설명|
|--|--|
|재사용성 증가|기존 코드를 수정하지 않고도 재사용 가능|
|유연한 연동|인터페이스가 다른 외부 라이브러리, 레거시 코드 연결 가능|
|기존 코드 보존|원래 클래스는 그대로 두고 외부에서 감싸기만 하면 됨|
### 단점
- 클래스가 많아져 구조가 복잡해질 수 있음
- 너무 많이 쓰면 코드의 진짜 구조 파악이 어려워짐
### 실무 사용 사례
|사용 위치|설명|
|--|--|
|Spring|`HandlerAdapter`, `WebMvcConfigurerAdapter`|
|JDBC|`DataSource`와 실제 DB 커넥션 사이 어댑터|
|API 연동|외부 API 포맷을 내부 포맷으로 변환|
|레거시 코드 통합|옛날 시스템을 최신 인터페이스에 맞출 때|
### 예시
##### 클라이언트가 원하는 인터페이스
```Java
public interface UsbCCharger {
    void charge();
}
```
##### 호환되지 않는 기존 클래스
```Java
public class LightningCharger {
    public void lightningCharge() {
        System.out.println("⚡ 아이폰 라이트닝 포트로 충전 중...");
    }
}
```
##### 어댑터 클래스
```Java
public class LightningToUsbCAdapter implements UsbCCharger {
    private LightningCharger lightningCharger;

    public LightningToUsbCAdapter(LightningCharger lightningCharger) {
        this.lightningCharger = lightningCharger;
    }

    @Override
    public void charge() {
        System.out.println("🧩 어댑터를 통해 USB-C → Lightning 변환");
        lightningCharger.lightningCharge();
    }
}
```
##### 사용 예시
```Java
public class Main {
    public static void main(String[] args) {
        LightningCharger lightning = new LightningCharger();
        UsbCCharger adapter = new LightningToUsbCAdapter(lightning);

        adapter.charge(); // 클라이언트는 USB-C처럼 사용하지만, 내부는 라이트닝!
    }
}
```
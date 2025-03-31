# Exception
- 시스템 동작 중 예기치 않았던 이상 상태가 발생해 수행 중인 프로그램이 영향 받는 것
### Error와의 차이
- Error는 메모리 부족이나 스택 오버플로우 같이 발생하면 복구 할 수 없는 심각한 오류
- Exception은 발생하더라도 수습할 수 있는 비교적 덜 심각한 오류
- 프로그래머가 코드를 작성해주면 비정상적인 종료를 막을 수 있음
### Exception의 종류
#### 1. Checked Exception
- 예외 처리가 필수이며 처리하지 않으면 컴파일되지 않음
- RuntimeException 이외의 모든 예외
- Ex) IOException, SQLException 등
#### 2. UnChecked Exception
- 컴파일 할 때 체크되지 않고, Runtime에 발생하는 Exception
- RuntimeException 하위의 모든 예외
- Ex) NullPointerException, IndexOutOfBoundException 등

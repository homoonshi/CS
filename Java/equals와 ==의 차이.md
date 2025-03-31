# equals()와 ==의 차이
### ==
- 항등 연산자(Operator)
- 참조 비교 (Reference Comparison)
    - 두 객체가 같은 메모리 공간을 가리키는지 확인
- 반환 형태 (boolean)
    - 같은 주소면 true
    - 다른 주소면 false
- 모든 기본형에 적용 가능
### equals()
- 객체 비교 메서드(Method)
- 내용 비교 (Content Comparison)
    - 두 객체의 값이 같은지 확인
    - 문자열의 데이터/내용을 기반으로 비교
- 기본형 적용 X
- 반환 형태 (boolean)
    - 같은 내용이면 true
    - 다른 내용이면 false
    
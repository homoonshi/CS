# Hoisting(호이스팅)
- 자바스크립트에서 변수, 함수 선언이 코드의 최상단으로 끌어올려지는 것처럼 동작하는 현상
- 변수를 아래에서 선언했어도, 자바스크립트가 내부적으로는 미리 선언한 것처럼 처리
```JavaScript
console.log(y); // ❌ ReferenceError
let y = 10;
```
```JavaScript
sayHello(); // Hello!
function sayHello() {
  console.log("Hello!");
}
```
### 요약
|구분|호이스팅 유무|선언 전 접근 가능 유무|값 할당까지 호이스팅 되는지|
|--|---|---|---|
|var| O | 가능(값 undefined) | X |
|let / const| O | 불가능(ReferenceError)|X|
|함수 선언식|O|가능|O|
|함수 표현식|변수만|X|X|


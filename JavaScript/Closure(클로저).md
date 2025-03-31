# Closure(클로저)
- 함수가 선언될 때의 외부 변수(스코프)를 기억하고, 그 변수에 계속 접근할 수 있는 함수
- 함수 안에서 함수가 외부 변수에 접근하면 클로저
```JavaScript
function outer() {
  let x = 10;

  function inner() {
    console.log(x); // 외부 변수 x에 접근
  }

  return inner;
}

const closureFunc = outer(); 
closureFunc(); // 10
```
```JavaScript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}

const counter = createCounter();
counter(); // 1
counter(); // 2
counter(); // 3
```
### 중요한 이유
|용도|설명|
|--|--|
|데이터 은닉|외부에서 접근 못하고 내부에서만 조작 가능(캡슐화)|
|상태 유지| 함수가 호출된 이후에도 내부 상태 기억|
|콜백/이벤트 처리| 비동기 처리에서 변수 기억할 때 유용|


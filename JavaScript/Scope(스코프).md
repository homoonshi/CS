# Scope(스코프)
- 변수나 함수가 어디에 선언되었는지에 따라 접근할 수 있는 범위를 결정하는 규칙
### 전역 스코프 (Global Scope)
- 파일 전체, 혹은 함수 외부에서 선언된 변수는 어디서든 접근 가능
```JavaScript
let a = 10;

function showA() {
  console.log(a); // 10 (접근 가능)
}
showA();
```
### 함수 스코프 (Function Scope)
- 함수 안에서 선언된 변수는 그 함수 안에서만 유효
```JavaScript
function myFunc() {
  let b = 20;
  console.log(b); // 20
}
myFunc();
console.log(b); // ❌ ReferenceError
```
### 블록 스코프 (Block Scope)
- { } 블록 내부에서 선언된 변수는 그 블록 내부에서만 유효
- let과 const만 해당, var는 해당 안됨
```JavaScript
{
  let x = 5;
  const y = 10;
  console.log(x, y); // 5 10
}
console.log(x); // ❌ ReferenceError
```
### 스코프 체이닝 (Scope Chain)
- 자바스크립트는 변수를 찾을 때 현재 스코프 -> 바깥 -> 바깥.. -> 전역 순으로 찾음
```JavaScript
let a = 1;

function outer() {
  let b = 2;
  function inner() {
    let c = 3;
    console.log(a, b, c); // 1 2 3
  }
  inner();
}
outer();
```
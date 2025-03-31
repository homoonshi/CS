# JS는 비동기 언어일까?
- 기본적으로 비동기 처리를 지원함
- 싱글 스레드 언어이나, 비동기 처리를 효율적으로 다룰 수 있는 구조
    - 이벤트 루프 + 콜백 큐 + 태스크 큐
### 왜 비동기 처럼 작동할까?
- 싱글 스레드 : 한 번에 한 줄씩 실행
- 이벤트 루프 : 대기 중인 비동기 작업을 순차적으로 처리
- 콜백 큐/태스크 큐 : 비동기 작업이 끝난 후 실행될 함수들이 들어감
### 비동기 처리 방법
#### 콜백 함수
```JavaScript
setTimeout(() => {
  console.log("3초 뒤 실행");
}, 3000);
console.log("바로 실행"); 
// 출력: "바로 실행" → (3초 후) "3초 뒤 실행"
```
#### Promise
```JavaScript
const fetchData = () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("데이터 받아옴");
    }, 2000);
  });
};

fetchData().then((result) => {
  console.log(result); // "데이터 받아옴"
});
```
#### async/await
```JavaScript
const fetchData = () => {
  return new Promise((resolve) => {
    setTimeout(() => resolve("완료!"), 1000);
  });
};

async function run() {
  const result = await fetchData();
  console.log(result); // "완료!"
}
run();
```
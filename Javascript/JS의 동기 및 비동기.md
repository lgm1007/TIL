# JavaScript 에서의 동기/비동기
### 동기/비동기 프로그래밍
#### 동기(Synchronous) 프로그래밍
* 동기 프로그래밍은 코드가 순차적으로 실행되며, 한 작업이 끝나야 다음 작업이 실행되는 방식을 의미한다.
* 이는 코드의 흐름이 한 번에 하나의 작업에 집중되어 동작하며, 작업이 완료될 때까지 다른 작업을 기다려야 한다.
#### 비동기(Asynchronous) 프로그래밍
* 비동기 프로그래밍은 한 작업이 시작되고 완료되기까지 기다리지 않고, 다른 작업을 동시에 실행할 수 있는 방식이다.
* 이를 통해 시간이 오래 걸리는 작업을 기다리지 않고 다른 작업을 수행할 수 있으며, 작업이 완료되면 특정한 콜백 함수나 이벤트 핸들러를 호출하여 결과를 처리한다.
#### 구현 예제
1. 콜백 함수
```javascript
console.log("첫 번째 작업");
setTimeout(() => {
  console.log("두 번째 작업");
}, 1000);
console.log("세 번째 작업");

// 출력 결과:
// 첫 번째 작업
// 세 번째 작업
// (1초 후) 두 번째 작업
```
2. Promise
```javascript
console.log("첫 번째 작업");
new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log("두 번째 작업");
    resolve();
  }, 1000);
}).then(() => {
  console.log("세 번째 작업");
});

// 출력 결과:
// 첫 번째 작업
// (1초 후) 두 번째 작업
// 세 번째 작업
```
3. Async / Await
```javascript
async function doTasks() {
  console.log("첫 번째 작업");
  await new Promise((resolve) => setTimeout(resolve, 1000));
  console.log("두 번째 작업");
  console.log("세 번째 작업");
}
doTasks();

// 출력 결과:
// 첫 번째 작업
// (1초 후) 두 번째 작업
// 세 번째 작업
```


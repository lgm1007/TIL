# [Express] app.use()
### 미들웨어 레벨
* 미들웨어는 요청과 응답 중간에 위치한다.
* 미들웨어는 함수로, 사실상 Express에서는 거의 모든 게 미들웨어다.
* 요청과 응답을 조작하여 기능을 추가하거나 나쁜 요청을 거르기도 한다.
* 미들웨어는 상단에 있는 미들웨어가 먼저 실행되며 하단으로 갈수록 나중에 실행된다.
### 미들웨어 사용방법 중 app.use 사용법
* `app.use(미들웨어)`
  * 모든 요청에서 미들웨어 실행
* `app.use(미들웨어 사용경로, 미들웨어)`
  * "미들웨어 사용경로"로 시작되는 모든 요청에서 미들웨어 실행
* `app.METHOD(미들웨어)`
* `app.METHOD(미들웨어 사용경로, 미들웨어)`
  * `METHOD`: get, post, put 등의 http 메서드
#### 예) Express에서 정적 파일 제공
```javascript
const app = require('express');

// 프로젝트의 루트 경로 등록
app.use(express.static(__dirname));
// public 디렉토리 등록
app.use(express.static('public'));
```
* `express.static()` 인자로 전달되는 디렉토리의 밑에 있는 데이터들은 웹 브라우저의 요청에 따라 서비스를 제공해줄 수 있다.


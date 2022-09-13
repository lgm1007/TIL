# es6 문법
* **ES** : ECMAScript의 약자, 자바스크립트의 표준 규격을 나타내는 용어
* es5는 2009년에, es6는 2015년에 출시되었다.
### 1. let, const 키워드
* `let` : 블로스코프를 가지고 재선언 불가 재할당 가능한 변수 선언 키워드
* `const` : 상수 선언 키워드
* 기존 `var` 키워드만 있었을 때보다 예측 가능한 코드를 작성할 수 있음
### 2. 템플릿 리터럴
* 사용법은 ``(back tick)으로 가능
* `${}` : 중괄호 앞 달러 기호로 자바스크립트 표현식 사용이 가능함
```javascript
// es5
var str1 = ', ';
var str2 = 'World!';
var str3 = 'Hello' + str1 + str2;

// es6
let str1 = ', ';
let str2 = 'World!';
let str3 = `Hello ${str1} ${str2}`;
```
### 3. 객체 리터럴
* 이전보다 훨씬 간결한 코드를 작성할 수 있음
* 메서드에 더 이상 콜론(:)이나 function을 붙이지 않아도 됨
* 함수명이 겹치는 경우에는 한 번만 쓸 수 있음
* 객체의 프로퍼티를 동적으로 생성하려면 객체 리터럴 바깥에서 [text + 1]과 같이 선언해야 했지만, es6부터는 객체 안에서 바로 속성으로 사용할 수 있음
```javascript
const myFunc = function() {
	console.log('myFunc');
};

const text = 'TEXT';

const obj = {
	inside() {
		console.log('객체 안에 바로 함수 선언');
	},
	myFunc,
	[text + 1]: '텍스트+1'
};

obj.inside();   // 객체 안에 바로 함수 선언
obj.myFunc();   // myFunc
console.log(obj.text1);  // 텍스트+1
```
### 4. 화살표 함수
* 함수 표현식을 화살표 함수로 표현 가능
* 화살표 함수가 추가되어 함수를 간결하게 나타낼 수 있게 되어 가독성 및 유지 보수성이 올라감
* 함수의 본문에 `return`만 있는 경우 화살표 함수는 `return`과 {} 를 생략할 수 있음, 단 같이 생략해야 함
```javascript
// es5
function plus(a, b) {
	return a + b;
}

// es6
// 함수 표현식 - 화살표 함수
const plus = (a, b) => {
	return a + b;
}
// 함수 표현식 - 화살표 함수 생략형
const plus = (a, b) => a + b;
```
### 5. 구조 분해 할당
* 객체나 배열에서 사용
* 값을 해제한 뒤, 개별 값을 변수에 새로 할당하는 과정을 의미
```javascript
// 배열에서 Spread
const arr = [1, 2, 3];
const [one, two, three] = arr;

console.log(one);       // 1
console.log(two);       // 2
console.log(three);     // 3

const obj = {
	red: '빨강',
	blue: '파랑',
}
const { red, blue } = obj;

console.log(red);   // 빨강
console.log(blue);  // 파랑
```
### 6. Promise
* 자바스크립트에서 비동기 처리를 기존에는 콜백 함수를 사용한 콜백 패턴으로 처리하였다.
  * 콜백헬을 일으킴
* Promise 후속처리 메서드를 이용해 에러 처리를 효과적으로 할 수 있게 되었음
### 7. Class
* 자바스크립트는 프로토타입 기반의 언어
* 클래스 기반 객체 지향 프로그래밍도 할 수 있게 Class 키워드 도입
* 자바스크립트에서 Class는 내부적으로 프로토타입을 이용해 만들어짐
* Class는 `let`, `const` 키워드처럼 동작함

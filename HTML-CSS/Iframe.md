# Iframe
* Inline frame의 약자로, 웹페이지 안에 또 다른 웹페이지를 삽입하는 것을 의미
### 사용법
```html
<iframe src="삽입하려 하는 웹페이지 URL" title="내용"></iframe>
```
* `src`: 웹페이지의 주소를 입력
* `title`: 웹 브라우저에게 해당 iframe이 어떤 iframe 인지 알려주는 역할, 필수 속성은 아님
* **예제**
```html
<iframe src="https://wikidocs.net/" title="내용" width="50%" height="350px"></iframe>
```
### a 태그로 iframe에서 열 수 있는 여러개의 웹페이지 삽입
```html
<iframe src="삽입하고자 하는 웹피이지 URL" title="내용" name="frame"></iframe>
<a href="iframe에서 열고자 하는 웹페이지 URL 1" target="frame">웹 페이지 1</a>
<a href="iframe에서 열고자 하는 웹페이지 URL 2" target="frame">웹 페이지 2</a>
```
* iframe에서 `name` 속성으로 이름 지정
* `<a>`에서 클릭했을 때 해당 웹페이지를 iframe에서 열 수 있도록 `target` 값을 iframe의 `name` 값으로 지정
### iframe에서 부모 페이지의 Javascript 함수 호출
#### Window.postMessage()
  * 안전하게 `cross-origin` 통신을 하게 해주는 방법
    * `cross-origin` 통신(CORS = Cross-origin resource sharing): 웹페이지 상의 제한된 리소스를 최초 자원이 서비스된 도메인 밖의 다른 도메인으로부터 요청할 수 있게 허용하는 구조

# CSS 선택자
* Html 요소들을 선택해주는 요소
* 이를 통해 특정 요소들을 선택하여 스타일을 적용할 수 있게 해줌
### Rule Set
* CSS에서 스타일이 어떤 방식으로 정의되는가
* Rule Set은 Html 페이지 안의 특정 요소들을 어떻게 렌더링(Rendering)할 것인지 브라우저에게 알려주는 CSS 문장
![ruleset](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F230B463D58BE6E120B927B)
### 선택자의 종류
#### 1. 전체 선택자 (Universal Selector)
```css
/* CSS */
* { margin: 0; padding: 0; }
```
* Html페이지 내부의 모든 요소(태그)에 같은 css 속성을 적용하는 선택자
* 보통 margin이나 padding 값들을 초기화하는 등 기본값을 정해둘 때 사용
* 자주 사용하게 되면 **문서 안의 모든 요소들을 읽어내려야 하기** 때문에 페이지 로딩 속도가 느려질 수 있어 자주 사용하지 않는 것이 좋다.


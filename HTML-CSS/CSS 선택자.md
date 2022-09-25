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
#### 2. 태그 선택자 (Type Selector)
```css
/* CSS */
p { background: gray; color: darkgray; }
```
* Html 요소를 직접 지정하는 가장 간단한 선택자
#### 3. 클래스 선택자 (Class Selector)
```css
/* CSS */
.class1 { background: yellowgreen; color: darkgreen; }
div.class2 { font-size: 14px; font-weight: bold; }
```
* 주어진 값을 class 속성값으로 갖는 Html 요소를 찾아 선택
* 선택하려는 속성값 앞에 마침표를 추가해 작성
* 예제 첫번째 규칙
  * class1 이라는 class 속성값을 가진 모든 태그를 선택
* 예제 두번째 규칙
  * 마침표 앞에 태그를 붙여, 범위를 해당 태그에 한함  
##### 클래스 선택자를 사용하기 전에 고려해야 할 부분
1. class 요소 대신 사용할 수 있는 Html 태그가 있는지 확인
    * 스타일 시트를 적용하지 못하는 일부의 브라우저가 존재할 수 있기 때문
2. DOM 트리 상단에 사용되는 클래스나 ID가 있는지 확인
    * 불필요하게 많은 class 선택자를 사용할 필요는 없음
    * DOM 트리 상단에 같은 스타일을 공유하는 클래스나 ID가 있다면, 새로운 클래스 선택자를 사용할 필요 없이 함께 사용하면 됨
#### 4. ID 선택자 (ID Selector)
```css
/* CSS */
#id1 { background: yellowgreen; color: darkgreen; }
div#id2 { font-size: 14px; font-weight: bold; }
```
* 주어진 값을 id 속성값으로 갖는 Html 요소를 찾아 선택
* `#`를 사용하여 선택
##### 클래스 선택자를 사용해야 할지, ID 선택자를 사용해야 할지...?
* 한 페이지 내에서 **여러 번 반복될** 필요가 있는 스타일은 클래스 선택자를 사용, **단 한번 유일**하게 적용될 스타일은 ID 선택자를 사용한다.
    * class 속성은 어떤 분류 안에 포함된 요소의 특징을 정의하는 데 사용
    * class 속성은 속성값을 두 개 이상 가질 수 있음
      * 한 태그 내에서 여러 종류의 스타일을 정의할 수 있음
    * id 속성은 어떤 요소에 대해 유일한 특성을 정의 (Html 문서에서 특정 id 속성값은 오직 하나만 존재해야 함)
    * id 선택자의 우선순위가 클래스 선택자의 우선순위보다 더 높음
      * 우선으로 적용되어야 할 스타일을 id 선택자를 사용하여 정의하는 것이 좋음



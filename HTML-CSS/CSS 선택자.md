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
#### 5. 복합 선택자 (Combinator)
* 두 개 이상의 선택자 요소가 모인 선택자
```css
/* CSS */
/* 하위 선택자 */
section ul { border: 1px solid black; }

/* 자식 선택자 */
section>ul { border: 1px solid silver; }
```
* 태그들이 포함 관계를 가질 때 포함하는 요소를 부모 요소, 포함되는 요소를 자식 요소라고 함
* **하위 선택자**
  * 부모 요소에 포함된 `모든` 하위 요소에 스타일을 적용
* **자식 선택자**
  * 부모의 `바로 아래` 자식 요소에만 적용

```css
/* CSS */
/* 인접 형제 선택자 */
h1+ul { background: white; color: darkgray; }

/* 일반 형제 선택자 */
h1~ul { background: white; color: silver; }
```
* 같은 부모 요소를 가지는 요소들을 형제 관계라고 함
* 먼저 나오는 요소를 형 요소, 나중에 나오는 요소를 동생 요소라고 함
* **인접** 형제 선택자
  * 형제 중 `첫 번째` 동생 요소가 조건을 충족시킬 때만 스타일을 적용
* **일반** 형제 선택자
  * 조건을 충족하는 `모든` 동생 요소에 스타일을 적용
#### 6. 속성 선택자 (Attribute Selectors)
* Html 태그 안의 특정 속성들에 따라 선택하는 선택자
* 속성 값의 조건에 따라 다양한 스타일을 지정할 수 있음
```css
/* CSS */
/* Element[attr] 형식 */
a[href] { background: yellow; color: black; }

/* Element[attr="val"] 형식 */
input[type="text"] { background: #fff; border: 1px solid #ddd; }

/* Element[attr$="val"] 형식 */
a[href$=".xls"] { background: darkgreen; }

/* HTML 파트 */
<a href="index.html">Element[attr] 형식</a>
<input type="text" name="name" />
<a href="excels.xls">Element[attr$="val"] 형식</a>
```
* 속성 선택자는 모두 앞 쪽에 태그명과 대괄호(`[]`) 사이에 속성에 관련된 내용을 넣어 선택
* `E[attr="val"]` : 속성과 속성값이 완벽하게 일치하는 경우 선택
* `E[attr~="val"]` : 띄어쓰기를 통해 여러 개 올 수 있는 속성값 중 하나만 일치해도 선택
* `E[attr^="val"]` : "val" 으로 시작하는 속성값을 선택
* `E[attr$="val"]` : "val" 으로 끝나는 속성값을 선택
#### 7. 가상 클래스 선택자 (Pseudo-Classes Selectors)
* 웹 문서상에 실제로 존재하지 않지만 필요에 의해 임의로 가상의 선택자를 지정하는 선택자
* **링크 선택자(Link pseudo-classes)와 동적 선택자(user action pseudo-classes)**
  * `E::link` : 방문하지 않은 링크 E 선택
  * `E::visited` : 방문한 링크 E 선택
  * `E::active` : E 요소에 마우스 클릭 또는 키보드 엔터가 눌린 동안 E 선택
  * `E::visited` : E 요소에 마우스가 올라가 있는 동안 E 선택
  * `E::focus` : E 요소에 포커스가 머물러 있는 동안 E 선택
* **구조적 가상 클래스 선택자 (Structural pseudo-classes)**
  * `E::root` : 문서의 최상위 요소(html)를 선택
  * `E::nth-child(n)` : 앞으로부터 지정된 순서와 일치하는 요소가 E라면 선택 (E가 아닌 요소의 순서가 계산에 포함)
  * `E::nth-last-child(n)` : 뒤로부터 지정된 순서와 일치하는 요소가 E라면 선택 (E가 아닌 요소의 순서가 계산에 포함)
  * `E::nth-of-type(n)` : E 요소 중 앞으로부터 순서가 일치하는 E 요소를 선택 (E 요소의 순서만 계산에 포함)
  * `E::nth-last-of-type(n)` : E 요소 중 끝으로부터 순서가 일치하는 E 요소를 선택 (E 요소의 순서만 계산에 포함)
  * `E::first-child` : 첫 번째 등장하는 요소가 E라면 선택
  * `E::last-child` : 마지막에 등장하는 요소가 E라면 선택
  * `E::first-of-type` : E 요소 중 첫 번째 E를 선택
  * `E::last-of-type` : E 요소 중 마지막 E를 선택
  * `E::only-child` : E 요소가 유일한 자식이면 선택
  * `E::only-of-type` : E 요소가 유일한 타입이면 선택
  * `E::empty` : 텍스트 및 공백을 포함하여 자식 요소가 없는 E를 선택
* **언어 선택자 (Language pseudo-classes)**
  * `E::lang(ko)` : HTML lang 속성의 값이 'ko'으로 지정된 요소를 선택
* **부정 선택자 (Negation pseudo-classes)**
  * `E::not(S)` : S가 아닌 E 요소를 선택
* **목적 선택자 (The target pseudo-classes)**
  * `E::target` : E의 uri가 요청되면 선택 (E는 id가 지정되어 있어야 함)
* **UI 요소 상태 선택자 (The UI element states pseudo-classes)**
  * `E::enabled` : 사용 가능한 폼 콘트롤 (input, textarea, select, button) E를 선택
  * `E::disabled` : 사용 불가능한 폼 콘트롤 (input, textarea, select, button) E를 선택
  * `E::checked` : 선택된 폼 콘트롤 (input checked="checked")을 선택
* **가상 엘리먼트 선택자 (Pseudo-Elements)**
  * `E::first-line` : E 요소의 첫 번째 라인을 선택
  * `E::first-letter` : E 요소의 첫 번째 문자를 선택
  * `E::before` : E 요소의 시작 지점에 생성된 요소를 선택
  * `E::after` : E 요소의 끝 지점에 생성된 요소를 선택
### 선택자의 우선순위
* 스타일 시트는 다음 3가지의 CSS 원천 소스(Original Source)를 가질 수 있음
  * 제작자(author) 원천 소스 : 웹 사이트 제작자가 지정하는 자신의 페이지 스타일
  * 사용자(user) 원천 소스 : 사용자가 직접 정하는 자신이 사용할 스타일
  * 사용자 도구(user agent) 원천 소스 : 웹 브라우저가 자체에 지정된 기본 스타일
* 스타일을 적용하는 데 동시에 여러 가지 방법을 사용하게 되면 스타일이 충돌할 수 있음
  * 원천 소스간은 물론, 선택자 간의 우선순위를 알아야 할 필요
#### 1) 원천 소스 우선 순위
```
!important 선언을 한 사용자 스타일 > !important 선언을 한 제작자 스타일 > 제작자 스타일 > 사용자 스타일 > 사용자 도구 선언 (브라우저 자체의 선언)
```
#### 2) CSS 명시도(Specificity) 계산
```
!important > id [ 100 ] > class [ 10 ] > tag [ 1 ] > * [ 0 ]
```
* `!important` 선언을 사용한 형식이 우선 순위가 가장 높음
  * 꼭 필요한 곳에만 사용해야 함
* 나머지 선택자는 대괄호 `[]` 안에 숫자를 각각의 점수로 부여하여 우선순위를 계산
  * (예시설명) ID 선택자의 개수를 세어 개당 100으로 계산
  * 클래스 선택자의 개수를 세어 개당 10으로 계산
  * 태그 선택자의 개수를 세어 개당 1로 계산
  * 공용 선택자는 모두 0으로 계산
  * 가상 엘리먼트는 무시
#### 3) 마지막에 지정된 스타일을 우선 적용
* 스타일 우선순위가 같거나, 계산 방법이 없는 경우 **마지막에 지정된 스타일이 우선**으로 적용됨



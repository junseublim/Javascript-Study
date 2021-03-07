# 문서제어

## DOM Tree

웹 페이지의 내용은 Document 객체가 관리한다. 웹 브라우저가 웹 페잊를 읽어 들이면 렌더링 엔진은 웹 페이지의 HTML 문서 구문을 해석하고 Document 객체에서 문서 내용을 관리하는 DOM 트리라고 하는 객체의 트리 구조를 만든다. 

DOM 트리를 구성하는 객체 하나를 노드라고 한다. 노드에는 기본적으로 세가지 종류가 있다.

- 문서 노드 : 전체 문서를 가리키는 Document 객체. document로 참조.
- HTML 요소 노드 : HTML 요소를 가리키는 객체
- 텍스트 노드 : 텍스트를 가리키는 객체

HTML은 요소 뒤에 공백 문자가 여러 개 있어도 무시한다. 그러나 DOM 트리는 요소 앞뒤에서 연속적인 공백 문자를 발견하면 텍스트롤 취급하여 텍스트 노드로 생성한다. 이렇게 공백 문자만으로 구성된 텍스트 노드를 공배 노드라고 한다. 단, html 요소 안에 있는 첫 공백 문자와 마지막 공백 문자에 대해서는 공백 노드를 생성하지 않는다.

### 노드 객체의 프로퍼티

| Property        | Explanation                                              |
|-----------------|----------------------------------------------------------|
| parentNode      | refers parent node. parent node of document is null.     |
| childNodes      | NodeList of its children                                 |
| firstChild      | first child. null if none.                               |
| lastChild       | last child. null if none.                                |
| nextSibling     | next node with same parent                               |
| previousSibling | previous node with same parent                           |
| nodeValue       | text contents of text node. null for element node.       |
| nodeName        | capitalized name for element node. '#text' for text node |

### html 요소의 트리

HTML 문서 안의 요소에만 관심이 있는 경우에도 childNodes와 firstChild로 노드를 참조하면 공백 문자의 유무에 따라 검색 방법을 바꿔야 해서 불편하다. 그래서 각 노드에는 DOM 트리 안의 텍스트 노드를 무시하고 HTML 문서에서 요소의 계층 구조만 가져 오기 위한 프로퍼티가 마련되어 있다. 

- childNodes
- parentElement
- firstElementChild
- lastElementChild
- nextElementSibling
- previousElementSibling
- childElementCount

## 노드 객체 가져오기

자바스크립트로 HTML 요소를 제어하려면 그 전에 제어하고자 하는 요소 객체를 먼저 가져와야 한다.

### id 속성으로 가져오기

```js
document.getElementById('id');
```

### 요소 이름으로 가져오기

```js
document.getElementsByTagName('tag');
//Nodelist를 반환한다.

document.getElementsByTagName("*");
//와일드 카드 지정. 모든 요소를 담아서 반환한다.
```

### class 속성으로 가져오기

```js
document.getElementsByClassName('class');

document.getElementsByClassName('class1 class2');
//둘다 있는 요소들 반환
```

### name 속성으로 가져오기

```js
document.getElementsByName('name');
```

### CSS 선택자로 가져오기

1. querySelectorAll : 인수로 넘긴 선택자와 일치하는 요소 객체가 담긴 NodeList를 가져올 수 있다. 
    - 반환값은 '살아 있는 상태'가 아니다. NodeList에 포함된 요소는 메서드를 호출한 시점에 일치한 요소이다. HTML 문서의 내용이 바뀌어도 NodeList는 바뀌지 않는다,. 이는 앞에서 배운 getElementsBy~ 메서드들과 다른 점이다.
2. querySelector : 선택자와 일치하는 요소 객체 중에서 첫번째 요소 객체를 반환한다. 

## 속성 값의 읽기와 쓰기

### 요소의 속성 값

대부분의 HTML 요소에는 속성을 설정해서 추가적인 정보를 더할 수 있다. 

요소 객체의 일반적인 html 속성을 읽기 위해서는 다음과 같이 표기한다.
```js
요소 객체.속성 이름
```
이 프로퍼티는 읽기, 쓰기 모두 가능하다. 

Html 요소의 속성 이름은 대소문자를 구분하지 않지만 자바스크립트 요소 객체의 속성 프로퍼티는 대소문자를 구분한다. HTML 요소의 속성을 요소 객체의 속성 프로퍼티로 작성할때는 카멜 표기법으로 작성한다.

### 속성 값 가져오기

```js
요소 객체.getAttribute(속성의 이름)
```

요소의 속성을 가져온다. 해당 속성이 없을 경우 null 또는 빈 문자열을 반환한다.

### 속성 값 설정하기

```js
요소 객체.setAttribute(속성 이름, 속성 값)
```

getAttribute, setAttribute는 요소 객체의 속성 프로퍼티에 비해 다음과 같은 장점이 있다.
- HTML 일반적인 속성을 물론 모든 속성을 설정할 수 있다.
- 속성 이름을 프로그램 실행 중에 동적으로 설정할 수 있다.

### 속성 있는지 확인

```js
요소 객체.hasAttribute(속성 이름)
```

### 속성 삭제하기
```js
요소 객체.removeAttribute(속성 이름)
```

### 전체 속성의 목록 가져오기

요소 객체에는 attributes 프로퍼티가 정의되어 있다. 이 프로퍼티는 NameNodeMap 객체로 그 요소에 설정된 모든 속성의 속성 노드 객체가 담겨 있다. 또한 NamedNodeMap 객체는 유사 배열 객체이명 읽기 전용이다. NamedNodeMap 객체의 요소인 속성 노드 객체의 name 프로퍼티에는 속성 이름이 담겨 있으며, value 프로퍼티에는 속성 값이 담겨 있다. 


## HTML 요소의 내용 읽고 쓰기

### innerHTML

요소 안의 html 코드를 가리킨다. html 코드를 읽거나 쓸 수 있다.

### textContent, innerText

textContent 프로퍼티는 요소의 내용을 웹 페이지에 표시했을 때의 텍스트 정보를 표시한다. textContent 프로퍼티 값은 지정한 요소의 자식 노드인 모든 텍스트 노드를 연결한 값이다.

innterText는 다음과 같은 차이점이 있다.
- textContent는 script 요소 안의 텍스트를 반환하지만 innerText는 반환하지 않는다.
- textContent는 공백 문자를 그대로 반환하지만 innerText는 남는 공백 문자를 제거한다.
- innerText는 table, tbody, tr 요소 등의 테이블 요소를 수정할 수 없다.

## 노드 생성/삽입/삭제

### 노드 생성

```js
var element = document.createElement(요소의 이름); //요소 노드
var element = document.createTextNode(텍스트); //텍스트 노드
```

createElement로 생성한 노드 객체는 메모리에 생성되어 있을 뿐 문서의 DOM트리와는 아무런 관계가 없다. 

### 노드 삽입

#### 요소의 마지막에 삽입하기

해당 요소의 마지막 자식으로 노드 객체를 삽입한다.
```js
요소 노드.appendChild(삽입할 노드)
```
appendChild 메서드로 노드 객체를 삽입하면 그 객체가 DOM 트리에 추가되고 DOM 트리의 각 노드 계층 구조를 정의하는 프로퍼티(parentNode, childNode)가 바뀐다.

#### 지정한 자식 노드의 바로 앞에 삽입하기: insertBefore

```js
요소 노드.insertBefore(삽입할 노드, 자식 노드)
```

#### 노드 옮기기

이미 있는 노드를 appendChild, insertBefore 메서드로 문서에 삽입하면 해당 노드를 현재 위치에서 삭제하고 새로운 위치에 삽입한다. 결과적으로 위치를 옮기게 된다.


### 노드 삭제하기

```js
노드.removeChild(자식 노드)
```

이 떼 삭제할 수 있는 노드가 해당 노드의 자식 노드이다. 즉 삭제하려는 노드의 부모 노드 객체에서 removeChild 메서드를 호출한다. 특정 노드를 삭제하고자 할때는 다음과 같이 작성한다.

```js
node.parentNode.removeChild(node);
```

### 노드 치환하기
```js
노드.replaceChild(새로운 노드, 자식 노드);
```
자식 노드를 제거하고 새로운 노드로 치환한다. 부모 노드에서 자식 노드만 치환할 수 있다.

## HTML 요소의 위치 

### HTML 요소의 위치를 표현하는 좌표계

요소 위치를 표현하기 위한 좌표계에는 뷰포트 좌표계와 문서 좌표계가 있다. 두 좌표계 모두 좌표축의 길이 단위로 픽셀을 사용한다. 

1. 뷰 포트 좌표계 : 뷰 표트의 왼쪽 위 꼭짓점을 원점으로 하는 좌표계이다. 뷰 포트란 웹 브라우저에서 문서의 내용을 표시하는 영역을 말하며, 메뉴, 도구 모음, 탭 등을 포함하지 않는다.

2. 문서 좌표계 : 문서의 왼쪽 위 꼭짓점을 원점으로 하는 좌표계이다. 문서는 웹 브라우저 표시 영역 안에 표시되는데, 문서를 스크롤하면 문서 좌표계의 원점이 뷰 포트를 따라 이동한다. 문서 좌표계를 따르는 요소는 사용자가 문서를 스크롤해도 바뀌지 않는다. 

### HTML 요소의 위치와 크기 구하기

요소 객체의 getBoundingClientRect 메서드는 뷰 포트 좌표계로 측정한 해당 요소의 보더 박스 위치와 크기 정보를 담은 객체를 반환한다.

```js
var rect = 요소 객체.getBoundingClientRect();
```
ClientRect 객체를 반환하며 다음과 같은 프로퍼티를 갖고있다.
- left : 요소 박스의 왼쪽 위 꼭짓점의 X좌표
- top : 요소 박스의 왼쪽 위 꼭짓점의 Y좌표
- right : 요소 박스의 오른쪽 아래 꼭짓점의 X좌표
- bottom : 요소 박스의 오른쪽 아래 꼭짓점의 Y좌표
- width : 요소 박스의 너비
- height : 요소 박스의 높이

### 뷰포트의 크기 가져오기
```js
document.documentElement.clientWidth // 뷰 포트의 너비
document.documentElement.clientHeight // 뷰 포트의 높이

//IE9 이후 웹 브라우저는 다음도 지원한다
window.innerWidth // 뷰 포트의 너비
window.innerHeight // 뷰 포트의 높이
```

### 스크롤한 거리 구하기

문서의 뷰 포트를 스크롤한 거리를 제공하는 프로퍼티는 브라우저마다 다르다.

1. 인터넷 익스플로러, 파이어폭스
    ```js
    document.documentElement.scrollLeft 
    document.documentElement.scrollTop
    ```
2. 크롬, 사파리, 오페라, 엣지 등
    ```js
    document.body.scrollLeft 
    document.body.scrollTop
    ```
3. 크롬, 파이어폭스, 사파리, 오페라, 엣지, IE9 이상
    ```js
    window.pageXOffset
    window.pageYOffset
    ```

### 스크롤하기

#### 특정 위치로 스크롤하기

Window 객체의 scrollTo 메서드는 문서 좌표를 인수로 받으며, 뷰 포트 좌표의 원점까지 스크롤한다.
```js
window.scrollTo(X,Y);
```

#### 특정 거리만큼 스크롤하기
Window 객체의 scrollBy 메서드는 스크롤할 거리를 인수로 받아 문서를 그 거리만큼 스크롤한다.
```js
window.scrollBy(dx,dy);

//한 페이지 분량만큼 스크롤하기
window.scrollBy(0, window.innerHeight);
```

#### 특정 요소가 있는 위치까지 스크롤하기

요소 객체의 scrollIntoView(alignWithTop) 메서드는 그 요소가 웹 브라우저의 표시 영역에 들어올 때까지 스크롤한다.
```js
요소 객체.scrollIntoView(alignWithTop);
```
인수 alignWithTop은 생략되면 true로 간주. true일 경우 요소가 표시 영역의 위쪽 끝에 오도록 스크롤한다. false면 요소가 표시 영역의 아래쪽 끝에 오도록 스크롤한다.


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

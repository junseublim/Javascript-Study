# 웹 브라우저의 객체

## 클라이언트 측 자바스크립트

### 웹브라워에서 자바스크립트가 하는일

자바 스크립트는 웹 브라우저에서 다음의 일을 한다. 

1. 웹 페이지의 Document 객체 제어(HTML 요소와 CSS 스타일 작업)
    - DOM API를 활용
2. 웹 페이지의 Window 객체 제어 및 브라우저 제어
    - 웹 브라우저에  내장된 다양한 객체(Location, Navigator)를 활용한다.
3. 웹 페이시에서 발생하는 이벤트 처리
4. HTTP를 이용한 통신 제어
    - XMLHttpRequest 객체를 활용한다.

### 자바스크립트 코드 삽입 방법

1. script 요소의 내용물로 작성하기
    ```html
    <script>
        console.log("Hello")
    </script>
    ```
2. 외부 파일 읽어 들이기
    ```html
    <script src="../scripts/sample.js"></script>
    ```
3. 이벤트 처리기 속성에 작성하기
    ```html
    <input type="button" value="click" onclick="console.log('Hello!');">
    ```
4. JavsScript: URL(자바스크립트 의사 프로토콜)
    ```html
    <a href="javscript:console.log('Hello');">
    ```

3, 4는 거의 사용하지 않는다.

### 웹 브라우저에서의 자바스크립트 실행 순서

웹 브라우저에서 HTML 문서를 분석하고 표시하는 프로그램을 가리켜 렌더링 엔진이라고 한다. 렌더링 엔진은 다음과 같은 처리 과정을 거쳐 HTML 문서의 구문을 분석하고 DOM 트리를 구축한 다음 HTML 안에 지정된 자바스크립트 코드를 실행한다.

1. 웹 브라우저로 웹 페이지를 열면 가장 먼저 Window 객체가 생성된다. Window 객체는 웹 페이지의 전역 객체로 웹 페이지와 탭마다 생성된다.

2. Document 객체가 Window 객체의 프로퍼티로 생성되며 웹 페이지를 해석해서 DOM 트리의 구축을 시도한다. Document 객체는 readyState 프로퍼티를 가지고 있으며, 이 프로퍼티에는 HTML 문서의 해석 상태를 뜻하는 문자열이 저장된다. readyState의 초깃값은 "loading"이다.

3. HTML 문서는 구문을 작성 순서에 따라 분석하며 Document 객체 요소와 텍스트 노드를 추가해 나간다.

4. HTML 문서 안에 script 요소가 있으면 script 요소 안의 코드 또는 외부 파일에 저장된 코드의 구문을 분석한다. 그 결과 오류가 발생하지 않으면 그 시점에 코드를 실행한다. 이 때 script 요소는 동기적으로 실행된다. 즉, script 요소의 구문을 분석해서 실행할 때는 HTML 문서의 구문 분석이 일시적으로 막히고, 자바스크립트 코드의 실행을 완료한 후에 HTML 문서의 구문 분석을 재개한다.

5. HTML 문서의 모든 내용을 읽은 후에 DOM 트리 구축을 완료하면 document.readyState 프로퍼티 값이 "interactive"로 바뀐다. 

6. 웹 브라우저는 Document 객체에 DOM 트리 구축 완료를 알리기 위해 DOMContentLoaded 이벤트를 발생시킨다. 

7. img 등의 요소가 이미지 파일 등의 외부 리소스를 읽는다.

8. document.readyState가 complete으로 바뀐다. 마지막으로 웹 브라우저는 Window 객체를 상대로 load 이벤트를 발생시킨다.

9. 이 시점부터 다양한 이벤트를 수신하며, 이벤트가 발생하면 이벤트 처리기가 비동기로 처리된다.

window.load 이벤트는 모든 리소스를 다 읽어 들인 후에 발생한다. HTML 문서가 다 읽어지기 전에 자바스크립트로 HTML 요소를 조작하려면 객체가 없으므로 제대로 작동하지 않는다. 따라서 window.onload 이벤트 처리기에 초기화 스크립트를 등록한다.

이미지 등의 외부 리소스를 읽어 들이는 시점은 DOM 트리 구축이 끝난 후이다. 따라서 load 이벤트는 리소스를 읽는 시간만큼 사용자가 기다려야 하는 시간도 길어진다. 이를 방지하기 위해 load 대신 DOMContentLoaded 이벤트의 이벤트 처리기에 초기화 작업을 작성한 함수를 등록하면 사용자가 오래 기다리지 않고도 웹 페이지를 조작할 수 있다.

```js
document.addEventListener("DOMContentLoaded", function(e){
    // 초기화 작업
},false);
```

#### async , defer

async와 defer 속성은 script 요소의 논리 속성으로, html5부터 추가되었다. 둘다 script 요소에 적용할 수 있지만 인라인 스크립트에는 사용할 수 없다. 이 속성을 사용하면 자바스크립트 코드를 실행할 때 HTML 구문 분석을 막지 않는다. 

async 속성을 설정하면 코드가 비동기적으로 실행된다. 즉, html 문서의 구문 분석 처리를 막지 않으며 script 요소의 코드를 최대한 빨리 실행한다. 여러개의 script 요소에 async 속성을 설정하면 다 읽어 들인 코드부터 비동기적으로 실행하므로 실행 순서가 보장 되지 않는다. 읽어 들이는 순서에 의존하는 script 요소에는 설정하지 말아야 한다.

defer 속성을 설정하면 script 요소가 DOM 트리 구축이 끝난 후에 실행된다. 따라서 defer 속성은 DOMContentLoaded 이벤트의 대안으로 활용할 수 있다.

async, defer 속성이 설정된 script 요소에 document.write 메서드가 있으면 async와 defer 속성이 무시되어 동기적으로 실행된다.

## Window 객체

Window 객체는 전역 객체이며, 전역 변수는 Window 객체의 프로퍼티이다.
또한 웹 브라우저에서 사용할 수 있는 다양한 객체가 모두 Window 객체의 프로퍼티이다. 
Window 객체의 모든 프로퍼티 및 메서드는 'window.'를 생략할 수 있다. 

### Location 객체

Location 객체는 창에 표시되는 문서의 URL을 관리한다. 

### History 객체

History 객체는 창의 웹 페이지 열람 이력을 관리한다.

### Navigator 객체 

Navigator 객체는 스크립트가 실행 중인 웹 브라우저 등의 애플리케이션 정보를 관리한다.

### Screen 객체

Screen 객체는 화면 크기와 색상 등의 등의 정보를 관리한다.

### Document 객체

Document 객체는 창에 표시되고 있는 웹 페이지를 관리한다. 이 객체로 웹 페이지의 내용물인 DOM 트리를 읽거나 쓸 수 있다.

### 창 제어하기 

#### 창 여닫기
새로운 창, 탭을 열때는 open 메서드를 사용한다.

```js
var w = open(url, 창의 이름, 옵션);
```

- url: 새롭게 여는 창이 읽어 들이는 문서의 URL.
- 창의 이름: 새로운 창의 이름. 이미 같은 이름이 있다면 새로 열시 않고 그 창에 표시한다.
- 옵션: 새로운 창의 설정 값(창의 크기 등).

닫기 위해서는 close 메서드를 사용한다.

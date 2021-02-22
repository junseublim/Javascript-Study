
## Chapter 6: 웹브라우저에서의 입출력

### 대화상자
- window.alert : 경고 메세지를 표시
- window.prompt : 입력 대화상자를 표시
- window.confirm : 확인과 취소 버튼이 있다. 확인 클릭시 true, 취소 클릭시 false 반환.

window. 부분을 생략하고 호출할 수 있다.

### console

Console 객체의 주요 메서드
1. console.dir : 객체의 대화형 목록을 출력
2. console.error : 오류 메시지 출력
3. console.info : 메시지 타입 로그를 출력
4. console.log : 일반 로그를 출력
5. console.time : 처리 시간 측정용 타이머를 시작
6. console.timeEnd : 처리 사건 측정용 타이머를 정지시키고 타이머를 시작한 후에 흐른 시간을 밀리초 단위로 출력
7. console.trace : 스택 트레이스를 출력
8. console.warn : 경고 메시지를 출력


### 이벤트 처리기

웹 브라우저에서 동작하는 프로그램은 기본적으로 이벤트 주도형 프로그램이다. 이벤트란 사용자가 버튼을 클릭하는 행위처럼 단말기와 애플리케이션이 처리할 수 있는 동작이나 사건을 뜻한다. 이벤트 주도형 프로그램이란 이벤트가 발생할 때까지 기다렸다가 이벤트가 발생했을 때 미리 등록한 작업을 수행하는 프로그램을 말한다.

함수를 이벤트 처리기로 등록하는 방법은 3가지가 있다.

### HTML 요소의 속성으로 등록하기

```html
<input type="button" value="click" onclick="displayTime()">
```

html 요소에서 속성으로 직접 이벤트 처리기를 추가하는 방법이다. 이벤트 처리기 속성에는 이벤트가 발생했을 때 실행할 자바스크립트 문장을 문자열로 만들어 대입한다. 속성에 문장을 여러 개 작성하고자 할 때는 문장과 문장을 세미콜론으로 구분한다. 

이벤트 처리기 속성을 사용하는 방식은 HTML 코드와 자바스크립트 코드가 뒤섞이는 단점이 있다. 따라서 다른 방법들이 더 선호된다.

### DOM에서 가져온 HTML 요소에 이벤트 처리기 지정하기

DOM은 자바스크립트에서 HTML 요소를 조작할 수 있게 하는 인터페이스이다. 

#### DOM 객체

DOM에서는 HTML 문서나 HTML 요소를 가리키는 객체로 자바스크립트를 사용하여 문서를 조작한다. DOM의 주요 객체는 다음과 같이 분류할 수 있다.
- window: Window 객체, 웹 브라우저 윈도우 하나 혹은 탭 하나를 가리킨다.
- document: Document 객체, HTML 문서 전체를 가리킨다. HTML 문서에서 HTML 요소 객체를 가져오거나 HTML 요소를 새로 만드는 등 HTML 문서 전반에 걸친 기능을 제공한다.
- 요소 객체: HTML 문서의 요소를 가리키는 객체

DOM을 사용해서 이벤트 처리기 등록하는 방법은 다음과 같다.

1. window.onload를 사용해서 HTML 문서를 다 읽어 들인다.
2. document.getElementById 메서드를 사용하여 특정 id 속성 값을 가진 HTML 요소의 요소 객체를 가져온다.
3. 요소 객체의 이벤트 처리기 프로퍼티에 이벤트 처리기로 동작할 함수를 등록한다.

```js
function displayTime() {
    var d = new Date();
    console.log(d.toLocaleString());
}
window.onload = function() {
    var button = document.getElementById('button');
    button.onclick = displayTime;
}
```

이벤트 처리기 제거하기 위해서는 null을 등록하면 된다.

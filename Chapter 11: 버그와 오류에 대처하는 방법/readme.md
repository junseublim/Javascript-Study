# 버그에 대처하기

## Strict Mode

Strict Mode는 자바스크립트 언어의 사양 중에서 버그를 일으키기 쉬운 부분을 제거한다. 이는 버그를 최대한 발생하지 않게 하거나 버그가 발생했을 때 즉시 알 수 있도록 언어의 사양을 더욱 엄격하게 제한한다.

### Strict mode 설정

스크립트의 첫머리 (모든 문장 앞에), 또는 함수의 첫머리(모든 문장 앞에)에 "use strict";를 입력한다.
```js
function f(x) {
    "use strict";
    y = x;
}
f(2);
// -> Uncaught ReferenceError: y is not defined
```

함수 몸통의 첫머리에 입력하면 그 함수가 Strict mode로 동작한다. 그 함수에 중첩된 함수도 Strict mode로 동작한다.

스크립트 여러 개 있을 경우에는 Strict 모드는 모든 스크립트에 반영되지 않고 스크립트 단위로 적용된다.
```html
<script>
    "use script"; // Strict mode
</script>
<script>
    x = 2; // Strict mode 아님
</script>
```

### Strict Mode 시 바뀌는 점

1. 변수는 모드 선언해야만 한다. 선언되지 않은 변수, 함수, 함수의 인자에 값을 대입하면 ReferenceError 발생한다.
2. 함수를 직접 호출할 때, 함수 안의 this 값이 undefined가 된다. 비 Strict 모드에서는 함수 안의 this 값이 전역 객체의 참조가 된다.
3. with 문은 사용 불가능하다.
4. 함수 정의문에 같은 이름의 인수가 있으면 문법 오류
5. 객체에 같은 이름의 프로퍼티가 있으면 문법 오류 발생
6. NaN, Infinity, undefined를 표기하면 TypeError 발생
7. arguments[i]는 호출되었을 때의 인수 값을 유지한다. 비 Strict 모드에서는 arguments[i]가 인자의 별명이다. 따라서 한쪽을 수정하면 다른 쪽도 바뀐다.
8. arguments.callee를 읽을 수 없다.
9. eval로 실행한 코드는 호출자의 유효 범위 안에 새로운 변수, 함수를 선언할 수 없다.

### 스타일 가이드 활용하기

스타일 가이트란 프로그램을 작성할 때 버그를 피하고 가독성을 높이기 위해 권장되는 코딩 규칙을 정리한 것이다. 특히 여러 사람이 함께 프로그램을 개발한다면 스타일 가이드를 참고하여 구체적인 코딩 규칙을 정해 두는 것이 좋다.

1. Google JavaScript Style Guide
2. JavaScript style guide - MDN Docs
3. JQuery JavaScript Style Guide
4. Airbnb JavaScript Style Guide


## 예외 처리

### throw 문
```js
throw 표현식;
```
표현식은 어떤 타입의 값도 지정할 수 있다. 사용자에게 표시할 오류 메시지가 포함된 문자열이나 오류 코드를 의미하는 숫자도 허용된다. 그러나 일반적으로는 Error 객체나 Error 객체를 상속받은 객체를 지정한다.

#### 에러 객체
자바스크립트에는 예외를 표현하기 위한 내장 객체가 7개가 있다. 그중 Error 객체는 범용적인 예외를 표현하기 위한 객체이고 나머지는 특정 예외가 발생했을 때 표현하기 위한 객체이다.

1. Error : 범용적인 예외 객체
2. EvalError: eval 함수와 관련해서 발생하는 예외 객체
3. RangeError: 숫자 값이 허용 범위 벗어났을 떄
4. ReferenceError: 잘못된 참조 만났을 때
5. SyntaxError: 문법에 어긋났을 때
6. TypeError: 변수 및 인수 타입이 유효하지 않을 때
7. URIError: encodeURI와 decodeURI 메서드에 잘못된 인수가 전달되었을 떄

모든 에러 객체는 Error.prototype의 프로퍼티와 메서드를 상속받는다. 

- message : 오류 메세지를 뜻하는 문자열
- name : 오류 이름을 뜻하는 문자열
- toString() : 지정된 객체를 표현하는 문자열을 반환
```js
var error = New TypeError("배열이 아닙니다")
console.log(error.message) // 배열이 아닙니다
console.log(error.name) // TypeError
console.log(error.toString()) // TypeError : 배여리 아닙니다
```

#### try/catch/finally

1. try 블록 안에서 예외가 발생하면 catch로 예외를 넘긴다.
2. catch 블록 안에서는 try 블록에서 발생한 예외를 처리한다. catch 블록뒤의 괄호 안에는 변수를 입력할 수 있다. 그 변수에는 try 블록에서 던진 예외 값이 들어온다.
3. finally 블록 안의 코드는 예외 처리의 마지막에 반드시 실행된다. 

##### 예외가 여러개 발생했을 경우 대처

cath 블록안에서 예외 유형별로 처리를 작성해주면 도니다. instanceof 연산자로 예외 차입을 판별한다.

```js
try {
    //오류 발생
}
catch(e) {
    if (e instanceof TypeError) {
        // TypeError시 처리
    }
    else if (e instanceof ReferenceError) {
        // ReferenceError시 처리
    }
}
```
#### 예외의 전파
예외는 호출 스택을 거슬러 올라가며 전파된다. 호출 스택에서 예외 처리기를 찾지 못하면 프로그램이 강제로 종료되며 예외는 사요자 오류로 보고된다.

#### 비동기 처리의 콜백 함수가 던진 예외의 처리

비동기 처리의 콜백 함수가 던진 예외는 콜백 함수을 넘긴 함수로 전파되지 않는다.
```js
try {
    setTimeout(function throwError() {
        throw new Error("오류 발생");
    }, 1000);
} catch(e) {
    console.log(e);
}
```
이 코드를 실행하면 예외는 캐치되지 않고 프로그램이 종료된다. 예외를 던지는 콜백 함수는 함수 정의가 try 블록안에 있을 뿐 try 블록안에서 호출 된 것이 아니다. 이 콜백 함수를 호출한 주체는 타이머 이벤트이며 try 블록 안에서 발생한 예외가 아니다. 

제너레이터를 활용하면 비동기 처리 중에 발생한 예외를 잡을 수 있다. 
```js
function sleepAndError(g,n) {
    setTimeout(function() {
        for (var i=0; i<n; i++) console.log(i);
        g.throw(new Error("오류 발생"));
    }, 1000);
}

function run(callback, ...argsList) {
    var g = (function* (cb, args) {
        try {
            yield cb(g, ...args);
        }
        catch(e) {
            console.log("예외를 잡음 -> ", e);
        }
    })(callback, argsList);
    g.next();
}

run(sleepAndError,10);
```
sleepAndError 함수의 실행을 run 함수에 위임한다. sleepAndError 함수는 setTimeOut 함수를 사용해서 약 1초후에 예외를 던지는 비동기 처리를 하는 함수이다. sleepAndError 함수가 예외를 던질 때 그 예외를 throw 문이 아니라 첫 번째 인수로 받은 제너레이터 객체 g의 throw메서드에 던지는 부분이 포인트이다. 제너레이터 객체 g의 throw 메서드는 호출된 그 자리에서 예외를 던지는 것이 아니라 제너레이터를 한번 동작시킨 다음에 yield 문이 위치한 자리에서 예외를 던진다. 그러면 이 예외는 제너레이터가 던진 예외가 되므로 catch 블록 안에서 잡아낼 수 있다.

#### 반복문에서 빠져나오기

forEach 문은 실행하는 도중에 취소할 수 없지만 예외 메커니즘을 이용하면 실행 중인 반복문을 빠져 나올 수 있다.
```js
var a = [0,1,2,3,4,5,6,7,8,9];
try {
    a.foreEach(function(v,i,a) {
        if (i>5) throw false;
        return a[i] = v*v;
    });
}
catch(e) {
    if (e) throw e;
}
```
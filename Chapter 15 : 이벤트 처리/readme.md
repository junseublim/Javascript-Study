# 이벤트 처리

## 이벤트 리스너 등록, 삭제

### addEventListner로 이벤트 리스너 등록하기


```js
target.addEventListner(type, listener, useCapture);
```

- target : 이벤트 리스너를 등록할 DOM 노드
- type : 이벤트 유형을 뜻하는 문자열
- listener : 이벤트가 발생했을 때 처리를 담당하는 콜백 함수의 참조
- useCapture : 이벤트 단계
    - true : 캡처링 단계
    - false : 버블링 단계(기본값)

addEventListner의 장점

1. 같은 요소의 같은 이벤트에 이벤트 리스너를 여러 개 등록할 수 있다.
2. 버블링 단계와 캡처링 단계에서 활용할 수 있다. 반면 DOM 요소 객체에 직접 등록한 이벤트 처리기는 버블링 단계의 이벤트만 캡처 가능
3. removeEventListner, stopPropagation, preventDefault 등을 활용해서 이벤트 전파를 정밀하게 제어할 수 있다.
4. HTML 요소를 포함한 모든 DOM 노드에 이벤트 리스너를 등록할 수 있다.

### removeEventListener
```js
target.removeEventListner(type, listener, useCapture);
```
addEventListner와 인수가 같다.
만약 익명함수로 이벤트를 추가하였다면 삭제할 수 없다. 


## 이벤트 객체

이벤트 처리기와 이벤트 리스너는 이벤트 객체를 인수로 받는다. 이벤트 객체는 해당 이벤트의 다양한 정보를 저장한 프로퍼티와 이벤트의 흐름을 제어하는 메서드를 가지고 있다.

### 이벤트 객체의 프로퍼티

1. type : 이벤트 이름
2. target : 이벤트가 발생한 요소
3. currentTarget : 처리를 담당하는 이벤트 리스너가 등록된 요소
4. eventPhase : 이벤트 전파 단계(1: 캡처링, 2: 타깃, 3: 버블링)
5. timeStamp : 이벤트 발생시각
6. bubbles : 버블링 단계인지 
7. cancelable : preventDefault로 이벤트를 취소할 수 있는지
8. defaultPrevented : preventDefault로 기본 작업이 취소 되었는지
9. isTrusted : 해당 이벤트가 사용자의 액션에 의해 생성되었는지

## 이벤트의 전파

HTML 요소에서 발생하는 이벤트는 그 요소는 물론 그 요소의 모든 조상 요소에 전파된다. 

### 이벤트의 단계

1. 캡처링 단계 : 이밴트가 Window 객체에서 출발해서 DOM 트리를 타고 이벤트 타깃까지 전파된다. 이 단계에서 반응하도록 등록된 이벤트 리스너는 이벤트가 발생한 요소에 등록된 이벤트 처리기나 이벤트 리스너보다 먼저 실행된다. 

2. 타깃 단계 : 이벤트가 타깃에 전파되는 단계. 이벤트 타깃에 등록된 이벤트 처리기나 이벤트 리스너는 이 시점에서 실행된다.

3. 이벤트가 이벤트 타깃에서 출발해서 DOM 트리를 타고 Window 객체까지 전파된다. 이 단계에 반응하도록 등록된 이벤트 리스너는 이벤트가 발생한 요소에 등록된 이벤트 처리기나 이벤트 리스너 다음에 실행된다.

### 이벤트의 전파

#### 이벤트의 전파 취소하기

stopPropagation 메서드는 이벤트가 다음 요소로 전파되는 것을 막는다. 그러나 그 요소 객체의 그 이벤트에 등록한 다른 이벤트 리스너는 변함없이 실행된다.
```js
event.stopPropagation();
```

#### 이벤트 전파의 일시 정지

stopImmediatePropagation 메서드는 그 다음 요소로의 이벤트 전파를 일시적으로 멈춘다. 또한 그 요소 객체의 이벤트에 등록한 다른 이벤트 리스너도 일시적으로 멈춘다.
```js
event.stopImmediatePropagation();
```

#### 기본 동작 취소하기

```js
event.preventDefault();
```

## 이벤트 리스너 안의 this

기본적으로 이벤트 리스너 안의 this는 이벤트가 발생한 요소 객체를 가리킨다. 특정 객체를 가리키게 하는 방법에는 여러 가지가 있다.

### bind 메서드

bind 메서드를 사용하면 함수가 실행될 때의 this를 지정할 수 있다.

### 익명 함수 안에서 실행하는 방법

이벤트 처리기 또는 이벤트 리스너로 익명 함수를 넘기고 익명 함수 안에서 메서드를 호출하면 그 메서드의 this가 메서드를 참조하는 객체를 가리킨다. 

### 화살표 함수를 사용하는 방법

화살표 함수에서 this는 함수를 초기화하는 시점에 결정된다. 객체 안의 메서드를 화살표 함수로 표기하면 그 안의 this가 생성된 객체를 가리킨다.

### addEventListner의 두 번째 인수로 객체를 넘기는 방법

addEventListener 메서드의 두 번째 인수는 함수지만 함수 대신에 객체를 넘길 수 있다. 그리고 그 객체에 handleEvent 메서드가 있으면 그 메서드를 이벤트 리스너로 등록한다.

## 이벤트 리스너에 추가적인 정보를 넘기는 방법

### 익명 함수 안에서 실행하기

익명 함수를 이벤트 리스너로 지정하고 이벤트 리스너 안에서 함수를 실행하면 그 함수에 추가적인 정보를 값으로 넘길 수 있다.
```js
var box = document.getElementById("box");
box.addEventListener("click", function(e)  {
    changeBgColor(e,"red");
}, false);
function changeBgColor(e, color) {
    e.currentTarget.style.backgroundColor = color;
}
```

### 함수를 반환하는 함수를 이벤트 리스너로 등록하기

이벤트 객체를 인수로 받는 함수를 반환하는 함수를 정의해서 그 함수가 반환한 함수를 이벤트 처리기로 등록하는 방법도 있다. 이 방법이 가독성이 더 좋다.

```js
var box = document.getElementById("box");
box.addEventListener("click", changeBgColor("red"), false);
function changeBgColor(color) {
    return function(e) {
        e.currentTarget.style.backgroundColor = color;
    }
}
```
## 이벤트 위임

비슷한 방식으로 여러 요소를 다뤄야 할 때 사용한다. 이벤트 위임을 사용하면 요소마다 핸들러를 할당하지 않고, 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도 여러 요소를 한꺼번에 다룰 수 있다.

공통 조상에서 event.target을 이용하면 어디서 실제로 이벤트가 발생했는지를 알 수 있다.



## Promise 

### Promise의 기본

Promise는 비동기 처리를 실행하고 그 처리가 끝난 후에 다음 처리를 실행하기 위한 용도로 사용한다.

```js
var promise = new Promise(function(resolve, reject) { ... });
```

Promise에는 실행하고자 하는 처리를 작성한 함수를 인수로 넘긴다. 그리고 이 함수는 다음과 같은 인수를 받는다. 
- resolve : 함수 안의 처리가 끝났을 때 호출해야 하는 콜백 함수. resolve 함수에는 어떠한 값도 인수로 넘길 수 있다. 이 값은 다음 처리를 실행하는 함수에 전달된다.
- reject : 함수 안의 처리가 실패했을 때 호출해야 하는 콜백 함수. reject 함수에는 어떠한 값도 인수로 넘길 수 있다. 대부분의 경우 오류 메시지 문자열을 인수로 사용.

```js
var promise = new Promise(function(resolve, reject) {
    setTimeout(function() {
        console.log(A);
        resolve();
    }, 1000);
});
promise.then(function () {
    console.log("B");
});
// 실행결과 : 
// A
// B
```
1초 후에 A가 표시되고 그 다음에 B가 표시된다.

### Promise를 종료시키는 resolve 함수와 then 메서드

resolve 함수는 Promise를 종료시킨다. resolve 함수에 인수로 넘긴 값은 then 메서드에 인수로 넘긴 함수에 전달되어 다음 처리를 위해 사용된다.

### Promise를 실패로 처리하는 reject 함수와 catch 메서드

reject 함수는 Promise를 종료시킨다. resolve 함수와 마찬가지로 reject 함수에도 값을 넘길 수 있다. reject 함수가 실행되면 then 메서드에 넘긴 함수는 실행되지 않는다. 그 대신 catch 메서드에 넘긴 함수가 실행된다. 

### then의 두번째 인수

then의 두번째 인수로 실패 콜백 함수를 지정할 수 있다. 그러면 then 메서드에서 처리할 내용과 catch 메서드에서 처리할 내용을 then 메서드 하나로 작성할 수 있다.

### Promise가 실행하는 콜백 함수에 인수 넘기기

Promise가 실행하는 콜백 함수에 인수를 넘기려면 Promise 객체를 반환하는 함수를 정의하면 된다. 

```js
function buyAsync(mymoney) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            var payment = parseInt(prompt("지불하고자 하는 금액을 입력하세요")
            var balance = mymoney - payment;
            if (balance > 0) {
                console.log(`${payment}원을 지불했습니다.`);
                resolve(balance);
            }
            else{
                reject(`구매할 수 없습니다.`);
            }
        }, 1000);
    });
}

buyAsync(500).then(function(balance) {
    console.log(`잔액은 ${balance}원입니다.`);
})
.catch(function(error) {
    console.log(error);
});
```

### Promise로 비동기 처리 연결하기

Promise로 비동기 처리를 여러개 연결해서 순차적으로 실행하려면 then 메서드 안에서 실행하는 성공 콜백 함수가 Promise 객체를 반환하도록 만든다.

### 비동기 처리 여러 개를 병렬로 실행하기

#### Promise.all

비동기 처리 여러 개를 병렬로 실행할 수 있다. 그리고 모든 처리가 성공적으로 끝났을 때만 다음 작업을 실행하도록 만들 수 있다. all 메서드의 사용법은 다음과 같다. 
```js
Promise.all(iterable);
```
- iterable: Promise 객체가 요소로 들어 있는 반복 가능한 객체이다. 모든 Promise 객체가 resolve 함수를 호출하면 then 메서드에 지정한 함수를 실행한다. 그 때 then에 넘긴 함수는 인수로 response라는 배열을 받는다. 그 배열에서 앞서 모든 Promise 객체가 실행한 resolve 함수의 인수가 담겨 있다. 실패로 끝난 Promise 객체가 하나라도 있다면 가장 먼저 실패로 끝난 Promise 객체에서 실행한 reject 함수의 인수를 실패 콜백 함수에 인수로 넘긴다.

```js
function buyAsync(name, mymoney) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            var payment = parseInt(prompt(`${name}님, 지불하고자 하는 금액을 입력하세요`);
            var balance = mymoney - payment;
            if (balance > 0) {
                console.log(`${payment}원을 지불했습니다.`);
                resolve(balance);
            }
            else{
                reject(`구매할 수 없습니다.`);
            }
        }, 1000);
    });
}
Promise.All([
    buyAsync("Tom", 500),
    buyAsync("Huck", 600),
    buyAsync("Becky", 1000),
])
.then(function(balance) {
    console.log(`잔액은 ${balance}원입니다.`);
})
.catch(function(error) {
    console.log(error);
});
```

#### Promise.race

가장 먼저 종료한 Promise 객체의 결과만 다음 작업으로 보낸다. 나머지 작업은 실행되기는 하지만 결과값이 반환되지는 않는다.

```js
function buyAsync(name, mymoney) {
    return new Promise(function(resolve, reject) {
        setTimeout(function() {
            var payment = parseInt(prompt(`${name}님, 지불하고자 하는 금액을 입력하세요`);
            var balance = mymoney - payment;
            if (balance > 0) {
                console.log(`${payment}원을 지불했습니다.`);
                resolve(balance);
            }
            else{
                reject(`구매할 수 없습니다.`);
            }
        }, 1000);
    });
}
Promise.race([
    buyAsync("Tom", 500),
    buyAsync("Huck", 600),
    buyAsync("Becky", 1000),
])
.then(function(balance) {
    console.log(`잔액은 ${balance}원입니다.`);
})
.catch(function(error) {
    console.log(error);
});
```
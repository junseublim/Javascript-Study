
## Chapter 8 : 함수

### 함수를 정의하는 방법

함수를 정의하는 방법은 네 가지이다.

1. 함수 선언문
```js
function square(x) {return x*x}
```

2. 함수 리터럴로 정의하는 방법
```js
var square = function (x) {return x*x}
```

3. Function 생성자로 정의하는 방법
```js
var square = new Function("x", "return x*x");
```


3. 화살표 함수 표현식으로 정의하는 방법 (ECMAScript 6)
```js
var square = x => x*x;
```

함수 선언문(1번)으로 정의된 함수는 호이스팅 된다. 다른 방법들은 호이스팅되지 않는다.

### 중첩 함수

중첩 함수란 함수 내부에 선언된 함수를 의미한다. 

- 외부 함수의 최상위 레벨에만 중첩 함수를 작성할 수 있다. if 문, while 문 등의 문장 블록안에는 중첩 함수를 작성할 수 없다.

- 중첩 함수의 참조는 그 중첩 함수를 들러싼 외부 함수의 지역 변수에 저장되므로 외부 함수의 바깥에서는 읽거나 쓸 수 없다. 

- 또한 중첩 함수는 자신을 둘러싼 외부 함수의 인수와 지역 변수에 접근 할 수 있다. 이는 클로저의 핵심적인 구성 요소가 된다.

### 함수 호출하기

함수를 호출하는 방법은 4가지이다.

1. 함수의 참조가 저장된 변수 뒤에 그룹 연산자인 ()를 붙여서 호출

2. 메서드 호출: 객체의 프로퍼티에 저장된 값이 함수 타입일 경우 ()를 붙여서 호출.

3. 생성자 호출: 함수 또는 메서드를 호출할 때 함수의 참조를 저장한 변수 앞에 new 키워드를 추가하면 함수가 생성자로 동작한다.

4. call, apply를 사용한 간접 호출

### 즉시 실행 함수

즉시 실행 함수를 정의하는 방법은 두가지가 있다.
```js
(function() {...})();
```
```js
(function() {...}());
```
즉시 실행 함수는 전역 유효 범위를 오염시키지 않는 이름 공간을 생성할 때 사용한다.


### 함수의 인수

함수에서 정의식에 작성된 인자 개수보다 인수가 적게 전달되면 생략된 인수는 undefined가 된다. 논리합 연산자를 이용해서 생략시에 사용할 초깃값을 설정할 수 있다.

```js
function multiply(a, b) {
    b = b || 1; //b가 undefined일 경우, 1을 반환
    return a*b;
}
```

#### 가변 길이 인수 목록(Arguments 객체)

함수에서는 지역 변수로 arguments 변수가 존재한다. arguments는 Arguments 객체이다. 함수에 인수를 넘기면 순서대로 arguments에 유사 배열 객체로 저장된다.

```js
    arguments[0] // 첫 번째 인수값
    arguments[1] // 두 번째 인수값
```

Arguments 객체는 프로퍼티로 length와 callee를 갖고 있다. 
- arguments.length : 인수 개수
- arguments.callee : 현재 실행되고 있는 함수의 참조

arguments.callee를 사용하면 이름이 없는 익명함수도 재귀호출을 할 수 있다.

### 프로그램의 평가와 실행 과정

자바스크립트 엔진은 실행 가능한 코드를 만나면 그 코드를 평가해서 실행 문맥으로 만든다. 이 실행 가능한 코드의 유형은 3가지이다. 

- 전역 코드 : 전역 객체 Window 아래에 정의된 함수
- 함수 코드 : 함수를 의미
- eval 코드 : eval 함수

실행 문맥을 초기화하는 과정이 다르기 때문에 코드의 유형을 분류한다. 

실행 문맥은 실행 가능한 코드가 실제로 실행되고 관리되는 영역으로 실행에 필요한 모든 정보를 컴포넌트 여러 개가 나누어 관리하도록 만들어져 있다.

그중에서 가장 중요한 컴포넌트는 렉시컬 환경(Lexical Environment) 컴포넌트, 변수 환경 컴포넌트, 디스 바인딩(This Binding) 컴포넌트이다. 

렉시컬 환경 컴포넌트와 변수 환경 컴포넌트는 거의 독같다. 디스 바인딩 컴포넌트는 그 함수를 호출한 객체의 참조가 저장되는 곳이다. 이것이 가리키는 값이 곧 해당 실행 문맥의 this가 된다.

#### 렉시컬 환경 컴포넌트의 구성

실행 문맥의 구성 요소인 렉시컬 컴포넌트는 자바스크립트 엔진이 자바 스크립트 코드를 실행하기 위해 자원을 모아 둔 곳으로 구체적으로 함수 또는 블록의 유효 범위안에 있는 식별자와 그 결과값이 저장되는 곳이다. 자바스크립트 엔진은 해당 자바스크립트 코드의 유효 범위 안에 있는 식별자와 그 식별자가 가리키는 값을 키와 값의 쌍으로 바인드해서 렉시컬 환경 컴포넌트에 기록한다. 렉시컬 환경 컴포넌트는 환경 레코드와 외부 렉시컬 환경 참조 컴포넌트로 구성되어 있다. 

환경 레코드는 유효 범위 안에 포함된 식별자를 기록하고 실행하는 영역이다. 

외부 렉시컬 환경 참조에는 함수를 둘러싸고 있는 코드가 속한 렉시컬 환경 컴포넌트의 참조가 저장된다. 중첩된 함수 안에서 바깥 코드에 정의된 변수를 읽거나 써야 할 떄, 자바스크립트 엔진은 외부 렉시컬 환경 참조를 따라 한 단계씩 렉시컬 환경을 거슬러 올라가서 그 변수를 검색한다.

환경 레코드는 렉시컬 환경 안의 식별자와 그 식별자가 가리키는 값의 묶음이 실제로 저장되는 영역이다. 이 환경 레코드는 선언적 환경 레코드와 객체 환경 레코드로 구성되어 있으며 저장하는 값의 유형에 따라 쓰임새가 달라진다.

선언적 환경 레코드는 실제로 함수와 변수, catch 문의 식별자와 실행 결과가 저장되는 영역이다.

객체 환경 레코드는 실행 문맥 외부에 별도로 저장된 객체의 참조에서 데이터를 읽거나 쓴다.

#### 전역 환경과 전역 객체의 생성

자바스크립트 인터프리터는 시작하자마자 렉시컬 환경 타입의 전역 환경을 생성한다. 웹 브라우저에 내장된 자바스크립트 인터프리터는 새로운 웹 페이지를 읽어 들인 후에 전역 환경을 생성한다. 그리고 전역 객체를 생성한 다음 전역 환경의 객체 환경 레코드에 전역 객체의 참조를 대입한다.

### 클로저

클로저는 자바스크립트가 가진 기능으로, 이를 활용하면 변수를 은닉하여 지속성을 보장하는 등의 다양한 기능들을 구현할 수 있다. 

### 이름 공간

전연 유효 범위의 오염을 방지하기 위한 수단으로서 객체와 함수를 이름 공간으로 이용한다.

저역 변수와 전역 함수를 전역 객체에 선언하는 행위는 전역 유효 범위를 오염시킨다고 볼 수 있다. 전역 유효 범위가 오염되면 다음과 같은 상황에서 변수 이름과 함수 이름이 겹칠 수 있다.

- 라이브러리 파일을 여러 개 읽어 들여 사용할 때
- 규모가 큰 프로그램을 만들 떄
- 여러 사람이 한 프로그램을 만들 떄

전역 유효 범위 안에서 이름이 같은 변수나 함수를 선언하면 자바스크립트 엔진이 그 프로그램의 첫머리로 끌어올려서 변수 또는 함수를 단하나만 생성한다. 그러면 다른 목적으로 사용하는 코드가 같은 변수와 함수를 공유하게 되므로 올바르게 동작하지 않을 수 있다. 이러한 오류를 피하기 위해 오염을 최소화 하여야 한다. 

#### 객체를 이름 공간으로 활용하기

이름 공간이란 변수 이름과 함수 이름을 한곳에 모아 두어 이름 충돌을 미리 방지하고, 변수와 함수를 쉽게 가져다 쓸 수 있게 만든 메커니즘이다. 객체를 이름 공간으로 활용하려면 객체를 값으로 가지는 전역 변수를 하나 생성하고, 그 객체에 프로그램 전체에서 사용하는 모든 변수와 함수를 프로퍼티로 정의한다. 예를 들어 다음과 같은 방법으로 myApp이라는 전역 변수를 이름 공간으로 활용 할 수 있다.
```js
var myApp = myApp || {};
```

이렇게 작성해 두면 myApp이 이미 정의되어 있을 때는 그것을 사용하고 그렇지 않으면 빈 객체를 myApp에 할당한다. 이 객체에 전역 유효 범위에서 사용하고자 하는 모든 변수와 함수를 프로퍼티로 추가하면 된다.
```js
myApp.name = "Tom";
myApp.showName = function() {...};
```

이렇게 되면 myApp만이 사용자가 정의한 전역 변수가 되므로 전역 유효 범위의 오염을 최소화할 수 있다.
또한 myApp 객체 내에 새로운 객체를 추가함으로써 부분 이름 공간을 만들 수도 있다.

#### 함수를 이름 공간으로 활용하기

함수 안에서 선언된 변수의 유효 범위는 함수 내부이다. 따라서 그 변수를 함수 안에서는 읽거나 쓸 수 있지만 바깥에서는 읽거나 쓸 수 없다. 이 성질을 활용하면 함수를 이름 공간으로 활용할 수 있다.
```js
(function() {
    var x = "local x";
})();
console.log(x); // Uncaught ReferenceError
```
즉시 실행 함수 내부에서 선언한 변수인 x는 이 함수의 지역 변수이므로 전역 변수와 이름이 충돌하지 않는다. 따라서 일시적인 처리를 수행하고자 할 때 그 내용물을 즉시 실행 함수 안에 작성하면 전역 유효 공간을 오염시키지 않고 실행 할 수 있다.


### 객체로서의 함수

자바스크립트에서 함수는 일종의 객체이다. 따라서 함수는 값을 처리할 수 있으며 프로퍼티와 메서드를 가진다.

#### 함수의 프로퍼티

- caller : 현재 실행 중인 함수를 호출한 함수
- length : 함수의 인자 개수
- name : 함수를 표시할 때 사용하는 이름
- prototype : 포로토타입 객체의 참조

또한 함수는 Function 생성장의 prototype 객체의 프로퍼티를 상속받아서 사용한다.

- apply() : 선택한 this와 인수를 사용하여 함수를 호출한다. 인수는 배열 객체이다.
- bind() : 선택한 this와 인수를 적용한 새로운 함수를 반환한다.
- call() : 선택한 this와 인수를 사용하여 함수를 호출한다. 함수는 쉼표로 구분한 값이다.
- constructor() : Function 생성자의 참조
- toString() : 함수의 소스 코드를 문자열로 반환.

#### apply와 call 메서드

Function 객체의 메서드에는 apply와 call이 있다. this 값과 함수의 인수를 사용하여 함수를 실행하는 메서드이다. 둘은 본질적으로 같으며 함수에 인수를 넘기는 방법이 다르다.
```js
function say(greetings, honorifics) {
    console.log(greetings + " " + honorifics + this.name);
}

var tom = {name: "Tom"};
var becky = {name: "Becky"};

say.apply(tom, ["hello", "Mr."]); //hello Mr.Tom
say.call(becky, "hi", "Ms."); //hi Ms.Becky
```

#### bind 메서드

Function 객체의 bind 메서드는 객체에 함수를 바인드한다.
```js
function say(greetings, honorifics) {
    console.log(greetings + " " + honorifics + this.name);
}

var tom = {name: "Tom"};
var sayToTom = say.bind(tom);
sayToTom("Hello", "Mr."); // Hello Mr.Tom
```

이 코드에서 sayToTom 함수를 호출하면 항상 this가 객체 tom을 가리킨다. 

#### 함수에 프로퍼티 추가하기

다른 객체와 마찬가지로 함수에도 프로퍼티를 추가할 수 있다. 이 프로퍼티는 함수를 실행하지 않아도 읽거나 쓸 수 있다. 이를 응용하여 메모이제이션을 사용할 수 있다.
```js
function fibonacci(n) {
    if (n<2) return n;
    if (!(n in fibonacci)) {
        fibonacci[n] = fibonacci(n-1) + fibonacci(n-2);
    }
    return fibonacci[n];
}
for (var i =0; i<=20; i++) {
    console.log((" "+i).slice(-2) + ":"+fibonacci(i));
}
```

### 콜백 함수

콜백 함수란 다른 함수에 인수로 넘겨지는 함수를 의미한다. 콜백 함수는 함수를 호출할 때 무언가 새로운 일이 생기거나 그 함수의 실행이 끝나면 지정한 콜백 함수를 실행해 주도록 함수에 요청해야 할 때 사용한다. 이 때 콜백 함수의 주체는 어디까지나 함수를 호출한 호출자이다. 호출자가 목적에 따라 어떠한 콜백 함수를 사용할 것인지 정한다. 호출된 함수는 콜백 함수를 실행하지만 그 콜백 함수가 작업하는 내용에는 관여하지 않는다.

### ECMAScript 6부터 추가된 함수의 기능

#### 화살표 함수

화살표 함수는 다음과 같은 차이점이 있다.

1. this의 값이 함수를 정의할 떄 결정된다 : 함수 리터럴로 정의한 함수의 this 값을 함수를 호출할 때 결정된다. 그러나 화살표 함수의 this 값은 함수를 정의할 때 결정된다. 즉, 화살표 함수 바깥의 this값이 화살표 함수의 this 값이 된다.

```js
var obj = {
    say: function() {
        console.log(this); // [object Object]
        var f = function() {console.log(this);} // [object Window]
        f();
        var g = () => console.log(this); //[object Object]
        g();
    }
}
obj.say();
```

f는 say라는 함수의 중첩 함수이며 this의 값은 전역 객체를 가르킨다. 한편 화살표 함수 g의 this 값은 함수 g를 정의한 익명 함수의 this값인 객체 obj를 가리킨다.

화살표 함수는 call이나 apply 메서드를 사용하여 this를 바꾸어 호출해도 this 값이 바뀌지 않는다.
```js
var f = function() {console.log(this.name);};
var g = () => console.log(this.name);
var tom = {name: "Tom"};
f.call(tom); // "Tom"
g.call(tom); // ""
```

2. arguments 변수가 없다. 화살표 함수 안에는 arguments 변수가 정의되어 있지 않다.

3. 생성자로 사용할 수 없다.

4. yield 키워드를 사용할 수 없다.

#### 인수에 추가된 기능

1. 나머지 매개변수 : 인자가 들어가는 부분에 ...를 입력하면 그만큼의 인수를 배열로 받을 수 있다.

2. 인수의 기본 값 인자에 대입 연산자를 사용해서 기본값을 설정할 수 있다.


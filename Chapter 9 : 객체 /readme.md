
## Chapter 9 : 객체

### 프로토타입

생성자안에서 메서드를 정의하면 그 생성자로 생성한 모든 인스턴스에게 똑같은 메서드가 추가된다. 따라서 여러 인스턴스를 생성하면 그 개수만큼 메서드를 생성하여 메모리를 낭비하게 된다.
이를 해결하기 위해 프로토타입 객체에 메서드를 정의한다. 

#### 프로토타입 객체

자바스크립트에서는 함수도 객체이므로 함수 객체가 기본적으로 prototype 프로퍼티를 갖고 있다.

함수의 prototype 프로퍼티가 가리키는 객체를 그 함수의 프로토타입 객체라고 한다. prototype 포로퍼티는 기본적으로 빈 객체를 가리킨다.
프로토타입 객체의 프로퍼티는 생성자로 생성한 모든 인스턴스에세 그 인스턴스의 프로퍼티처럼 사용할 수 있다.

Prototype 객체의 프로퍼티는 읽기만 가능하고 수정이 불가능하다.

#### 프로토타입 상속

상속이란 일반적으로 특정 객체가 다른 객체로부터 기능을 이어받는 것을 말한다. 자바스크립트에서는 클래스가 아닌 객체를 상속한다. 자바스크립트의 상속은 프로토타입 체인이라고 부르는 객체의 자료 구조로 구현되어 있으며, 프로토타입 상속이라고 부른다. 

#### 프로토타입 체인

모든 객체는 내부 프로퍼티 [[Prototype]]을 가진다. 이것은 함수 객체의 prototype 프로퍼티와는 다른 객체이다. ECMAScript 5까지는 사용자가 이 내부 프로퍼티 [[Prototype]]을 읽거나 쓸 수 없었지만, ECMAScript 6부터는 \_\_proto\_\_ 프로퍼티에 [[Prototype]]의 값이 저장된다.

```js
var obj = {};
console.log(obj.__proto__); // Object {}
```

객체의 \_\_proto\_\_ 프로퍼티는 그 객체에게 상속을 해준 부모 객체를 가리킨다. 따라서 객체는 \_\_proto\_\_ 프로퍼티가 가리키는 부모 객체의 프로퍼티를 사용할 수 있다.
```js
var objA = {
    name: "Tom",
    sayHello: function() {console.log("Hello!" + this.name);}
};
var objB = {
    name: "Huck"
};

objB.__proto__ = objA;
var objC = {};
objC.__proto = objB;
objC.sayHello(); // Hello! Huck
```

1. objC.sayHello()가 호출되면 먼저 objC 자신이 sayHello라는 프로퍼티가 있는지 확인
2. 다음으로 objC의 \_\_proto\_\_가 가리키는 objB가 sayHello라는 프로퍼티 소유하고 있는지 확인
3. 다음으로 objB의 \_\_proto\_\_가 가리키는 objA가 sayHello라는 프로퍼티 소유하고 있는지 확인
4. objC가 name이라는 프로퍼티가 있는지 확인
2. 다음으로 objC의 \_\_proto\_\_가 가리키는 objB가 name이라는 프로퍼티 소유하고 있는지 확인

이처럼 자신이 갖고 있지 않은 프로퍼티를 \_\_proto\_\_ 프로퍼티가 가리키는 객체를 차례대로 거슬러 올라가며 검색한다. 이를 프로토타입 체인이라고 한다. 

여기에서 객체의 \_\_proto\_\_ 프로퍼티가 가리키는 객체가 바로 상속을 해 준 객체이며, 이 객체를 그 객체의 프로토타입이라고 한다. 객체는 자신이 가지고 있지 않은 특성을 프로토타입 객체에 위임한다고 할 수 있다.

이처럼 자바스크립트는 프로토타입 체인을 사용하여 객체의 프로퍼티를 다른 객체로 전파한다. 

프로토타입은 obj.\_\_proto\_\_ 혹은 Object.getPrototypeOf 메서드로 가져올 수 있다. ECMAScript6부터는 객체의 프로토타입을 설정하는 메서드인 Object.setPrototypeOf도 추가되었다. 

#### new 연산자의 역할
```js 
function Circle(center, radius) {
    this.center = center;
    this.radius = radius;
}
Circle.prototype.area = function() {
    return Math.PI * this.radius * this.radius;
}

var c = new Circle({x:0,,y:0}, 2.0);
```
new 연산자를 호출해서 인스턴스를 생성하면 내부적으로 다음과 같은 작업을 수행한다.

1. 빈 객체를 생성한다.
2. Circle.prototype을 생성된 객체의 프로토타입으로 설정한다. 이때 Circle.prototype이 가리키는 값이 객체가 아니라면 Object.prototype을 프로토타입으로 설정한다.
3. Circle 생성자를 실행하고 c를 초기화한다. 이때 this는 1 에서 생성한 객체로 설정한다.

#### 프로토타입 객체의 프로퍼티

함수를 정의하면 함수 객체는 기본적으로 prototype 프로퍼티를 갖게 된다. 그리고 이 prototype 프로퍼티는 프로토타입 객체를 가리키며, 이 프로토타입 객체는 기본적으로 constructor 프로퍼티와 내부 프로퍼티[[Prototype]]\(\_\_proto\_\_)을 가지고 있다.

##### constructor 프로퍼티

constructor 프로퍼티는 함수 객체의 참조를 값으로 가지고 있다.
```js
function F() {};
console.log(F.prototype.constructor); //Function F() {};
```
생성자와 생성자의 프로토타입 객체는 서로를 참조한다. 생성자의 prototype 프로퍼티가 프로토타입 객체를 가리키며, 이 프로토타입 객체의 constructor 프로퍼티가 생성자를 가리킨다. 반면 생성자로 생성한 인스턴스는 생성될 때의 프로토타입 객체의 참조만 가지고 있을 뿐 생성자와는 직접적인 연결고리가 없다.

##### 내부 프로퍼티 [[Prototype]]

함수 객체가 가진 프로토타입 객체의 내부 프로퍼티 [[Prototype]]는 기본적으로 Object.prototype을 가리킨다. 즉, 프로토타입 객체의 프로토타입은 Object.prototype이다.
```js
function F() {};
console.log(F.prototype.__proto__); // Object {}: Object prototype
```

이 덕분에 생성자로 생성한 인스턴스가 Object.prototype의 프로퍼티를 사용할 수 있다. 또한 Object.prototype의 프로토타입은 null을 가리킨다. 

##### 프로토타입 객체 교체 및 constructor 프로퍼티

생성자가 가진 prototype 프로퍼티 값을 새로운 객체로 교체할 때는 주의해야 한다. 프로퍼티만 정의되어 있는 새로운 객체를 prototype 프로퍼티 값으로 대입하면 인스턴스와 생성자 사이의 연결 고리가 끊겨 버린다. 따라서 새로운 prototype 프로퍼티 값에 constructor 프로퍼티를 정의해야 한다. 
```js 
function Circle(center, radius) {
    this.center = center;
    this.radius = radius;
}
Circle.prototype = {
    constructor : Circle,
    area: function() {return Math.PI*this.radius*this.radius;}
};
var c = new Circle({x:0,y:0}, 2.0);
console.log(c.consturctor) // Function Circle
console.log(c instanceof Circle) // true
```

##### 프로토타입의 확인

특정 프로토타입 객체가 그 객체의 프로토타입 체인에 포함되어 있는지 확인하는 방법에는 instanceof 연산자를 사용하는 방법과 isPrototypeOf 메서드를 사용하는 방법이 있다.

instanceof 연산자는 지정한 객체의 프로토타입 체인에 지정한 생성자의 프로토타입 객체가 포함 되어있는지 판정한다. instanceof 연산자는 논리값을 반환하는 이항 연산자이다.

isPrototypeOf 메서드는 특정 객체가 다른 객체의 프로토타입 체인에 포함되어 있는지 판정한다. 


### 접근자 프로퍼티
접근자 프로퍼티를 사용하면 프로퍼티를 읽고 쓸 때 윈하는 작업을 자동으로 처리할 수 있다.

#### 프로퍼티의 종류

객체의 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 나눌 수 있다.
- 데이터 프로퍼티 : 값을 저장하기 위한 프로퍼티
- 접근자 프로퍼티 : 값이 없음. 프로퍼티를 읽거나 쓸 때 호출하는 함수를 값 대신에 지정할 수 있는 프로퍼티

#### 접근자 프로퍼티

접근자란 객체 지향 프로그래밍에서 객체가 가진 프로퍼티 값을 객체 바깥에서 읽거나 쓸 수 있도록 제공하는 메서드를 말한다. 객체의 프로퍼티를 객체 바깥에서 직접 조작하는 행위는 데이터의 유지 보수성을 해치는 주요한 원인이다. 접근자 프로퍼티를 사용하면 데이터를 부적절하게 변경하는 것을 막고 특정 데이터를 외부로부터 숨길 수 있으며 외부에서 데이터를 읽으려고 시도할 때 적절한 값으로 가공해서 넘길 수 있다. 자바스크립트의 접근자 프로퍼티는 객체에 접근자를 정의 할 수 있게 한다. 접근자 프로퍼티를 사용하여 프로퍼티를 읽고 쓸 수 있게 하면 프로그램의 유지 보수성을 높일 수 있다. 

접근자 프로퍼티 하나에 대해 그 프로퍼티를 읽을 때의 처리를 담당하는 게터 함수와 쓸 때의 처리를 담당하는 세터 함수를 정의할 수 있다.
```js
var person = {
    _name: "Tom",
    get name() {
        return this._name;
    },
    set name(value) {
        var str = value.charAt(0).toUpperCase() + value.substring(1);
        this._name = str;
    }
};

console.log(person.name); //Tom
person.name = "huck";
console.log(person.name); //Huck
```

이 예제에서 name이라는 이름의 접근자 프로퍼티를 정의한다. 접근자 프로퍼티 name은 데이터 프로퍼티 _name의 값을 읽고 쓰는 일을 담당한다. 값을 쓸 때는 문자열의 첫 글자를 대문자로 바꾼 후에 _name 프로퍼티에 대입한다.

접근자 프로퍼티는 delete 연산자로 삭제할 수 있다.

객체를 생성한 후에 접근자 프로퍼티를 추가하거나 생성자안에 접근자 프로퍼티를 정의하려면 Object.defineProperty 나 Object.defineProperties 메서드를 사용한다. 

#### 데이터 캡슐화

즉시 실행 함수롤 클로저를 생성하면 데이터를 외부에서 읽고 쓸 수 없도록 숨기고 접근자 프로퍼티만 읽고 쓰도록 만들 수 있다.
```js
var person = (function() {
    var _name: "Tom";
    return {
        get name() {
            return this._name;
        },
        set name(value) {
            var str = value.charAt(0).toUpperCase() + value.substring(1);
            this._name = str;
        }
    }
})();
```

### JSON

JSON은 자바스크립트 객체를 문자열로 표현하는 데이터 포맷이다. JSON의 포맷은 자바스크립트 리터럴 표기법에 기반을 두고 있다. JSON 데이터는 전체를 작은 따옴표로 묶은 문자열이다. 이때 객체의 프로퍼티 이름은 큰 따옴표로 묶은 문자열로 표기한다. 숫자, 논리값, 배열은 자바스크립트오 ㅏ같은 표기법을 사용하지만 문자열은 반드시 큰따옴표로 묶어야 한다.

#### 자바스크립틑 객체를 JSON 문자열로 변환하기 : JSON.stringify

JSON.stringify 메서드는 인수로 받은 객체를 JSON 문자열로 바꾸어 반환한다. JSON.stringify 메서드의 사용법은 다음과 같다.
```
JSON.stringify(value[, replacer[, space]])
```

- value에는 JSON으로 변환할 객체를 지정한다. 
- replacer에는 함수 또는 배열을 지정한다. 함수를 지정하면 문자열로 만드는 프로퍼티의 키와 값을 함수의 인수로 받아서 프로퍼티 값을 표현하는 문자열을 반환한다. 배열을 지정하면 배열의 요소로 객체의 프로퍼티 이름을 필터링한다.
- spacer에는 출력하는 문자열을 구분할 때 사용할 공백 문자를 지정한다.

JSON.stringify 주의사항
- NaN, Infinity, -Infinity 는 null로 직렬화된다.
- Date 객체는 ISO 포맷의 날짜 문자열로 직렬화된다.
- Function, RegExp, Error 객체, undefined, 심벌은 직렬화할 수 없다.
- 객체 자신이 가지고 있는 열거 가능한 프로퍼티만 직렬화된다.
- 직렬화할 수 없는 프로퍼티는 문자열로 출력되지 않는다.
- 프로퍼티 중에서 키가 심벌인 프로퍼티는 직렬화되지 않는다.

#### JSON 문자열을 자바스크립트 객체로 환원하기 : JSON.parse

JSON.parse 메서드는 인수로 받은 문자열을 자바스크립트 객체로 환원해서 반환한다.
```
JSON.parse(text [, reviver])
```
- text에는 JSON 문자열을 지정한다.
- reviver에는 프로퍼티의 키와 값을 인수로 받는 함수를 지정할 수 있다. 이 함수는 환원될 객체의 프로퍼티 값을 반환해야만 한다.


### ECMAScrip6부터 추가된 객체의 기능

#### 프로퍼티 이름으로 심벌 사용하기

프로퍼티 이름으로 심벌을 사용할 수 있다. 심벌은 유일무이한 값으로 함수 바깥에서는 그 프로퍼티 값을 읽거나 쓸 수 없다. 그러나 Object.getOwnPropertySymbols 메서드를 사용하면 객체안에서 이름을 심벌로 지정한 프로퍼티 이름 목록을 가져올 수 있다.

##### 내장 생성자 prototype의 안전한 확장

일반적으로 Array와 Date 등 기본 생성자의 prototype에 메서드를 추가해서 확장하는 방법은 권자오디지 않는다. 심벌을 사용하면 메서드 이름이 겹치는 것을 피하면서도 기본 생성자의 prototype을 확장할 수 있다.


#### 객체 리터럴에 추가된 기능

1. 계산된 프로퍼티 이름 : 대괄호로 묶인 임의 계산식이 평가된 값을 프로퍼티 이름으로 사용할 수 있게 되었다.
```js
var prop = "name", i=1;
var obj = {[prop+i]: "Tom"};
console.log(obj) // Object {name1: "Tom"}
```

2. 프로퍼티 정의의 약식 표기 {prop} : 변수 prop이 선언되어 있을 때 {prop}을 {prop: prop}으로 사용 할 수 있게 되었다. 즉, 프로퍼티 이름이 변수 이름과 같을 때 {prop}으로 줄여서 표현할 수 있게 되었다.
```js
var prop = 2;
var obj = {prop};
console.log(obj) // Object { prop : 2}
```

3. 메서드 정의 약식 표기 {method() {}} : 프로퍼티 값으로 함수를 지정할 때 사용하는 약식 표기법이 추가되었다. 
```js
var person = {
    name: "Tom",
    sayHello () {
        console.log("Hello!");
    }
}
```

4. 제너레이터 정의 약식 표기 { *generator() {} } : 프로퍼티의 값이 제너레이터 함수일 때 사용할 수 있는 약식 표기법
```js
var obj = {
    *range(n) {for (var i =0; i<n; i++) yield i;}
};
```






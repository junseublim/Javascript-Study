
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



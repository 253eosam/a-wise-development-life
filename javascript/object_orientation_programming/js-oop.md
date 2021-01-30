# 객체지향

> 객체지향 프로그래밍 ( Object-Oriented Programming )

## 객체

- 상태 (state, data)
- 행위 (behave, process)

### 추상화 ( Abstraction )

- 공통의 속성이나 기능을 묶어 이름을 붙이는 것
- 객체 지향적 관점에서 클래스를 정의하는 것을 추상화

### 캡슐화 ( Encapuslation )

- 실제로 구현되는 부분을 외부에 드러나지 않도록 캡슐로 감싸 이용가능한 방법만알려준는 것
- 데이터 구조와 데이터를 다루는 방법들을 결합 시켜 묶는 것.

### 은닉화 ( Hiding )

- 내부 데이터, 내부 연산을 외부에서 접근하지 못하도록 은닉 혹은 격리 시키는 것
- 접근지정자 : `private` , 접근 & 제어 : `setter , getter`

### 상속성, 재사용 ( Inheritance ) ✅

- 상위 개념의 특징을 하위 개념이 물려받는 것
- 장점
    - 재사용으로 인해 코드가 줄어듬.
    - 자식 클래스는 부모 클래스의 모든 자료와 메소드를 물려받아 자유롭게 사용할 수 있지만, 자신의 자료와 메소드를 추가적으로 생성하여 새로운 형태로 발전 가능.

### 다형성 ( Polymorphism )

- 부모 클래스에서 물려받은 가상 함수를 자식 클래스 내에서 **오버라이딩** 되어 사용하는 것

# 자바스크립트로 객체 만들기

## 객체

> Object

- 객체란 서로 연관된 변수와 함수를 그룹핑한 그릇
- property & method

    ```jsx
    var person = {};
    person.name = 'donald';
    person.introduce = function(){
    	return 'My name is ' + this.name;
    }
    console.log(person.introduce());
    // My name is donald
    ```

    ```jsx
    var person = {
    	'name' : 'donald',
    	'introduce' : function(){
    		return 'My name is ' + this.name;
    	}
    }
    console.log(person.introduce());
    ```

## 생성자

> Constructor

- 객체를 만드는 역할
- 객체를 초기화하는 역할

    ```jsx
    function Person(){}
    var p1 = new Person();
    p1.name = 'doanld';
    p1.introduce = function(){
    	return 'My name is' + this.name;
    }
    console.log(p1.introduce());

    var p2 = new Person();
    p2.name = 'dao';
    p2.introduce = function(){
    	return 'My name is ' + this.name;
    }
    console.log(p2.introduce());
    ```

    ```jsx
    // 개선된 생성자를 이용한 초기화 방법
    function Person(name){
    	this.name = name;
    	this.introduce = function(){
    		return 'My name is ' + this.name;
    	}
    }

    var p1 = new Person('donald');
    console.log(p1.introduce());

    var p2 = new Person('dao');
    console.log(p2.introduce());
    ```

### 자바스크립트 생성자의 특징

- 자바스크립트에서 객체를 만드는 주체는 함수.
- 함수에 `new`를 붙이는 것을 통해서 객체를 만들 수 있다는 점은 자바스크립트에서 함수의 위상을 암시.

# 전역객체

> Global object

- 모든 객체는 이 전역객체의 프로퍼티

    ```jsx
    function func(){
    	console.log('Hello?');
    }
    func();
    window.func();
    ```

    → func(); 와 window.func();는 모두 실행 

    - 모든 전역변수와 함수는 `window` 객체의 프로퍼티
    - 객체를 명시하지 않으면 암시적으로 window의 프로퍼티로 간주

    ```jsx
    var o = {'func' : function(){
    	console.log('Hello?');
    }}
    o.func(); // Hello?
    window.o.func(); // Hello?
    ```

    → 자바스크립에서 모든 객체는 기본적으로 전역객체의 프로퍼티

    **웹브라우저**에서는 전역객체 `window`
    **node.js** 에서는 `global`

# this

- `this` 는 함수의 호출 맥락(context)를 의미.
- 함수 호출을 어떻게 하느냐에 따라서 this가 가리키는 대상이 달라짐.

## 함수호출

- 함수를 호출했을 때 this는 전역객체인 window와 같다.

    ```jsx
    function func(){
    	if( window === this ){
    		console.log('window === this');
    	}
    }
    func();
    // window === this
    ```

## 메소드의 호출

- 객체의 소속인 메소드의 this는 그 객체를 가르킴

    ```jsx
    var o = {
    	func : function(){
    		if( o === this){
    			console.log('o === this');
    		}
    	}
    }
    o.func();
    // o === this
    ```

## 생성자의 호출

- 함수를 호출 했을때, new 를 이용하여 호출했을 때의 차이

    ```jsx
    var funcThis = null;

    function Func(){
    	funcThis = this;
    }
    var o1 = Func();
    if(funcThis === window){
    	console.log('window ');
    }

    var o2 = new Func();
    if(funcThis === o2){
    	console.log('o2 ');
    }
    // window
    // o2
    ```

    → 생성자는 빈 객체를 만듬.

    → 객체내에서 this는 만들어진 객체를 가르킴.

    생성자가 실행되기 전까지는 객체는 변수에도 할당될 수 없기 때문에 this가 아니면 객체에 대한 어떠한 작업을 할 수 없기 때문.

    ```jsx
    function Func(){
    	console.log(o);
    }
    var o = new Func();
    // undefined
    ```

    ['new' 연산자와 생성자 함수](https://www.notion.so/new-0f6135f2ccf4495b9ef40bd6bcf3384f)

## apply, call

- 함수의 메소드인 `apply`, `call`을 이용하면 `this`의 값을 제어할 수 있다.

    ```jsx
    var o = {}
    var p = {}
    function func(){
    	switch(this){
    		case o:
    			console.log('o');
    			break;
    		case p:
    			console.log('p');
    			break;
    		case window:
    			console.log('window');
    			break;
    	}
    }
    func();
    func.apply(o);
    func.apply(p);
    // window
    // o
    // p
    ```

# 상속

- 상속은 객체의 로직을 그대로 물려 받는 또 다른 객체를 만들 수 있는 기능을 의미

    > Code 로 이해하기..

    ```jsx
    function Person(name){
    	this.name = name;
    	this.introduce = function(){
    		return 'My name is ' + this.name;
    	}
    }
    var p1 = new Person('donald');
    console.log(p1.introduce());	
    // My name is donald
    ```

    ```jsx
    function Person(name){
    	this.name = name;
    }
    Person.prototype.name = null;
    Person.prototype.introduce = function(){
    	return 'My name is ' + this.name;
    }
    var p1 = new Person('donald');
    console.log(p1.introduce());
    // My name is donald
    ```

    ```jsx
    function Person(name){
    	this.name = name;
    }
    Person.prototype.name = null;
    Person.prototype.introduce = function(){
    	return 'My name is' + this.name;
    }

    function Programmer(name){
    	this.name = name;
    }
    Programmer.prototype = new Person();

    var p1 = new Programmer('donald');
    console.log(p1.introduce());
    ```

    - Programmer 생성자 → prototype과 Person의 객체를 연결 → Programmer  객체도 메소드 introduce를 사용가능

    ```jsx
    function Person(name){
    	this.name = name;
    }
    Person.prototype.name = null;
    Person.prototype.introduce = function(){
    	return 'My name is ' + this.name;
    }

    function Programmer(name){
    	this.name = name;
    }
    Programmer.prototype = new Person();
    Programmer.prototype.coding = function(){
    	return "hello world!";
    }

    var p1 = new Programmer("donald");
    console.log(p1.introduce());
    console.log(p1.coding());
    // My name is donald
    // hello world
    ```

    → Programmer는 Person의 기능을 가지고 있으면서 Person이 가지고 있지 않은 기능인 메소드 coding을 가지고 있음.

# Prototype

> 객체의 원형

- 객체는 프로퍼티를 가질 수 있음. `prototype`은 프로퍼티 중에서도 용도가 약속되어 있는 특수한 프로퍼티.
- prototype에 저장된 속성들은 생성자를 통해서 객체가 만들어질 때 그 객체에 연결된다.

    ```jsx
    function Ultra(){}
    Ultra.prototype.ultraProp = true;

    function Super(){}
    Super.prototype = new Ultra();

    function Sub(){}
    Sub.prototype = new Super();

    var o = new Sub();
    console.log(o.ultraProp);
    // true
    ```

    1. 객체 o에서 ultraProp를 찾음
    2. 없으면 Sub.prototype.ultraProp 찾음
    3. 없으면 Super.prototype.ultraProp를 찾음
    4. 없으면 Ultra.prototype.ultraProp를 찾음

    → `prototype chain`

    **Super.prototype = Ultra.prototype 으로하면...**
    Super.prototype의 값을 변경하면 그것이 Ultra.prototype도 변경하기 때문.
    Super.prototype = new Ultra();는 Ultra.prototype의 원형으로 하는 객체가 생성되기 때문에 new Ultra()를 통해서 만들어진 객체에 변화가 생겨도 Ultra.prototype의 객체에는 영향을 주지 않음.

# 표준 내장 객체의 확장

> Standard Built-in Object

- 자바스크립트가 기본적으로 가지고 있는 개체들을 의미

## 내장객체

- Object
- Function
- Array

- Boolean
- Number
- Math

- RegExp
- String
- Date

```jsx
var arr = new Array('seoul', 'new york', 'ladarkh', 'pusan', 'Tsukuba');
function getRandomValueFromArray(haystack){
	var index = Math.floor(haystack.length * Math.random());
	return haystack[index];
}
console.log(getRandomValueFromArray(arr)); 
```

내장된 객체처럼 사용하기

```jsx
Array.prototype.rand = function(){
	var index = Math.floor(this.length * Math.random());
	return this[index];
}
var arr = new Array('seoul', 'new york', 'ladarkh', 'pusan', 'Tsukuba');
console.log(arr.rand());
```

# Object

- 객체의 가장 기본적인 형태를 가지고 있는 객체
- 아무것도 상속받지 않는 순수한 객체
- js에서 값을 저장하는 기본적인 단위 Object

```jsx
var grades = { 'doanld' : 27, 'k8805' : 6, 'sorialgi' : 80 };
```

- 자바스크립트의 모든 객체는 Object객체를 상속 받음.
    - Object 객체의 프로퍼티를 가지고있음

## Object 객체의 확장

```jsx
Object.prototype.contain = function(neddle){
	for( var name in this ) {
		if( this[name] === neddle ) {
			return true;
		}
	}
	return false;
}
var o = {'name' : 'donald', 'city' : 'seoul' }
console.log(o.contain('donald'));
var a = ['donald','leezche','grapittie'];
console.log(a.contain('leezche'));
```

→ Object 객체는 확장하지 않는 것이 좋다. ⇒ 모든 객체에 영향을 주기 때문

→ 이러한 문제를 회피하기 위해서 프로퍼티의 해당 객체의 소속인지를 체크해볼 수 있는 `hasOwnProperty`를 사용

```jsx
for(var name in o){
	if(o.hasOwnProperty(name))
		console.log(name);
}
```

# 데이터 타입

## 원시 데이터 타입

- 데이터 타입이란 데이터의 형태를 의미
- 데이터 타입은 크게 두가지로 구분
    - 객체
    - 객체가 아닌 것 (원시 데이터 타입 (primitive type))

### 객체가 아닌 것

- 숫자
- 문자열
- 불리언
- null
- undefined

## 레퍼 객체

```jsx
var str = 'coding';
console.log(str.length); // 6
console.log(str.charAt(0)); // "c"
```

- 문자열은 프로퍼티와 메소드 존재, **하지만 객체는 아니다.**

    → 내부적으로 문자열이 원시 데이터 타입이고 문자열과 관련된 어떤 작업을 하려고 할 때 자바스크립트는 임시로 문자열 객체를 만들고 사용이 끝나면 제거하기 때문.

    → 이러한 처리는 내부적으로 발생

```jsx
var str = 'coding';
str.prop = 'everybody';
console.log(str.prop); // undefined
```

→ str.prop를 하는 순간 자바스크립트 내부적으로 String 객체가 만들어진다. prop 프로퍼티는 이 객체에 저장되고 이 객체는 곧 제거된다. 그렇기 때문에 prop라는 속성이 저장된 객체는 존재하지 않게된다. 이러한 특징은 일반적인 객체의 동작 방법과는 다르다.

하지만 문자열과 관련해서 필요한 가능성을 객체지향적으로 제공해야 하는 필요 또한 있기 때문에 **원시 데이터 형을 객체처럼 다룰 수 있도록 하기 위한 객체를 자바스크립트는 제공하고 있는데 그것이 레퍼객체(wrapper object)다.**

레퍼객체로는 String, Number, Boolean이 있다. null과 undefine는 레퍼 객체가 존재하지 않는다.

# 참조

## 복제

```jsx
var a = 1;
var b = a;
b = 2;
console.log(a); // 1
```

## 참조

```jsx
var a = { 'id' : 1 };
var b = a;
b.id = 2;
console.log(a.id); // 2
```

→ 객체는 참조 데이터 형(참조 자료형)

### 예제로 학습

```jsx
var a = 1;
function func(b){
	b = 2;
}
func(a);
console.log(a); // 1
```

```jsx
var a = { 'id' : 1 };
function func(b){
	b = { 'id' : 2 };
}
func(a);
console.log(a.id); // 1
```

- 함수 func의 파라미터 b로 전달된 값은 객체 a.
- `(b = a)` b를 새로운 객체로 대체하는 것은 `(b = { 'id' : 2 })` b 가 가르키는 객체를 변경하는 것이기 때문에 객체 a에 영향을 주지 않음.

```jsx
var a = { 'id' : 1 };
function func(b){
	b.id = 2;
}
func(a);
console.log(a.id); // 2
```

- 파라미터 b는 객체 a의 레퍼런스.
- 이 값의 속성를 바꾸면 그 속성이 소속된 객체를 대상으로 수정작업을 한 것이 되기 때문에 b의 변경은 a에도 영향을 미침.
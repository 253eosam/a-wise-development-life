# 함수지향

## 유효범위

-   Scope는 변수의 수명을 의미.

```javascript
var vscope = 'global';
function fscope() {
	console.log(vscope);
}
fscope();

/* result 
global
*/
```

→ 함수 밖에서 변수를 선언하면 그 변수는 전역변수가 됨.

-   전역변수는 에플리케이션 전역에서 접근 가능
-   어떤 함수안에서도 그 변수에 접근 할 수 있다.

```javascript
var vscope = 'global';
function fscope() {
	var vscope = 'local';
	console.log('함수안 ' + vscope);
}
fscope();
console.log('함수밖 ' + vscope);

/* result
함수안 local
함수밖 global
*/
```

→ 지역변수의 유효범위는 함수 안이고, 전역변수의 유효범위는 애플리케이션 전역인데, 같은 이름의 지역변수와 전역변수가 동시에 정의되어 있다면 지역변수가 `우선한다`

```javascript
var vscope = 'global';
function fscope() {
	vscope = 'local';
	console.log('함수안' + vscope);
}
fscope();
console.log('함수밖' + vscope);

/* result
함수안 local
함수밖 local
*/
```

→ 함수밖에서도 vscope의 값이 local인 이유는 함수안에서 `var`를 사용하지 않아 vscope는 지역변수가 아닌 전역변수가 된다.

**변수를 선언할때는 꼭 `var`를 붙이는 것을 습관화하는 것이 중요.
전역변수를 사용해야 하는 경우 그 이유를 명확하게 알고 사용해야함.**

### 유효범위의 효용

```javascript
function a() {
	i = 0;
}
for (i = 0; i < 5; i++) {
	a();
	console.log(i);
}
```

### 전역변수 사용

-   불가피하게 전역변수를 사용해야 하는 경우는 하나의 객체를 전역변수로 만들고 객체의 속성으로 변수를 관리하는 방법을 사용

```javascript
MYAPP = {};
MYAPP.calculator = {
	left: null,
	right: null,
};
MYAPP.coordinate = {
	left: null,
	right: null,
};

MYAPP.calculator.left = 10;
MYAPP.calculator.right = 20;
function sum() {
	return MYAPP.calculator.left + MYAPP.calculator.right;
}
console.log(sum());

/* result
30
*/
```

-   전역변수를 사용하고 싶지 않다면 익명함수를 호출 하는 방법.

```javascript
(function () {
	var MYAPP = {};
	MYAPP.calculator = {
		left: null,
		right: null,
	};
	MYAPP.coordinate = {
		left: null,
		right: null,
	};
	MYAPP.calculator.left = 10;
	MYAPP.calculator.right = 20;
	function sum() {
		return MYAPP.calculator.left + MYAPP.calculator.right;
	}
	console.log(sum());
})();
```

→ 위 방법은 자바스크립트에서 로직을 모듈화하는 일반적인 방법

### 유효범위의 대상(함수)

-   자바스크립트는 함수에 대한 유효범위만을 제공

```javascript
for (var i = 0; i < 1; i++) {
	var name = 'Hello world~!';
}
console.log(name);
```

→ 자바에서는 위 문법을 허용하지 않음

### 정적 유효범위

-   자바스크립트는 함수가 선언된 시점에서 유효범위를 가짐.

→ `정적 유효범위(static scoping), 렉시컬(lexical scoping)`

```javascript
var i = 5;

function a() {
	var i = 10;
	b();
}

function b() {
	console.log(i);
}

a();

/* result 
5
*/
```

# 값으로서의 함수와 콜백

## 값으로서의 함수

-   **JavaScript에서는 함수도 객체이다.**

→ 일종의 값 ( 자바스크립트의 언어적 특성 )

```javascript
function a() {}
```

-   변수 a에 함수 a가 담겨진 값.

```javascript
a = { b: function () {} };
```

-   함수는 값이기 때문에 다른 함수의 인자로 전달 될수도 있음.

```javascript
function cal(func, num) {
	return func(num);
}
function increase(num) {
	return num + 1;
}
function decrease(num) {
	return num - 1;
}
console.log(cal(increase, 1));
console.log(cal(decrease, 1));
```

-   함수는 함수의 리턴 값으로도 사용할 수 있음.

```javascript
function cal(mode) {
	var funcs = {
		plus: function (left, right) {
			return left + right;
		},
		minus: function (left, right) {
			return left + right;
		},
	};
	return funcs[mode];
}
console.log(cal('plus')(2, 1));
console.log(cal('minus')(2, 1));

/* result
3
3
*/
```

-   당연히 배열의 값으로도 사용가능

```javascript
var process = [
	function (input) {
		return input + 10;
	},
	function (input) {
		return input * input;
	},
	function (input) {
		return input / 2;
	},
];
var input = 1;
for (var i = 0; i < process.length; i++) {
	input = process[i](input);
}
console.log(input);

/* result
65.5
*/
```

## 콜백

### 처리의 위임

-   값으로 사용될 수 있는 특성을 이용하면 함수의 인자로 함수로 전달 가능
-   값으로 전달된 함수는 호출 될 수 있기 때문에 이를 이용하면 함수의 동작을 완전히 바꿀 수 있음.
-   인자로 전달된 함수 sortNumber의 구현에 따라서 sort의 동작 방법이 바뀜

```javascript
function sortNumber(a, b) {
	// 위 예제와 비교해서 a와 b의 순서를 바꾸면 정렬순서가 반대가 된다.
	return b - a;
}
var numbers = [20, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1];
console.log(numbers.sort(sortNumber));
```

### 비동기 처리

-   콜백은 비동기처리에서도 유용함.
-   **오래걸리는 작업이 있을 때 이 작업이 완료된 후에 처리해야 할일을 콜백으로 지정하면 작업이 끝났을 때 미리 등록한 작업을 실행 가능**

# 클로저

-   클로저(closure)는 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것

## 내부함수

-   자바스크립트는 함수 안에서 또 다른 함수를 선언 가능

```javascript
function outter() {
	function inner() {
		var title = 'Hello world~!';
		console.log(title);
	}
	inner();
}
outter();
```

→`inner`  함수를 내부함수라고 함.

```javascript
function outter() {
	var title = 'Hello world~!';
	function inner() {
		console.log(title);
	}
	inner();
}
outter();
```

→ 내부함수 `inner` 에서 `outter`의 지역변수에 접근할 수 있음.

### 클로저

-   클로저(closure)
-   내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근 할 수 있음.

```javascript
function outter() {
	var title = 'Hello world~!';
	// return에 의해 내부함수가 값으로 살아있고 그로인해 외부함수 살아있음.
	return function () {
		console.log(title);
	};
}
inner = outter();
inner();
```

→ **내부함수가 외부함수의 지역변수에 접근 할 수 있고, 외부함수는 외부함수의 지역변수를 사용하는 내부함수가 소멸될 때까지 소멸되지 않는 특성**

```javascript
function factory_movie(title){
    return{
        get_title : function(){
            return title;
        },
        set_title : function(_title){
            title = _title;
        }
}
ghost = factory_movie('Ghost');
matrix = factory_movie('Matrix');

console.log(ghost.get_title()); // Ghost
console.log(matrix.get_title()); // Matrix

ghost.set_title('공각기동대');

console.log(ghost.get_title()); // 공각기동대
console.log(matrix.get_title()); // Matrix
```

→ ghost는 title의 값을 공유하고 있음.

→ 외부함수가 실행될 때마다 새로운 지역번수를 포함하는 클로저가 생성되기 때문. `ghost 와 matrix는 서로 독립된 객체`

→ `factory_movie`  의 지역변수는 정의된 객체의 메소드에서만 접근 할 수 있음.
_factory_movie 메소드를 통해서 만들어진 객체 뿐_
⇒ 자바스크립트는 기본적으로 private한 속성을 지원하지 않는데, 클로저의 이러한 특성을 이용해서 private 한 속성을 사용할 수 있다.

private 속성은 객체의 외부에서 접근 할 수 없는 외부에 감춰진 속성이나 메소드를 의미.
이를 통해 객체의 내부에서만 사용해야 하는 값이 노출됨으로서 생길 수 있는 오류를 줄일 수 있음.

```javascript
var arr = [];
for (var i = 0; i < 5; i++) {
	arr[i] = function () {
		return i;
	};
}
for (var index in arr) {
	console.log(arr[index]());
}
// 5,5,5,5,5
```

```javascript
var arr = [];
for (var i = 0; i < 5; i++) {
	arr[i] = (function (id) {
		return function () {
			return id;
		};
	})(i);
}
for (var index in arr) {
	console.log(arr[index]());
}
// 0,1,2,3,4
```

# arguments

-   함수에 `arguments`의 변수에 담긴 유사 배열 존재.

-   함수를 호출할 때 입력한 인자가 담김

```javascript
function sum(){
var i, _sum = 0;
for(i = 0 ; i < arguments.length, i++){
console.log(i + ' : ' + arguments[i]);
_sum += arguments[i];
}
return _sum;
}
console.log('result : ' + sum(1,2,3,4));
```

→ `arguments`라는 객체의 인스턴스 (배열처럼 보일 수 있음)

## 매개변수의 수

-   .length : 함수에 정의된 인자의 수
-   arguments.length : 함수로 전달되는 실제 인자의 수

```javascript
function zero() {
	console.log('zero.length', zero.length, 'arguments', arguments.length);
}
function one(arg1) {
	console.log('one.length', one.length, 'arguments', arguments.length);
}
function two(arg1, arg2) {
	console.log('two.length', two.length, 'arguments', arguments.length);
}
zero(); // zero.length 0 arguments 0
one('val1', 'val2'); // one.length 1 arguments 2
two('val1'); // two.length 2 arguments 1
```

## 함수호출

-   기본방법

```javascript
function func() {}
func();
```

-   `func`는 객체 `Function`이 가지고 있는 메소드들을 상속하고 있음.

```javascript
function sum(arg1, arg2) {
	return arg1 + arg2;
}
console.log(sum.apply(null, [1, 2]));
```

-   `sum`은 Function 객체의 인스턴스.
-   첫번째 인자는 함수(sum)가 실행될 맥락.
-   두번째 인자는 배열, 배열에 담겨있는 원소가 함수(sum)의 인자로 순차적으로 대입

```javascript
o1 = { val1: 1, val2: 2, val3: 3 };
o2 = { v1: 10, v2: 50, v3: 100, v4: 25 };
function sum() {
	var _sum = 0;
	for (name in this) {
		_sum += this[name];
	}
	return _sum;
}
console.log(sum.apply(o1)); // 6
console.log(sum.apply(o2)); // 185
```

→ 객체 Function의 메소드 apply의 첫번째 인자는 함수가 실행될 맥락?

-   `sum.apply(o1)` 는 함수 sum을 객체 o1의 메소드로 만들고 sum을 호출한 후에 sum을 삭제.

```javascript
o1.sum = sum;
console.log(o1.sum());
delete o1.sum();
```

→ 일반적으로 객체지향 언어에서는 하나의 객체에 소속된 함수는 그 객체의 소유물. **하지만 Javascript의 함수는 독립적인 객체로서 존재, `apply` & `call` 메소드를 통해 다른 객체의 소유물인 것처럼 실행 가능.**

→ apply의 첫번째 인자로 `null` 을 전달하면 apply가 실행된 함수 인스턴스는 `전역객체(브라우저에서는 window)를 맥락으로 실행

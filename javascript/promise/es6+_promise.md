# ES6+ Promise

- 싱글쓰레드인 자바스크립트에서 비동기 처리를 위해서 콜백(callback)을 사용.
- 비동기 처리를 처리하는데 유연함

    → 많이 사용 시 발생할 수 있는 단점들이 생겨남.

## 목적

- 비동기 처리를 순차적으로 실행할 필요성.

    → 중첩시켜서 표현

    - 에러, 예외처리가 어려움.
    - 중첩으로 인한 복잡도 증가.
- **위 2가지 목적을 해결하기 위해서 `라이브러리` 로 지원, `ES6`에서 지원..!!**

---

> 비동기 처리에서 성공과 실패를 분리하여 메소드로 수행..!!

### 예외 처리 실패

```jsx
try{
	setTimeout(() => { throw 'Error!'; }, 1000);
} catch (e) {
	console.log('do not catch error..');
	console.log(e);
}
```

try-catch에서 비동기 함수의 콜백메소드에서 에러를 만들지만 catch하지 못함.(setTimeout()함수의 콜백은 이벤트큐에 있다가 콜스택이 비어지면 실행되기 때문)
고로 콜백메서드의 중첩은 에러 처리가 힘듬.

### 프로미스 생성, 실행

```jsx
// create promise
const promise1 = function(param){
	return new Promise(function(resolve, reject){
		if(param) resolve("A");
		else rejecgt("B");
	});
}
// play promise
promise1(true).then(function(result){
	console.log(result); // A
},function(err){
	console.log(err); // B
});
```

- `new Promise((resolve, reject){...})` 로 생성
- `fulfilled state` → `resolve() 실행`
- `rejected state` → `reject() 실행`

> 💡 비동기함수를 만들어서 사용해야할 때 프로미스 객체를 리턴하게 만들어서 사용하면 콜백 헬(콜백 중첩)을 방지할 수 있고 에러처리를 수원할게 할 수 있음. 
<br><br>
> 다른 누군가 만든 비동기 함수 역시 promise 객체를 리턴하게 만들어 놨으므로 promise를 실행(.then or .catch)를 하면 됨.

### 비동기 함수 중간에 에러가 난다..? Promise.catch()

```jsx
asyncThing1()
	.then(function() { return asyncThing2();})
	.then(function() { return asyncThing3();})
	.catch(function(err) { return asyncRecovery1();})

	.then(function() { return asyncThing4();}, function(err) { return asyncRecovery2(); })
	.catch(function(err) { console.log("Don't worry about it");})

	.then(function() { console.log("All done!");});
```

- 비동기 메소드가 프로미스 객체를 리턴하면 계속해서 .then, .catch하는 식으로 체이닝이 가능
- .catch는 비동기 메소드중에 에러가 나면 캐치가 가능

[프로미스 실행 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile22.uf.tistory.com%2Fimage%2F99E159505A6F3413271E8C)

### 여러개의 프로미스가 모두 완료되었을 때 실행하는 방법

```jsx
const param = true;
const promise1 = new Promise(function(resolve, reject){
	if(param) resolve("A");
	else reject("B");
});
const promise2 = new Promise(function(resolve, reject){
	if(param) resolve("A2");
	else reject("B2");
});
Promise.all([promise1, promise2]).then(function(values){
	console.log("1,2,3 all completed", values);
});

// 1,2,3 all completed
// [A,A2]
```

- Promise.all()을 사용하면 파라미터로 갖는 모든 프로미스가 완료되면 수행하고 프로그미스들의 값을 보여줌.
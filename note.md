#### 현재 app 구조

`TodoInput`에서 `newTodoItem`의 데이터를 받아서 추가 -> 로컬스토리지에 새로 생김 -> `TodoList`에 바로 반영이 안된다(새로고침해야만 반영). 분리된 구조.

![before](./img/todo-before.png )


#### 개선한 app구조
`TodoInput`, `TodoList` 등 하위 컴포넌트는 표현만 할 것. 실질적인 기능 동작은 상위 컴포넌트인 `App`에서 이루어지도록 한다. `App` 컴포넌트에서는 props로 속성만 내릴 것.
하위 컴포넌트에서 신호를 받았을 때 제어하는 구조.

![after](./img/todo-after.png )

`App`의 경우에는 Container라는 개념으로 보면 된다.
컴포넌트 설계 관점으로 보면 컴포넌트에도 
단순히 화면에 표현하기만하는 컴포넌트가 있는가하면(presentor),
앱의 동작 등의 데이터 조작을 하는 걸 container라고 함

이렇게 하면 한 곳에서 관리를 할 수 있게되어 각 이벤트별로 분리/해당 내용을 props로 내려주기만 하면 전체적인 컴포넌트 동작을 매끄럽게 이을 수 있다.


템플릿 작성시 하나의 ```<template>``` 안에는 한 개의 root 태그만 있어야 한다. 

## slot 
특정 컴포넌트의 일부 UI들만 재사용할 수 있는 기능. '재정의'하는 관점에서 slot을 사용한다. 
Modal 중에서 케이스가 나뉘는 경우, slot으로 같은 컴포넌트를 재사용하는 것.
Slot의 내용을 갖고있는 컴포넌트를 등록한 컴포넌트에서 재정의할 수 있다.

Modal.vue

     <slot name="header"> default header</slot>

TodoInput.vue

    <h3 slot="header">custom header</h3>


## Transitions

사용 관점: 트랜지션이나 애니메이션이 부수적인 효과로만 취급되는 게 아니라, 프레임워크 자체에서 지원되기 때문에 간단한 이펙트들을 바로바로 추가할 수 있다. 

트랜지션을 사용할 때는 [클래스 이름](https://vuejs.org/v2/guide/transitions.html#Transition-Classes)에 따라 트랜지션들이 조합이 된다. 



## ES6
1. const & let
  	- 새로운 변수 선언 방식
  	- 기존 자바스크립트의 유연함(애매모호함) 때문에 생기는 오류들을 방지할 수 있다.
  	- 블록 단위 ```{}``` 로 변수의 범위가 제한되었다.
  	- ```const``` : 한번 선언한 값에 대해 **변경할 수 없음**
  	- ```let``` : 한번 선언한 값에 대해 **다시 선언할 수 없음**
  
```
// ES5
var a = 10;

// ES6
let b = 20;
const c = 30;

// let과 const의 차이점
// const는 한 번 선언하면 값을 바꿀 수 없다
let a = 10;
a = 20;   // a=20
const b = 10;
b = 20;   // ERROR

let c = 10;
let c = 20;   // ERROR! 한 번 선어난 변수는 재선언 불가. 값을 바꾸는 것만 가능

// const 배열은 push가 가능
const arr = [];
arr.push(10);   // arr: 10


// var
var a = 10;
if(true){
	var a=20;
	console.log(a);   // 20
}

// let
let a = 10;
if(true){
let a = 20;      // 사용 가능. 재선언이 아니라 블록 안에 들어가서 범위가 달라진 것
console.log(a);  // 20
}
```
  
---
 2. ES5 특징
 * 변수의 Scope
	* 기존의 ES5는 {}에 상관없이 스코프가 결정됨
    
```
// {}를 기점으로 블록이 구분된다
for(var i=0; i<5; i++){
console.log('반복문 안', i);  // 0,1,2,3,4
}
console.log('반복문 밖', i);   // 5

for(let k=0; k<4; k++){
console.log('반복문 안', k);
}
console.log('반복문 밖',k);    // ERROR
```


* Hoisting
	* Hoisting이란 선언한 함수와 변수를 해석기가 가장 상단에 있는 것처럼 인식되는 것(끌어올려지는 것)을 의미한다.
	* js 해석기는 라인 순서와 관계 없이 함수 선언문과 변수를 위한 메모리 공간을 먼저 확보한다.
	* 따라서 ```function a()```과 ```var```는 코드의 최상단으로 끌어올려진 것처럼(hoistied) 보인다.


```
var sum = 5;
sum = sum + i;    // 에러가 나지 않는다

function sumAll(){
...
}

var i = 10;


// 실제 할당 순서
// #1. 함수 선언식과 변수 선언을 hoisting
var sum = 5;
function sumAll(){
...
}
var i;

// #2 변수 대입 및 할당, 연산
sum = 5;
sum = sum + i;
i = 10;
```

----
3. 함수 선언문 변화

    
```
// 함수 선언문
function sum(){
}

// 표현식
var sum = function(a,b){
return a+b;
}

// ES6에서의 함수 선언문 변화
// function()에서 화살표로
var sum = (a,b) => {
return a+b;
}
```

```
function f(){
	{
		let x;
		{
			// 새로운 블록 안에 새로운 x의 스코프가 생김
			const x = "sneaky";
			x = "foo";    // 위에 이미 const로 x를 선언했으므로 다시 값을 대입하면 에러 발생
		}
		// 이전 블록 범위로 돌아왔기 때무에 'let x'에 해당하는 메모리에 값을 대입
		x = bar;
		let x = "inner";   // Uncaught SyntaxError: Identifier 'x' has already been declared
	}
}
```

----
4. 화살표 함수
```
// ES5 함수 정의 방식
var sum = function(a,b){
	return a+b;
};

// ES6 함수 정의 방식
var sum = (a, b) => {
	return a+b;
};
```
```
// ES5
var arr = ["a","b","c"];
arr.forEach(function(value){
	console.log(value);   // a, b, c
});

// ES6
var arr = ["a","b","c"];
arr.forEach(value => console.log(value));   // a, b, c
```

----
5. Enhanced Object Literals(향상된 객체 리터럴)
* 객체의 속성을 메서드로 사용할 때 function 예약어를 생략하고 생성 가능. 괄호 하나만으로 속성 메서드 함수의 역할을 한다
```
var dictionary = {
	words: 100,
	// ES5
	lookup: function(){
		console.log("find words");
	},
	// ES6
	lookup(){
		console.log("find words");
	}
};
```



## Vuex
복잡한 어플리케이션의 컴포넌트들을 효율적으로 관리해주는 라이브러리
* Vuex 라이브러리의 주요 속성
	1. state: 여러 컴포넌트에 공유되는 ```data```(property)
	2. getters: 연산된 state값을 접근하는 속성 ```computed```
	3. mutations: state값을 변경하는 이벤트 로직, 메서드 ```methods```
	4. actions: 비동기 처리 로직을 선언하는 메서드 ```aysnc methods```

* Vuex로 해결할 수 있는 문제
	- MVC 패턴에서 발생하는 구조적 오류(양방향 데이터 전송을 단방향으로 변경)
	- 컴포넌트 간 데이터 전달 명시
	- 여러 개의 컴포넌트에서 같은 데이터를 업데이트 할 때 동기화 문제


#### State
- 여러 컴포넌트 간에 공유할 데이터, 상태
```
// Vue
data: {
	message: 'Hello Vue.js!'
}

// Vuex
state: {
	message: 'Hello Vue.js!

}
```
```
<!-- Vue -->
<p>{{ message }}</p>

<!-- Vuex -->
<!-- Vue.use(Vuex) 처럼 global functionality로 접근했기 때문에 이런 식으로 사용 -->
<p>{{ this.$store.state.message }}</p>
```


#### getters
- state 값을 접근하는 속성이자 ```computed()```처럼 미리 연산된 값을 접근하는 속성
```
// store.js
state: {
	num: 10
},
getters: {
	getNumber(state){
		return state.num;
	},
	doubleNumber(state){
		return state.num * 2;
	}
}
```
```
<p>{{ this.$store.getters.getNumber }} (10)</p>
<p>{{ this.$store.getters.doubleNumber }} (20)</p>
```


#### mutations
- state의 값을 변경할 수 있는 유일한 방법이자 메서드
- mutation은 ```commit()```으로 동작시킨다.
```
// store.js
state: {
	num: 10
},
mutations: {
	printNumbers(state){
		return state.num
	},
	sumNumbers(state, anotherNum){
		return state.num + anotherNum;
	}
}

// App.vue
this.$store.commit('printNumbers');
this.$store.commit('sumNumbers',20);
```

- state를 변경하기 위해 mutations를 동작시킬 때 인자를 전달할 수 있음
- 파라미터 이름은 편의상 payload라고 많이들 쓴다...
- 여러가지 값을 보내고 싶을 땐 객체화시켜서 key: value로 넘길 수 있음
```
// store.js
state: { storeNum : 10 },
mutations:{
	modifyState(state, payload){
		console.log(payload.str);
		return state.storeNum += payload.num;
	}
}
// App.vue
this.$store.commit('modifyState', {
	str: 'passed from payload',
	num: 20
})
```
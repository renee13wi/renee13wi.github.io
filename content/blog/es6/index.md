---
title: ECMAScript 2015
date: "2021-01-18T23:00:03.284Z"
description: ECMAScript 2015는 ECMAScript 언어의 6번째 표준 스펙(Spec)입니다.  
--- 

넷스케이프에서 자바스크립트를 지원하면서 자바스크립트가 성공하자 마이크로소프트가 J스크립트를 개발했습니다.  
넷스케이프는 표준화를 위해 자바스크립트 기술 규격을 ECMA 인터내셔널에 제출하였고 ECMA-262라는 표준이 생겨났습니다.  
ECMAScript 2015는 ECMAScript 언어의 6번째 표준 스펙(Spec)입니다.  

이 컨텐츠는 Nomard ECMAScript 2015 강의에 의거하여 ECMAScript 2015에서 새롭게 추가된 기능을 정리해보고자 합니다.  

## variables

### Let and Const
ES5까지 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이였다.  
ES6에서는 블록 유효 범위를 갖는 새로운 변수 선언 방법을 지원한다.  
바로 let, const이다.  
let은 var와 유사하게 동작하고, const는 재할당 및 재선언이 불가능하다.

``` jsx
var name = "lalalal";
// 큰 어플리케이션을 만들 때 var 문제가 될 수 있음

const name = "renee"; // const는 변하지 않는다는 뜻
name = "eunhye"; 
// Error ..? const가 variable의 내용이 변하는 걸 막아주기 때문이다, 값을 다시 정할 수 없게 된다

const person = {
  name: 'renee'
}
person.name = "eunhye" // 객체 안에 어떤걸 바꿀 순 있음

// let은 이전의 var와 같은것
let renee = "las";
renee = "lalala";

// 되도록이면 const를 사용하자
```

ES5의 var는 큰 어플리케이션을 만들 때 문제가 될 수 있다.  
ES6에서 새롭게 추가된 let은 var와 같이 값을 재 설정 할 수 있다.
반면 const는 객체 안에 선언된 값을 변경하는 것은 가능하지만 그 외 값 변경은 불가능하다.

### DeadZone
temporal dead zone은 let이랑 같이 소개되는 개념

자바스크립트는 위에서 부터 아래로 코드를 실행함
-> 무조건 위에서 아래로 진행되는게 아니다.

호이스팅은 자바스크립트가 프로그램을 실행하기 전에 
var로 선언된 부분을 어딘가로 이동시킨다는 것
호이스팅은 프로그램이 시작되면 그들이 제일 위로 가는 것을 의미함

```jsx
console.log(myName); // undefinded
var myName = "renee";

var myName;
console.log(myName);
myName = "renee";
// var들을 제일 위로 올려주는 것, variable들이 미리 끌어올려진다는 것 : 호이스팅
```

### blockscope
scope는 기본적으로 버블이다.  
이 버블이 variable에 접근 가능한지 아닌지를 감지해준다.

```jsx
if(true) {
	const hello = "hi";
}
console.log(hello); // hello is not defined

if(true) {
	var hello = "hi";
}
console.log(hello); // hi
```

이 경우에는 const, let 둘다 block scope로 되어있다.
이 말은 그 block안에서만 존재한다는 뜻, block은 {} 의미한다.
var를 쓰면 block scope가 없어서 console.log 실행시 hi가 출력되는 것을 볼 수 있다.
var는 function scope를 가지고 있다.

```jsx
function a () {
	var hello = "hi";
}
console.log(hello); // hello is not defined
```

함수 레벨 스코프(Function-level scope) : 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. 즉, 함수 내부에서 선언한 변수는 지역 변수이며 함수 외부에서 선언한 변수는 모두 전역 변수이다.  

블록 레벨 스코프(Block-level scope) : 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다. 즉, 코드 블록 내부에서 선언한 변수는 지역 변수이다.

var는 그만 사용하는게 좋다. 많은 사람들이 root에서 var를 교체하거나 var를 그냥 수정하지 않는 이유는 많은 웹 사이트를 망가트리기 때문이다. 
자바스크립트란 언어는 웹에서 사용되는 언어로서 var를 사용하고 있거나 앞으로도 사용할 웹 사이트가 엄청 많을거다.
만약 한순간에 다른걸로 바뀐다고 생각해본다면 많은 웹사이트들이 깨지게 될 것이다.  
그게 바로 새로운 단어 let과 const를 만든 이유다.

---

## Functions
### Arrow Function
Arrow Function은 자바스크립트에서 함수의 모습을 개선한 것으로서 좀 더 보기 좋게 만든 것을 의미한다. 

#### 화살표 함수 선언 방법
``` jsx
// 매개변수 지정 방법
    () => { ... } // 매개변수가 없을 경우
     x => { ... } // 매개변수가 한 개인 경우, 소괄호를 생략할 수 있습니다.
(x, y) => { ... } // 매개변수가 여러 개인 경우, 소괄호를 생략할 수 없습니다.

// 함수 몸체 지정 방법
x => { return x * x }  // single line block
x => x * x             // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다. 위 표현과 동일하다.

() => { return { a: 1 }; }
() => ({ a: 1 })  // 위 표현과 동일하다. 객체 반환시 소괄호를 사용한다.

() => {           // multi line block.
  const x = 10;
  return x * x;
};
```

``` jsx

function nameOf(arg){

}
// 익명함수를 만들 때
const hello = function(arg){ 
  
}
const names = ["nico", "lynn", "flynn"];

function addHeart(item) {
  return item + "하트";
}

const hearts = names.map(function(item) 
  return item + "하트";
);

// map은 각각의 아이템마다 함수를 호출하는 일을 한다
// 각각의 아이템에서 값을 return하고 그게 새로운 array가 된다.
// const hearts = names.map(addHeart);

const hearts = names.map(item => {
  return item + "하트";
});

const hearts = names.map(item => item + "하트"); 
// implicit return : 같은 줄에 뭘 적던지 간에 return됨

const hearts = names.map(item => { 
  // {} 사용 시 implicit return의 기능은 사라짐
  return item + "하트";
}) 

console.log(hearts);
```

### Arrow Function을 사용하지 않아야 할 때

하지만 this 키워드를 사용해야하는 경우를 제외하고 대부분의 경우에 arrow function을 사용 할 수 있다.
화살표 함수 안에 this는 window를 참조한다.  
바깥 bubble을 참조한다.  
this를 쓰고 싶으면 arrow function을 쓰면 안되고 화살표 함수는 this를 window object로 가지고 있다.

```jsx
const button = document.querySelector("button");

button.addEventListener("click", function(){
  this.style.backgroundColor = "red"
});

button.addEventListener("click", () => {
  this.style.backgroundColor = "red"
  console.log(this); // window 출력, cannot set property 'backgroundColor' of undefined
});

const renee = {
  name: "eunhye",
  age: 24, 
  addYear: () => {
    this.age++
  }
}

console.log(renee.age); // 24
renee.addYear();
renee.addYear();
console.log(renee.age); // 24
```

#### this

화살표 함수는 this 키워드를 사용하는 경우를 제외하곤 대부분 사용 가능하다.
하지만 화살표 함수를 사용하지 말고 일반 함수를 사용해야 될 때가 있다.
- addEventListener 함수의 콜백 함수
- 메소드
- prototype
- 생성자 함수

##### addEventListener 함수의 콜백 함수
화살표 함수는 this를 이벤트로부터 가지고 있지 않다.  
만약 사용하고 싶다면 function을 사용해야된다.

``` js
const button = document.querySelector("button");

// 화살표 함수
button.addEventListener("click", () => {
  console.log(this); // window 출력, cannot set property 'backgroundColor' of undefined
  this.style.backgroundColor = "red"
})

// 일반적인 함수
button.addEventListener("click", function(){
  // this // 이벤트 리스너를 붙이고 이벤트 리스너에 function이 있으면 자바스크립트는 버튼을 this 키워드에 넣는다.
  this.style.backgroundColor = "red" // 작동됨
});
```

##### 메소드
``` jsx
// 화살표 함수
const test = {
  name : "renee",
  age : "29",
  addAge : () => {
    this.age ++;
    console.log(`2020년에는 ${this.age}살이다...`)
  }
};

console.log(test); // {name: "renee", age: "29", addAge: ƒ}
test.addAge(); // 2020년에는 NaN살이다

// 일반적인 함수
const test = {
  name : "renee",
  age : "29",
  addAge() {
    this.age ++;
    console.log(`2020년에는 ${this.age}살이다...`)
  }
}
console.log(test); // {name: "renee", age: "29", addAge: ƒ}
test.addAge(); // 2020년에는 30살이다
```
화살표 함수로 객체의 메소드를 정의하였을 때와 같은 문제가 발생한다.  
따라서 prototype에 메소드를 할당하는 경우, 일반 함수를 할당한다.

##### prototype
``` jsx
// 화살표 함수
const test = {
	name: 'renee',
};

Object.prototype.greeting = () => console.log(`Hi ${this.name}`);
test.greeting(); // Hi undefined

// 일반적인 함수
const test = {
	name: 'renee',
};

Object.prototype.greeting = function() {
  console.log(`Hi ${this.name}`);
};

test.greeting(); // Hi renee
```
##### 생성자 함수
화살표 함수는 생성자 함수로 사용할 수 없다.
생성자 함수는 prototype 프로퍼티를 가지며 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor를 사용한다.
하지만 화살표 함수는 prototype 프로퍼티를 가지고 있지 않다.

``` jsx
const Test = () => {};

// 화살표 함수는 prototype 프로퍼티가 없다
console.log(Test.hasOwnProperty('prototype')); // false

const test = new Test(); // TypeError: Test is not a constructor
```


### 화살표 함수를 사용한 Array Operation 관한 내용

```Array.prototype.```  
find : 제공되는 테스트 조건을 만족하는 첫번째 엘리먼트 값을 리턴하는 함수  
filter : 메소드는 제공된 함수의 조건을 만족한 모든 엘리먼트로 새로운 array를 만듬  
map : forEach지만 반환된 element들로 새로운 array를 만듬  
forEach : 각 array의 엘리먼트 마다 제공된 함수를 실행함

```jsx
const emails = ["nco@no.com", "naver@google.com", "lynn@gmail.com", "nico@nomad.com", "nico@gmail.com"];

const foundMail = emails.find(item => item.includes("@gmail.com"));
console.log(foundMail); 

const noGmail = emails.filter(email => !email.includes("@gmail"))
console.log(noGmail);

const clean = emails.map(email => email.split("@")[0]);
console.log(clean);

emails.forEach(email => {
	clean.push(email.split("@")[0]);
});
```

### Default Values

만약에 ```parameter값(aName)```이 정의되지 않았다면 ```hello anonymous```라고 출력된다.
하지만 이 방법은 좋은 방법이 아닙니다. ```A || B``` : 앞에 것이 존재 하지 않으면 뒤에 것을 가져가고,
뒤에 것이 존재하지 않으면 앞에 것을 가져갑니다.
```A || B```를 Default Value를 이용해서 변형하게 되면 parameter부분에 ```function test(A = B) {}```
이같이 정의하게 되면 parameter값이 정의되지 않았을 때 Default Value를 쓸 수 있게 됩니다.

```jsx
function sayHi(aName) {
	return "Hello " + aName;
}

function sayHi(aName) {
	return "Hello " + (aName || " anon");
}

function sayHi(aName = " anon"){
	return "Hello" + aName;
}

const sayHi = (aName = "anon") => "hello " + aName;

console.log(sayHi());
```





---

##### 참고사이트
[MDN ECMAScript2015](https://developer.mozilla.org/ko/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)  
[우아한형제들 기술블로그](https://woowabros.github.io/experience/2017/12/01/es6-experience.html)  
[Nomad ECMAScript2015](https://nomadcoders.co/es6-once-and-for-all)  
[Poiemaweb ECMAScript2015](https://poiemaweb.com/es6-block-scope)
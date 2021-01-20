---
title: ECMAScript 2015
date: "2021-01-18T23:00:03.284Z"
description: ECMAScript 2015는 ECMAScript 언어의 6번째 표준 스펙(Spec)입니다.  
--- 

넷스케이프에서 자바스크립트를 지원하면서 자바스크립트가 성공하자 마이크로소프트가 J스크립트를 개발했습니다.
넷스케이프는 표준화를 위해 자바스크립트 기술 규격을 ECMA 인터내셔널에 제출하였고 ECMA-262라는 표준이 생겨났습니다.
ECMAScript 2015는 ECMAScript 언어의 6번째 표준 스펙(Spec)입니다.  

이 컨텐츠는 **Nomard ECMAScript 2015** 강의에 의거하여 ECMAScript 2015에서 새롭게 추가된 기능을 정리해보고자 합니다.  

---

## variables

### Let and Const
ES5까지 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이였다.  
ES6에서는 블록 유효 범위를 갖는 새로운 변수 선언 방법을 지원한다.  
바로 let, const이다.  
let은 var와 유사하게 동작하고, const는 재할당 및 재선언이 불가능하다.  

[MDN Let](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)  
[MDN Const](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)  

```jsx
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

[MDN block](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/block)  

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

[MDN Arrow Function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98)  

#### 화살표 함수 선언 방법
```jsx
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

```jsx

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
```jsx
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
```jsx
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

```jsx
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

## Strings
### Template literals
ES6에서는 템플릿 리터럴(Template literal)이라고 불리는 새로운 문자열 표기법을 도입하였다.  
템플릿 리터럴은 일반 문자열과 비슷해 보이지만, ', " 같은 통상적인 따옴표 문자 대신 백틱(backtick) 문자 `를 사용한다.

[MDN Template literals](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)  

``` js
// ES5
const sayHi = (aName = "anon") => "hello" + aName + "test";

// ES6
const sayHi = (aName = "anon") => `hello ${aName} test`;

// ES6
const add = (a, b) => a + b;
console.log(`hello how are you ${add(6, 6)}`);
```

템플릿 리터럴은 + 연산자를 사용하지 않아도 간단한 방법으로 새로운 문자열을 삽입할 수 있는 기능을 제공합니다.  
이를 문자열 인터폴레이션(String Interpolation)이라 한다.  
문자열 인터폴레이션 ${...}으로 표현합니다.

### Fragment HTML
Javascript 안에서 html을 쓸 수 있다는 점이다.  

[MDN DocumentFragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)  

``` js
const wrapper = document.querySelector(".wrapper");

// ES5
const addWelcome = () => {
	const div = document.createElement("div");
	const h1 = document.createElement("h1");
	h1.innerText = "Hello";
	h1.className = "title";
	div.append(h1);
	wrapper.append(div);
};

// ES6
const addWelcome = () => {
	const div = `
		<div>
			<h1 class="title">Hello</h1>
		</div>
	`
	wrapper.innerHTML = div;
};

setTimeOut(addWelcome, 5000);
```
``` js
// ES6
const friends = ["eunhye", "hyelim", "jiyoon"]
const list = `
	<h1>People i love</h1>
	<ul>
		${friends.map(friend => `<li>${friend}</li>`).join("")}
	</ul>
`
```

### Cloning Styled Components
Styled Components는 리액트 라이브러리, 패킷에서 주로 쓸 수 있습니다.
```js
// ES6
const styled = css => console.log(css);
styled`border-radius:10px;color:red`; 
// 이렇게 호출하면 string은 argument가 되는 형식

const styled = aElement => {
	const el = document.createElement(aElement);
	return args => {
		const styles = args[0];
		el.style = styles;
		return el;
	};
}
const title = styled("h1")`
	background-color: red;
	color: blue;
`;
const subtitle = styled("span")`
	color: green;
`;
title.innerText = "We just cloned";
subtitle.innerText = "styled Components";
document.body.append(title, subtitle);
```
---

## Array
배열(array)은 1개의 변수에 여러 개의 값을 순차적으로 저장할 때 사용합니다.  
자바스크립트의 배열은 객체이며 유용한 내장 메소드를 포함하고 있습니다.  
배열은 Array 생성자로 생성된 Array 타입의 객체이며 프로토타입 객체는 Array.prototype입니다.  

### Array.from() and Array.of()
**Array.of()** : 메서드는 인자의 수나 유형에 관계없이 가변 인자를 갖는 새 Array 인스턴스를 만듭니다.  

[MDN Array.of()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/of)

``` js
const friends = ["nico", "lynn", "dal", "mark"];
const friends = Array.of("nico", "lynn", "dal", "mark");
console.log(friends);
```

**Array.from()** : 메서드는 유사 배열 객체(array-like object)나 반복 가능한 객체(iterable object)를 얕게 복사해 새로운Array 객체를 만듭니다.  

[MDN Array.from()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)  

``` HTML
<button class="btn">1</button>
<button class="btn">2</button>
<button class="btn">3</button>
<button class="btn">4</button>
<button class="btn">5</button>
<button class="btn">6</button>
<button class="btn">7</button>
<button class="btn">8</button>
<button class="btn">9</button>
<button class="btn">10</button>
```
``` js
const buttons = document.getElementsByClassName("btn");

const buttons = document.querySelectorAll("button");
console.log(buttons); // NodeList(10)

buttons.forEach(button => { // buttons 는 forEach란 메소드를 가지고 있지 않다.
  button.addEventListner("click", function(){
    console.log("I ve been clicked")
  });
});
Array.from(buttons).forEach(button => {
  button.addEventListner("click", function(){
    console.log("I ve been clicked")
  });
});
```
buttons는 Array인 것 같지만 사실 Array가 아닙니다. Array-like Object라고 표현합니다.
위 코드와 같이 buttons는 forEach란 메소드를 가지고 있지 않습니다. 이럴 때는 ```Array.from(buttons)```로 호출하면
forEach 메소드를 가지고 있어 buttons에 해당 이벤트 함수를 적용 할 수 있습니다.  

### Array.find() Array.findIndex() Array.fill()

**Array.find()** : 메서드는 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환합니다. 그런 요소가 없다면 undefined를 반환합니다.  

[MDN Array.find()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)  
```js
const friends = [
	"nico@gmail.com",
	"lynn@naver.com",
	"dal@yahoo.com",
	"mark@hotmail.com",
	"flynn@korea.com"    
];
const target = friends.find(friend => friend.includes("@korea.com"));
console.log(target); // flynn@korea.com
```

**Array.findIndex()** : 메서드는 주어진 판별 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환합니다. 만족하는 요소가 없으면 -1을 반환합니다.  

[MDN Array.findIndex()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)  

```js
const friends = [
	"nico@gmail.com",
	"lynn@naver.com",
	"dal@yahoo.com",
	"mark@hotmail.com",
	"flynn@gorea.com"
];

const check = () => friends.findIndex(friend => friend.includes("@gorea.com"));
check(); // 4
```

**Array.fill()** : 메서드는 배열의 시작 인덱스부터 끝 인덱스의 이전까지 정적인 값 하나로 채웁니다.  

[MDN Array.fill()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)  

---

## Destructuring
디스트럭처링(Destructuring)은 구조화된 배열 또는 객체를 Destructuring(비구조화, 파괴)하여 개별적인 변수에 할당하는 것이다.
배열 또는 객체 리터럴에서 필요한 값만을 추출하여 변수에 할당하거나 반환할 때 유용하다.

### Object destructuring (객체 디스트럭처링)

[MDN 객체 구조 분해](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EA%B0%9D%EC%B2%B4_%EA%B5%AC%EC%A1%B0_%EB%B6%84%ED%95%B4)  

``` javascript
const settings = {
	notifications: {
		follow: true,
		alerts: true,
		unfollow:false
	},
	color: {
		theme: "dark"
	}
}

// 이전 방식
if(settings.notifications.follow) {

}

// Destructuring 구조
const {
	notifications: {follow},
	color
} = settings;

console.log(follow) // true

const {follow} = settings;
// 이렇게는 follow에 도달할 수 없다. 
// 먼저 notifications로 접근하고 notifications 안에 있는 follow로 접근 할 수 있다.

const {
	notifications: {
		follow
	}
} = settings;
// const notifications = settings.notifications 와 같은 것
```
### Function Destructuring (함수 디스트럭처링)

``` javascript
/*
 user 세팅을 저장하는 함수라고 하고 이름을 saveSetting
*/

function saveSettings(followAlert, unfollowAlert, mrkAler, themeColor) {
	console.log(color);
}

function saveSettings({ follow, alert, color="greens" }) {
	console.log(color);
}

function saveSettings({ notifications, color: { theme } }) {
	console.log(color);
}

saveSettings({
	notifications: {
		follow: true,
		alert: true,
		mkt: false
	},
	color: {
		theme: "blue"
	}
});
```

### Array Destructuring (배열 디스트럭처링)

[MDN 배열 구조 분해](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EB%B0%B0%EC%97%B4_%EA%B5%AC%EC%A1%B0_%EB%B6%84%ED%95%B4)  

``` javascript
const days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

// const mon = days[0];
// const tue = days[1];
// const wed = days[2];
const [mon, tue, wed] = days;
//const [mon, tue, wed, thu = "Thu"] = days;
```

---

## For of Loop

``` javascript
/*
  루프는 기본적으로 같은 일을 반복하는 것 
*/
const friends = ["Nico", "Lynn", "ha", "hu"]

for ( let i = 0; i < friends.length; i++ ) {
	console.log(`${i} I love kimchi`);
	console.log(friends[i]);
}
```
``` javascript
// forEach는 배열에 있는 각각의 엘리먼트에 대해 특정한 액션을 실행
// forEach는 current item 말고 index를 붙여서 함수를 호출 할 수 있다
// 첫번재 인자 : current item
// 두번째 인자 : index
// 세번째 인자 : current array
const friends = ["Nico", "Lynn", "ha", "hu"]
const addHeart = (c, i, a) => console.log(c, i, a);
friends.forEach(addHeart);
```
``` javascript
// forEach와는 다르다
// const로 선언 할 지, let으로 선언 할 지 선택할 수 있다
// forEach에서는 안됨
const friends = ["Nico", "Lynn", "ha", "hu"]

for (const friend of friends) {
	console.log(friend);
}
```
``` javascript
// for of loop 는 중도에 멈출 수도 있다 
const friends = ["Nico", "Lynn", "Japan Guy", "Autumn", "Dal", "Mark", "Pipe", "Theo"];

// Dal을 찾는 순간 멈추고 싶을 때
for(const friend of friends) {
	if(friend == "Dal") {
		break;
	} else {
		console.log(friend);
	}
}
```

---
## Classes
[MDN Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)  

### Class?
자바스크립트는 프로토타입 기반 객체지향 언어입니다. 다른 객체지향 언어들과 차이는 있지만, 강력한 객체지향 프로그래밍 능력을 지니고 있습니다.
프로토타입 기반 프로그래밍은 클래스가 필요 없는 객체지향 프로그래밍 스타일로 프로토타입 체인과 클로저 등으로 객체 지향 언어의 상속, 캡슐화 등의 개념을 구현 할 수 있습니다.

``` js
// ES5
var Renee = (function () {
	// Constructor
	function Renee(name) {
		this._name = name;
	}

	// Public method
	Renee.prototype.sayHi = function() {
		console.log('Hi ' + this._name);
	};
	
	// return constructor
	return Renee;
}());

var me = new Renee('eunhye');
me.sayHi();
```

### 인스턴스
object라는 이름의 객체의 새로운 인스턴스를 만들 때에는 new object로 사용하고, 차후에 접근할 수 있도록 변수에 결과를 받습니다.<br>
아래의 예제에서 Renee라는 이름의 클래스를 정의한 후에, 두개의 인스턴스를 생성하고 있습니다.

``` js
function Renee() {}

const eunhye() = new Renee();
const park() = new Renee();
```
### Constructor

#### 클래스 필드(class field)
클래스 내부의 캡슐화된 변수를 말한다. 데이터 멤버 또는 멤버 변수라고도 부른다. 클래스 필드는 인스턴스의 프로퍼티 또는 정적 프로퍼티가 될 수 있다.
쉽게 말해, 자바스크립트의 생성자 함수에서 this에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서는 클래스 필드라고 부른다.

``` js
class Foo {
	name = ''; // SyntaxError
	constructor() {}
}
```

constructor는 인스턴스를 생성하고 클래스 필드를 초기화하기 위한 특수한 메소드입니다.

``` js
// 클래스 선언문
class Renee {
	// constructor(생성자). 이름을 바꿀 수 없다.
	constructor(name) {
		// this는 클래스가 생성할 인스턴스를 가리킨다.
		// _name은 클래스 필드이다.
		this._name = name;
	}
}

// 인스턴스 생성
const me = new Renee ('park eunhye');
console.log(me); // Renee {_name: "park eunhye"}
```

Constructor는 클래스 내에 한 개만 존재 할 수 있으며 만약 2개 이상의 Constructor를 포함하면 문법 에러(SyntaxError)가 발생합니다.
인스턴스를 생성 할 때 new 연산자와 함께 호출한 것이 바로 Constructor이며 Constructor의 파라미터에 전달한 값은 클래스 필드에 할당합니다.
Constructor는 생략 가능하며 생략 시 클래스에 Constructor() {}를 포함한 것과 동일하게 동작합니다.
즉, 빈 객체를 생성합니다. 따라서 인스턴스에 프로퍼티를 추가하려면 인스턴스를 생성한 이후, 프로퍼티를 동적으로 추가해야 합니다.
Constructor는 인스턴스의 생성과 동시에 클래스 필드의 생성과 초기화를 실행합니다.
따라서 클래스 필드를 초기화해야 한다면 Constructor를 생략해서는 안됩니다.

### 클래스 상속 
클래스 상속(Class Inheritance)은 코드 재사용 관점에서 매우 유용합니다.<br>
새롭게 정의할 클래스가 기존에 있는 클래스와 매우 유사하다면, 상속을 통해 그대로 사용하되 다른 점만 구현하면 됩니다.<br>
코드 재사용은 개발 비용을 현저히 줄일 수 있는 잠재력이 있으므로 매우 중요하다.
- extends 키워드
- super 키워드
- static 메소드와 prototype 메소드의 상속

#### extends 키워드
``` javascript
class User {
	constructor(name, lastName, email, password) {
		this.username = name;
		this.lastName = lastName;
		this.email = email;
		this.password = password;
	}
	sayHello() {
		console.log(`Hello, my name is ${this.username}`);
	}
	getProfile() {
		console.log(`${this.username} ${this.lastName} ${this.email} ${this.password}`);
	}
	updatePassword(newPassword, currentPassword){
		if(currentPassword == this.password) {
			this.password = newPassword
		} else {
			console.log("Can't change password");
		}
	}
}
const miniUser = new User("eunhye", "park", "eunhye@mail.com", "1234"); 

// console.log(miniUser.password);
// miniUser.updatePassword("hello", "1234");
// console.log(miniUser.password);

// user 클래스에서 extends 해서 불러옴
// admin 클래스는 deletewebsite라는 function을 가지고 있다고 해보자
class Admin extends User {
	deleteWebsite() {
		console.log("Deleteing the whole website...");
	}
}

const miniAdmin = new Admin("eunhye", "park", "eunhye@mail.com", "1234");

miniAdmin.deleteWebsite();

console.log(miniAdmin.email);
```

#### super 키워드
``` javascript
class User {
	constructor({username, lastName, email, password}) {
		this.username = username;
		this.lastName = lastName;
		this.email = email;
		this.password = password;
	}
	getProfile() {
		console.log(`${this.username} ${this.lastName} ${this.email} ${this.password}`);
	}
	updatePassword(newPassword, currentPassword){
		if(currentPassword == this.password) {
			this.password = newPassword
		} else {
			console.log("Can't change password");
		}
	}
}
// 만약 여러 arguments를 가지고 있다면 object로 하는게 좋다, 왜냐하면 어떤 값을 넘겨주는지를 볼 수 있기 때문이다
const miniUser = new User({
	username:"eunhye", 
	lastName:"park", 
	email:"eunhye@mail.com", 
	password:"1234"
}); 

// super는 호출하게 된다 counstructor의 바로 원시 클래스, user를 가리킨다.
class Admin extends User {
	constructor({username, lastName, email, password, superAdmin, isActive}) {
		super({username, lastName, email, password});
		this.superAdmin = superAdmin;
		this.isActive = isActive;
	}
	deleteWebsite() {
		console.log("Deleteing the whole website...");
	}
}
const admin = new Admin({
	username:"eunhye", 
	lastName:"park", 
	email:"eunhye@mail.com", 
	password:"1234",
	superAdmin: true,
	isActive: true
});

// admin 인스턴스만 minuUser를 호출할 수 있다
// minuUser는 user의 인스턴스다
// admin 인스턴스는 admin인스턴스와 user인스턴스를 합친 것

```

---

## Promises
멀티 태스킹은 한 가지 이상의 것을 한 번에 동시에 생각하는게 아닙니다. 단지 어떤 사이를 빠르게 스위칭 하는 것을 의미합니다.
컴퓨터는 두 가지 일을 동시에 할 수 있습니다. 예를 들면 컴퓨터는 화면을 녹화하면서 오디오를 녹음 할 수 있고 wi-fi를 연결 할 수 있고, 시간이 바뀌는 것을 기다릴 수 있습니다.  

자바스크립트가 이와 같습니다.  

동시에 많은 일들을 할 수 있습니다. 그리고 이런 것의 실행을 막지 않습니다.

``` javascript
const hello = fetch("http://google.com");

console.log("something");
console.log(hello); 
// => something
// => fetch Error
```

이론적으로 fetch Error는 Something 전에 나와야 합니다. google.com을 fetch한 뒤 Error를 얻은 후 표시하고 something을 console에 노출하고
하지만 자바스크립트는 그렇지 않습니다. 프로그램 실행을 멈추지 않으며 순차적으로 처리하는 것이 아니라 한꺼번에 실행됩니다. 
이것이 바로 비동기성(Async)입니다.  

[MDN Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)  

Promise는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냅니다.
Promise를 만들 때는 실행 할 수 있는 function을 넣어야합니다.

```js
const promisel = new Promise((resolve, reject) => {
  setTimeout(resolve, 3000, "Yes you are");
});

console.log(promisel);

setInterval(console.log, 1000, promisel);
// Promise {<pending>}
// Promise {<pending>}
// Promise {<pending>}
// Promise {<fulfilled>: "Yes you are"}
// Promise {<fulfilled>: "Yes you are"}
// Promise {<fulfilled>: "Yes you are"}
// Promise {<fulfilled>: "Yes you are"}
// ...
```

promise를 이용하여 무언가를 불러올 때는 then을 사용해봅니다.  

[MDN Promise.prototype.then()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)  

```javascript
const promisel = new Promise((resolve, reject) => {
	setTimeout(resolve, 3000, "Yes you are")
});

promisel.then(value => console.log(value));
```
then은 넣고 싶은 만큼 넣어서 chaning 해주면 됩니다.

``` javascript
const promisel = new Promise((resolve, reject) => {
	resolve(2)
});

promisel
	.then(number => {
		console.log(number * 2);
		return number * 2
	})
	.then(otherNumber => {
		console.log(otherNumber * 2);
	});
```

```javascript
const promisel = new Promise((resolve, reject) => {
	resolve(2)
});

const timesTwo = ( number ) => number * 2;

promisel
	.then(timesTwo)
	.then(timesTwo)
	.then(timesTwo)
	.then(timesTwo)
	.then(timesTwo)
	.then(() => {
		throw Error("Something");
	})
	.then(lastNumber => console.log(lastNumber))
	.catch(error => console.log(error));
```

promise에 에러가 생기면 catch를 이용하여 error를 호출 할 수 있습니다.

[MDN Promise.prototype.catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)  

```javascript
const promisel = new Promise((resolve, reject) => {
	setTimeout(reject, 3000, "You are ugly")
});

promisel
	.then(value => console.log(value))
	.catch(value => console.log(value));
```

### Promise.all

### Promise.race

### .finally


--- 

## Async, Await

### try catch finally

### Parallel Async Await







---
##### 참고사이트  
[Nomad ECMAScript2015](https://nomadcoders.co/es6-once-and-for-all)  

**ECMAScript2015 기본지식**  
[MDN ECMAScript2015](https://developer.mozilla.org/ko/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)  
[Poiemaweb ECMAScript2015](https://poiemaweb.com/es6-block-scope)  
[우아한형제들 기술블로그](https://woowabros.github.io/experience/2017/12/01/es6-experience.html)  

**variables**  
[MDN Let](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)  
[MDN Const](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)  
[MDN block](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/block)  

**Functions**  
[MDN Arrow Function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98)  

**String**  
[MDN DocumentFragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)  
[MDN Template literals](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals)

**Array**  
[MDN Array.of()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/of)  
[MDN Array.from()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/from)  
[MDN Array.find()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)  
[MDN Array.findIndex()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)  
[MDN Array.fill()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)  

**Destructuring**  
[MDN 객체 구조 분해](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EA%B0%9D%EC%B2%B4_%EA%B5%AC%EC%A1%B0_%EB%B6%84%ED%95%B4)  
[MDN 배열 구조 분해](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EB%B0%B0%EC%97%B4_%EA%B5%AC%EC%A1%B0_%EB%B6%84%ED%95%B4)  

**Classes**  
[MDN Classes](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes)  
[MDN super](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/super)  

**Promises**  
[MDN Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)  
[MDN Promise.prototype.then()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)  
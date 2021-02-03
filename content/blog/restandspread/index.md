---
title: Rest Parameter, Spread Operator
date: "2021-02-03T18:00:00.284Z"
description: Rest 파라미터와 Spread 연산자
---

이 글은 Rest parameter, Spread Operator MDN, Poiema를 참고하여 정리한 글입니다. 

먼저 인수, 인자, 매개변수 뜻을 살펴보겠습니다. 

**매개변수(Parameter) = 인자 = 가인수**  
함수의 정의에서 전달받은 인수를 함수 내부로 전달하기 위해 사용하는 변수를 의미합니다.

**인수(argument)**  
함수가 호출될 때 함수로 값을 전달해주는 값을 말합니다.

```jsx
function func(x, y, z) { // x, y, z라는 3개의 매개변수
	return x+y+z
}

func(1,2,3) // 인수 1, 2, 3을 전달하여 함수 호출
```
---

## Rest Parameter
Rest 파라미터는 구문은 **정해지지 않은 인수**를 배열로 나타낼 수 있게 합니다.
함수의 마지막 파라미터 앞에 `...`을 붙여 모든 나머지 인수를 자바스크립트 배열로 대체합니다.
마지막 파라미터만 Rest파라미터가 될 수 있습니다.

```jsx
function func(a, b, ...rest) {
	console.log(a)
	console.log(b)
	console.log(rest)
}

func("one", "two", "three", "four", "five", "six")

// one
// two
// [three, four, five, six]
```

**Rest 파라미터 및 arguments 객체간 차이**  

1. Rest 파라미터는 구분된 이름이 주어지지 않은 유일한 대상인 반면, arguments 객체는 함수로 전달된 모든 인수를 포함합니다.
2. arguments 객체는 실제 배열이 아니고 rest 파라미터는 Array 인스턴스로, sort, map, forEach 또는 pop같은 메서드가 바로 인스턴스에 적용될 수 있음을 뜻합니다.
3. 즉 arguments 객체는 자체에 특정 추가 기능이 있습니다.

ES5에서는 ```arguments``` 객체를 활용하여 인수를 전달받는다.  
```arguments``` 객체는 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회가능한(iterable) 유사 배열 객체(array-like object)이며 함수 내부에서 지역 변수처럼 사용할 수 있다.
```arguments``` 프로퍼티는 현재 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 deprecated되었다. 
```Function.arguments```와 같은 사용 방법은 권장되지 않으며 함수 내부에서 지역변수처럼 사용 할 수 있는 ```arguments``` 객체를 참조하도록 한다.
```arguments``` 객체는 유사 배열 객체이므로 배열 메소드를 사용하려면 ```Function.prototype.call```을 사용해야 하는 번거로움이 있다.

```jsx
function func(a, b) {
	var normalArray = Array.prototype.slice.call(arguments);
	var normalArray = [].prototype.slice.call(arguments);
	var normalArray = Array.from(arguments);
}
```

ES6에서는 rest파라미터를 사용하여 가변 인자의 목록을 배열로 전달받을 수 있다.  
이를 통해 유사 배열인 arguments 객체를 배열로 변환하는 번거로움을 피할 수 있다.

```jsx
function func(...rest) {
	console.log(arguments)

	console.log(Array.isArray(rest)) // true
}
```

ES6의 화살표 함수에는 함수 객체의 arguments 프로퍼티가 없다.  
따라서 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 rest파라미터를 사용해야 한다. 

```jsx
var nomalFunc = function () {}
console.log(normalFunc.hasOwnProperty('arguments')) // true

const arrowFunc = () => {};
console.log(arrwFunc.hasOwnProperty('arguments')) // false
```

---

## Spread 문법 (전개구문)
spread문법은 대상을 개별 요소로 분리한다. 

```jsx
// 함수 호출
myFunction(...iterableObj)

// 배열 리터럴과 문자열
[...iterableObj, '4', 'five', 6]

// 객체 리터럴
let objClone = { ...obj }
```

### 함수의 인수로 사용하는 경우
`apply()` 대체  
배열을 분해하여 배열의 각 요소를 파라미터에 전달하고 싶은 경우, Function.prototype.apply를 사용하는 것이 일반적이다.

```jsx
// apply 사용
function func(x, y, z) {}
var args = [0, 1, 2]
func.apply(null, args)

// spread 사용
function func(x, y, z) {}
var args = [0, 1, 2]
func(...args)
```
Rest 파라미터는 반드시 마지막 파라미터이어야 하지만 Spread 문법을 사용한 인수는 자유롭게 사용할 수 있다.

### 배열에서 사용하는 경우

기존에 두 배열을 합치려면, push, concat, splice를 사용해야 했다.  
하지만spread 문법을 배열에서 사용하면 보다 간결하고 가독성 좋게 표현할 수 있다.

**배열 복사, push()**

```jsx
var arr1 = [1, 2, 3]
var arr2 = [4, 5, 6]

// ES5
Array.prototype.push.apply(arr1, arr2) // apply 메소드의 2번째 인자는 배열. 이것은 개별 인자로 push 메소드에 전달된다.
console.log(arr1)

// ES6
arr1.push(...arr2)
```

**배열 연결, concat()**

```jsx
var arr1 = [1, 2, 3]
var arr2 = [4, 5, 6]

// ES5
arr1 = arr1.concat(arr2)

// ES6
arr1 = [...arr1, ...arr2]
```

**splice() ⇒ Poiema 참고**
```jsx
// ES5
var arr1 = [1, 2, 3, 6];
var arr2 = [4, 5];

/*
apply 메소드의 2번째 인자는 배열. 이것은 개별 인자로 splice 메소드에 전달된다.
[3, 0].concat(arr2) → [3, 0, 4, 5]
arr1.splice(3, 0, 4, 5) → arr1[3]부터 0개의 요소를 제거하고 그자리(arr1[3])에 새로운 요소(4, 5)를 추가한다.
*/
Array.prototype.splice.apply(arr1, [3, 0].concat(arr2));

console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]

// ES6
const arr1 = [1, 2, 3, 6];
const arr2 = [4, 5];

// ...arr2는 [4, 5]을 개별 요소로 분리한다
arr1.splice(3, 0, ...arr2); // == arr1.splice(3, 0, 4, 5);

console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]
```

---

## Reference URL

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/rest_parameters)

[https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

[https://poiemaweb.com/es6-extended-parameter-handling](https://poiemaweb.com/es6-extended-parameter-handling)

[https://medium.com/@shlee1353/자바스크립트-spread와-rest-문법정리-f42efa73d3db](https://medium.com/@shlee1353/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-spread%EC%99%80-rest-%EB%AC%B8%EB%B2%95%EC%A0%95%EB%A6%AC-f42efa73d3db)
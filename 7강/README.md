# 스코프
- 스코프는 변수와 상수, 매개변수가 언제 어디서 정의되는지 결정
```javascript
function f(x) {
  return x + 3;
}
f(5); // 8
x;    // ReferenceError: x is not defined
```
- x가 함수의 바디를 벗어나면 x는 없어진다. x의 스코프가 함수 f라고 말한다.
> 일부 언어는 선언, 정의를 구분한다. 선언은 존재를 알리고, 정의는 값을 부여하는 것인데, 자바스크립트는 선언 동시에 값이 주어지므로 (우리가 안 주면 undefined로 주어짐) 이 둘을 구분하지 않는다.
## 스코프의 존재
- 스코프와 존재의 차이
  - 스코프 : 프로그램의 현재 실행 중인 부분, 즉, 실행 컨텍스트에서 현재 보이고 접근할 수 있는 식별자를 말함
  - 존재 : 식별자가 메모리가 할당된 무언가를 가리키고 있다는 뜻
## 정적 스코프와 동적 스코프
```javascript
function f1() {
  console.log('one');
}

function f2() {
  console.log('two');
}

f2();
f1();
f2();
```
- f1이 f2보다 먼저 정의됐지만, f2 => f1 => f2 순으로 실행된다.
- 자바스크립트의 스코프는 정적이다. 즉, 소스 코드만 봐도 변수가 스코프에 있는지 판단이 가능하다. 단, 함 수 스코프 안에 어떤 변수가 있는지 함수를 정의할 때 알 수 있다는 뜻이지, 호출할 때 알 수 있는건 아니다.
```javascript
const x = 3;

function f() {
  console.log(x);
  console.log(y);
}

{ // 새 스코프
  const y = 5;
  f();
}
```
- 변수 x는 함수 f를 정의할 때 존재히지만, y는 그렇지 않는다. y는 다른 스코프에 존재한다. 다른 스코프에서 y를 선언하고 그 스코프에서 f를 호출하더라도, f를 호출하면 x는 그 바디안의 스코프에 있지만 y는 그렇지 않다. => 이것이 정적 스코프다.
- 즉, 함수는 정의 될 때 접근할 수 있었던 식별자에는 여전히 접근할 수 있지만, 호출할 때 스코프에 있는 식별자에 접근할 수 없다.
- 자바스크립트의 정적 스코프는 전역 스코프와 블록 스코프, 함수 스코프에 적용된다.
## 전역 스코프
- 스코프는 계층적이며 트리의 맨 아래에는 바탕이 되는 무언가가 있어야 한다. 즉, 프로그램을 시작할 때 암시적으로 주어지는 스코프가 필요하다. 이 스코프를 *전역 스코프*라고 한다.
- 자바스크립트 프로그램을 시작할 때, 어떤 함수도 호출하지 않았을 때, 실행 흐름은 전역 스코프에 있다.
- 전역 스코프에 선언한 것은 무엇이든 프로그램의 모든 스코프에서 볼 수 있다. (*전역 변수*와 비슷)
```javascript
let name = "Irena"; // 전역
let age = 25; // 전역

function greet() {
  console.log(`Hello, ${name}!`);
}
function getBirthYear() {
  return new Date().getFullYear() - age;
}
```
- 위 함수의 문제는 스코프에 대단히 의존 적이라는 것이다. 어떤 함수든, 프로그램 어디서든 상관없이 name 값을 (실수로, 의도적으로) 바꿀 수 있따.
- 또한, name과 age는 흔한 변수명이라서 다른 곳에서 사용될 가능성이 크다.
- 그래서 객체에 보관하는게 좋다.
```javascript
let user = {
  name = "Irena",
  age = 25,
}
function greet() {
  console.log(`Hello, ${user.name}!`);
}
function getBirthYear() {
  return new Date().getFullYear() - user.age;
}
```
- 만약 사용자가 더 많아지면 어떻게 될까? 전역 스코프에 의존되지 않도록 해보자
```javascript
function greet(user) {
  console.log(`Hello, ${user.name}!`);
}
function getBirthYear(user) {
  return new Date().getFullYear() - user.age;
}
```
- 위 함수는 모든 스코프에서 호출할 수 있고, 명시적으로 user를 전달받는다.
- 프로그램이 복잡해질 수록 전역 스코프에 의존하지 않도록 하자.
## 블록 스코프
- let과 const는 식별자를 블록 스코프에서 선언한다. (블록은 중괄호로 묶는 것)
- 블록 스코프는 그 블록의 스코프에서만 보이는 식별자를 의미한다.
```javascript
console.log('before block');
{
  console.log('inside block');
  const x = 3;
  console.log(x) // 3
}
console.log(`outside block; x = ${x}`) // Reference Error
```
- x가 블록 안에서 정의 됐으니 나가는 순간 정의되지 않은 것으로 간주된다.
- var로 선언했다면 x는 밖에서도 부를 수 있다.
## 변수 숨기기
```javascript
{
  // block 1
  const x = 'blue';
  console.log(x);
}
console.log(typeof x); // "undefined";
{
  // block 2
  const x = 3;
  console.log(x); // "3"
}
console.log(typeof x); // "undefined";
// 위 두 변수는 다른 스코프에 있는 이름만 같은 두 개의 변수이다.
```
- 스코프가 중첩되는 경우를 살펴보자
```javascript
{
  // 외부 블록
  let x = 'blue';
  console.log(x); // "blue"
  {
    // 내부 블록
    let x = 3;
    console.log(x); // "3"
  }
  console.log(x); // "blue"
}
console.log(x); // Reference Error
```
- 변수 숨김을 잘 보여주는 예제, 내부 블록의 x는 외부 블록에서 정의한 x와는 이름만 같을 뿐 다른 변수이므로 외부 스코프의 x를 숨기는(가리는) 효과가 있다.
- 내부 블록에 들어가 새 변수 x를 정의하는 순간, 두 변수가 모두 스코프 안에 있다는 것이 중요하다. 변수의 이름이 같아서 외부 스코프에 있는 변수에 접근할 방법이 없다.
```javascript
{
  // 외부 블록
  let x = { color: "blue" };
  let y = x // y와 x는 같은 객체를 가리킨다.
  let z =  3;
  {
    // 내부 블록
    let x = 5; // 이제 바깥의 x는 가려진다.
    console.log(x); // 5
    console.log(y.color); // "blue"
    y.color = red;
    console.log(z); // 3
  }
  console.log(x.color); // red
  console.log(y.color); // red
  console.log(z); // 3
}
```
- 이처럼 변수를 숨기면 해당 이름으로는 절대 접근 할 수 없다.
## 함수, 클로저, 정적 스코프
- 함수가 특정 스코프에 접근할 수 있도록 의도적으로 그 스코프에서 정의하는 경우가 많다.
- 이런 것을 보통 클로저라고 부른다. 스코프를 함수 주변으로 좁히는 것이라고 생각해도 된다.
```javascript
let globalFunc; // 정의되지 않은 전역 함수
{
  let blockVar = 'a';
  globalFunc = function() {
    console.log(blockVar);
  }
}
globalFunc(); // 'a'
```
- globalFunc는 블록 안에서 값을 할당받았다. 이 블록 스코프와 그 부모인 전역 스코프가 클로저를 형성한다. 분명 함수는 스코프에서 빠져나왔음에도 불구하고 blockVar에 접근할 수 있다.
- 일반적으로는 스코프에서 빠져나가면 해당 스코프에서 선언한 변수는 메모리에서 제거해야 안전하다. 즉, 스코프 안에서 함수를 정의하면 해당 스코프는 더 오래 유지된다.
```javascript
let f; // 정의되지 않은 함수
{
  let o = { note: 'Safe' };
  f = function() {
    return o;
  }
}
let oRef = f();
oRef.note = "Not so sfae after all!";
```
- 일반적으로 자신의 스코프에 없는 것들에는 접근할 수 없지만, 함수를 정의해 클로저를 만들면 접근할 수 없었던 것들에 접근할 방법이 생긴다.
## 즉시 호출하는 함수 표현식
```javascript
(function() {
  // IIFE 바디
})();
```
- 함수 표현식으로 익명 함수를 만들고 그 함수를 즉시 호출한다.
- IIFE(즉시 호출하는 함수 표현식)의 장점은 내부에 있는 것들이 모두 자신만의 스코프를 가지지만, IIFE 자체는 함수이므로 그 스코프 밖으로 무언가를 내보낼 수 있다는 것이다.
```javascript
const message = (function() {
  const secret = "I'm a secret";
  return `The secret is ${secret.length} characters long`;
})();
console.log(message);
```
- 변수 secret은 IIFE의 스코프 안에서 안전하게 보호되며 외부에서 접근할 수 없다. IIFE는 함수이므로 무엇이든 반환할 수 있다.
```javascript
const f = (function() {
  let count = 0;
  return function() {
    return `I have been called ${++count} time(s).`;
  }
})();
f(); // ... 1 ...
f(); // ... 2 ...
```
- 이 처럼 변수 count는 IIFE 안에서 안전하게 보관되어 손댈 방법이 없으며, f는 자신이 몇 번 호출됐는지 항상 정확히 알고 있다.
- ES6에서 블록 스코프 변수를 도입하면서 IIFE가 필요한 경우가 줄었지만, 여전히 매우 쓰인다.
- 클로저를 만들고 클로저에서 무언가 반환받을 때에는 유용하게 쓸 수 있다.
## 함수 스코프와 호이스팅
- ES6에서 let 도입 전엔 var를 썼었고, 이렇게 선언된 변수들은 함수 스코프라 불리는 스코프를 가졌다. ( var로 선언한 전역 변수는 명시적인 함수 안에 있지는 않지만 함수 스코프와 똑같이 동작한다.)
- let은 선언하면, 선언하기 전에는 존재하지 않는다. var는 언제 어디서든 사용할 수 있으며, 심지어 선언 전에도 사용할 수 있다.
```javascript
let var1;
let var2 = undefined;
var1; // undefined
var2; // undefined
undefinedVar; // RefenceError

x; // ReferenceError
let x = 3;

x; // undefined
var x = 3;
x; // 3
```
- 변수를 선언하지도 않았는데 그 변수에 접근할 수 있다. var로 선언한 변수는 끌어올린다는 뜻의 *호이스팅*이라는 메커니즘을 따른다.
- 함수나 전역 스코프 전체를 살펴보고 var로 선언한 변수를 맨 위로 끌어올린다.
```javascript
// 이 전 예제가 이렇게 해석된다.
var x;
x; // undefined
x = 3;
x; // 3
```

```javascript
if (x !== 3) {
  console.log(y);
  var y = 5;
  if (y === 5) {
    var x = 3;
  }
  console.log(y);
}
if (x === 3) {
  console.log(y)
}
// 위 식을 아래와 같이 해석한다.
var x;
var y;
if (x !== 3) {
  console.log(y);
  y = 5;
  if (y === 5) {
    var x = 3;
  }
  console.log(y);
}
if (x === 3) {
  console.log(y)
}
```
- var를 이용해 변수를 선언하면 자바스크립트는 같은 변수를 여러 번 정의하더라도 무시한다. 즉, 변수 숨김이 불가능하다. 그래서 let이 만들어 졌다. var에는 let보다 나은 점이 없다. var 쓰지마셈
- 그래도 var와 호이스팅이 필요한 이유는 ES6가 보편적이지 않고, 함수 선언 역시 끌어올려지기 때문에 이 메커니즘을 이해해야한다.
## 함수 호이스팅
- var와 마찬가지로, 함수 선언도 스코프 맨 위로 끌어올려진다. 단, 변수에 할당한 함수 표현식은 끌어올려지지 않는다.
```javascript
f();
function f() {
  console.log('f');
}
```
```javascript
f(); // Reference Error
let f = function() {
  console.log('f');
}
```
## 사각지대
- let으로 선언하는 변수는 선언하기 전까지 존재하지 않는다는 직관적인 개념이인 사각지대.
## 스트릭트 모드
- ES5 문법에서는 암시적 전역 변수라는 것이 생길 수 있다. var로 선언하는 것을 잊으면, 자바스크립트는 전역 변수를 참조하려 한다고 간주하고, 전역 변수가 존재하지 않으면 스스로 만든다.
- 그러한 이유로 스트릭트 모드가 생겼다. 'use strict'를 사용하면 된다. 스트릭트 모드에서는 암시적 전역 변수를 허용하지 않ㄴ든다.
- 전역 스코프에서 'use strict'를 사용하면 스크립트 전체가 스트릭트 모드로 실행되고, 함수안에서 사용하면 해당 함수만 스트릭트 모드로 실행된다.
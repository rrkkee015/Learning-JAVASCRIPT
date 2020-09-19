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
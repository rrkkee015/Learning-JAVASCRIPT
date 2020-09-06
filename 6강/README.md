# 함수
- 함수 값을 반환하지 않으면 `undefined`가 반환된다.
## 호출과 참조
- 자바스크립트에서 함수도 객체.
```javascript
getGreeting(); // "Hello World"
getGreeting // function getGreeting()

// 참조를 가능하게 만들었기 때문에 아래도 가능
const f = getGreeting;
f(); // "Hello World"

const o = {};
o.f = getGreeting;
o.f() // "Hello World"

const arr = [1, 2, 3];
arr[1] = getGreeting;
arr[1](); // "Hello World!"
```
- 자바스크립트에서 값 뒤에 괄호를 붙이면 자바스크립트는 그 값을 함수로 간주하고 호출한다. 함수가 아닌 값 뒤에 괄호를 붙이면 에러가 발생한다.
## 함수와 매개변수
```javascript
function f(x) {
  console.log(x); // 3
  x = 5;
  console.log(x) // 5
}

let x = 3;
f(x)
console.log(x) // 3

// 객체의 경우엔 값이 변경된다.
function f(o) {
  o.message = "Hi"
}

let o = {
  message: "초기 값"
};
console.log(o.message) // "초기 값"
f(o);
console.log(o.message) // "Hi"
```
- 위의 예제가 원시 값과 객체의 핵심적인 차이, 원시 값은 불변이므로 수정을 할 수 없고, 원시 값을 담은 변수는 수정할 수 있지만, 원시 값 자체는 바뀌지 않는다. 반면 객체는 바뀔 수 있다.
- 함수 밖, 함수 안 서로 다른 개체이지만 같은 객체를 가리키고 있기에 가능하다.
```javascript
function f(o) {
  o.m = "2"
  o = {
    m : "3"
  }
  console.log(o.m) // "3"
}

o = {
  m : "1"
};
console.log(o.m) // "1"
f(o);
console.log(o.m) // "2"
```
## 매개변수가 함수를 결정하는가?
```javascript
function f(x) {
  console.log(x); "undefined"
}
f(); // 에러가 뜨지 않는다.

// 객체 해체 가능
function f({s, v, o}) {
  console.log(s, v, o) // 1, 2, 3
};

a = {
  s : 1,
  v : 2,
  o : 3
};
f(a);

// 배열 해체 가능
function f([ s, v, o ]) {
  console.log(s, v, o);
}

a = [1,2,3];
f(a);

function f(p, ...w) {
  console.log(p); // "c"
  console.log(w); // ["v", "b"]
}

f("c", "v", "b");
```
## 매개변수 기본 값
- ES6 이후로 기본값을 지정하는 기능도 생겼다.
```javascript
function f(a, b = "default", c) {
  console.log(a); // 5
  console.log(b); // default
  console.log(c); // undefined
}

f(5);
```
## 객체의 프로퍼티인 함수
```javascript
const o = {
  name: 'W',
  bark: function() {return : 'hi';}
}

// ES6로 더욱 간편하게 만들 수 있따.
const o = {
  name: 'W',
  bark() {return 'hi';}
}
```
## this 키워드
- 일반적으로 this는 객체의 프로퍼티인 함수에서 의미가 있다. 메서드를 호출하면 this는 호출한 메서드를 소유하는 객체가 된다.
```javascript
// 메서드 안에 보조함수가 존재
const o = {
  name: 'Julie',
  greetBackwards: function() {
    function getReverseName() {
      let nameBackwards = '';
      for (let i = this.name.length-1; i >=0; i--) {
        nameBackwards += this.name[i];
      }
      return nameBackwards
    }
    return `${getReverseName()} si eman ym. olleH`;
  }
};
o.greetBackwards();

// 이름을 거꾸로 쓰고자 중첩된 함수 getReverseName을 사용함.
// o.greetBackwards()를 후출하는 시점에서 this를 의도한 대로 o에 연결하지만, greetBackwards 안에서 getReverseName을 호출하면 this는 o가 아닌 다른 것에 묶인다.

// 이를 해결하기 위해서 다른 변수에 this를 할당하는 것이다.
const o = {
  name: 'Julie',
  greetBackwards: function() {
    const self = this;
    function getReverseName() {
      let nameBackwards = '';
      for (let i = self.name.length-1; i >=0; i--) {
        nameBackwards += self.name[i];
      }
      return nameBackwards
    }
    return `${getReverseName()} si eman ym. olleH`;
  }
};
o.greetBackwards();
```
## 함수 표현식과 익명 함수
- 함수를 선언하면 함수에 바디와 식별자가 모두 주어진다. 자바스크립트는 익명 함수도 지원하며, 익명 함수에서는 함수에 식별자가 주어지지 않는다.
- 함수 표현식을 이용해서 함수를 호출하면 된다.
```javascript
const f = function() {
  // ...
}
// f가 함수를 가리키기 때문에 f()로 함수를 호출할 수 있따. 차이점은 먼저 함수 표현식으로 익명 함수를 만들고 그 함수를 변수에 할당했다는 것.

const g = function f() {
  // ...
}
// 이런식으로하면 이름 g에 우선순위가 있다. 함수 바깥에서 함수에 접근할 때 g를 써야 하며, f로 접근하려 하면 변수가 정의되지 않았다는 에러가 뜬다.
// 보통 재귀로 자기 자신을 부를 때 이런 방식이 필요할 수 있다.


const g = function f(stop) {
  if(stop) console.log('f stopped');
  f(true);
};
g(false);
// 함수 안에서는 f를 써서 자기 자신을 호출하고 밖에서는 g를 써서 함수를 호출한다.
```
- 함수 선언과 함수 표현식이 같다면 자바스크립트는 둘을 어덯게 구분할까? 답은 **컨텍스트**이다.
- 함수 선언이 표현식으로 사용됐다면 그건 함수 표현식이고, 표현식으로 사용되지 않았다면 함수 선언이다.
- 복잡하게 생각할 것 없이 나중에 호출할 생각으로 함수를 만든다면 함수 선언을 사용하고, 다른 곳에 할당하거나 다른 함수에 넘길 목적으로 함수를 만든다면 함수 표현식을 사용하면 된다.
## 화살표 표기법
- ES6에서 새로 만든 표기법이다. 간단히 말해 function이라는 단어와 중괄호 숫자를 줄이려고 만든 단축 문법이다.
  1. function을 생략해도 된다.
  2. 함수에 매개변수가 단 하나 뿐이라면 괄호도 생략할 수 있다.
  3. 함수 바디가 표현식 하나라면 중괄호와 return 문도 생략할 수 있다.
- 화살표 함수는 항상 익명이다. 이름 붙은 함수를 만들 수 없고, 변수에 할당할 수 있다.
```javascript
const f1 = function() { return "hello"; }
const f1 = () => "hello";

const f2 = function(name) { return `Hello, ${name}`; }
const f2 = name => `Hello, ${name}`;

const f3 = function(a, b) { return a + b; }
const f3 = (a, b) => a + b;
```
- 화살표 함수에는 일반적인 함수와 중요한 차이가 있는데, this가 다른 변수와 마찬가지로 정적으로 묶인다는 것이다.
```javascript
const o = {
  name: 'Julie',
  greetBackwards: function() {
    const getReverseName = () => {
      let nameBackwards = '';
      for(let i = this.name.length-1; i>=0; i--) {
        nameBackwards += this.name[i];
      }
      return nameBackwards;
    };
    return `${getReverseName()} si eman ym, olleH`;
  },
};

o.greetBackwards();
```
- 그 외에도 화살표 함수는 객체 생성자로 사용할 수 없고, arguments 변수도 사용할 수 없다. ES6로 넘어오고 나서 확산 연산자가 생겼으니 arguments 변수는 필요가 없긴 하다.
## call과 apply, bind
- call 메서드는 모든 함수에서 사용할 수 있으며, this를 특정 값으로 지정할 수 있다.
```javascript
const bruce = { name: "Bruce" };
const madeline = { name: "Madeline" };

function greet() {
  return `Hello, I'm ${this.name}`; // 이 함수는 어떤 객체에도 연결되지 않았지만 this를 사용한다.
}

greet(); // "Hello, I'm undefined"
greet.call(bruce) // "Hello, I'm Bruce"
greet.call(madeline) // "Hello, I'm Madeline"
// 함수를 호출하면서 call을 사용하고 this로 사용할 객체를 넘기면 해당 함수가 주어진 객체의 메서드인 것처럼 사용할 수 있다.

function update(birthYear, occupation) {
  this.birthYear = birthYear;
  this.occupation = occupation;
}

update.call(bruce, 1949, 'singer');
// bruce는 이제 { name: "Bruce", birthYear: 1949, occupation: "singer" }
update.call(madeline, 1942, 'actress');
// madeline은 이제 { name: "Madeline", birthYear: 1942, occupation: "actress" }
```
- apply는 함수 매개변수를 처리하는 방법만 제외하면 call과 완전 같다. call은 매개변수를 직접 받지만, apply는 매개변수를 배열로 받는다.
```javascript
update.apply(bruce, [1955, "actor"]);
// bruce는 이제 { name: "Bruce", birthYear: 1955, occupation: "actor" }
update.apply(madeline, [1918, "writer"]);
// madeline은 이제 { name: "Madeline", birthYear: 1918, occupation: "writer" }
```
- apply의 경우 배열 요소를 함수 매개변수로 사용해야 할 때 유용하다.
```javascript
const arr = [2, 3, -5, 15, 7];
Math.min.apply(null, arr); // -5
Math.max.apply(null, arr); // 15
// Math.min, Math.max는 매개변수를 받아 그 중 최솟값과 최댓값을 반환한다. apply를 사용하면 기존 배열을 이들 함수에 바로 넘길 수 있따.

// this의 값에 null을 쓴 이유는 this와 관계 없이 동작하기 때문이다.

// ES6의 확산 연산자(...)를 사용해도 apply와 같은 결과를 얻을 수 있다. update 메서드는 this 값이 중요하므로 call을 사용해야 하지만, Math.min과 Math.max는 this 값이 무엇이든 관계없으므로 확산 연산자를 그대로 사용할 수 있다.
const newBruce = [1940, "martial artist"];
update.call(bruce, ...newBruce); // apply(bruce, newBruce)와 같다.

Math.min(...arr); // -5
Math.max(...arr); // 15
```
- this의 값을 바꿀 수 있는 마지막 함수는 bind이다. bind를 사용하면 함수의 this 값을 영구히 바꿀 수 있다. update 메서드를 이리저리 옮기면서 호출할 때 this 값은 항상 bruce가 되게끔, call이나 apply, 다른 bind와 함께 호출하더라도 this 값이 bruce가 되도록 하려면 bind를 사용한다.
```javascript
const updateBruce = update.bind(bruce);

updateBruce(1904, "actor");
// burce는 { name: "Bruce", birthYear: 1904, occupation: "actor" } 이다.
updateBruce.call(madeline, 1274, "king");
// madeline은 변하지 않는다.
// bruce는 { name: "Bruce", birthYear:  1274, occupation: "king" } 이다.

// bind는 함수의 동작을 영구적으로 바꾸므로 버그의 원인이 될 수 있다.
// bind를 사용한 함수는 call이나 apply, 다른 bind와 함께 사용할 수 없는거나 마찬가지다. 함수를 여기저기서 call이나 apply로 호출해야 하는데, this 값이 그에 맞춰 바뀌어야 하는 경우를 상상해보자
// 위와 같은 경우에서 bind를 사용하면 망한다.

// bind에 매개변수를 넘기면 항상 그 매개변수를 받으면서 호출되는 새 함수를 만드는 효과가 있다.
const updateBruce1949 = update.bind(bruce, 1949);

updateBruce1949("singer");
// bruce는 { name: "Bruce", BrithYear: 1949, occupation: "singer" }이다.
// 즉. bind를 통해 burce가 태어난 해 1949가 고정되고, 직업은 자유롭게 바꿀 수 있는 함수가 하나 더 생긴거다.
```

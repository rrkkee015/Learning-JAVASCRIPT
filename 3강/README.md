# 리터럴과 변수, 상수, 데이터 타입
## 변수와 상수
- let / const의 경우 ES6에서 처음 생겼다. 이 전에는 var만 사용할 수 있었다.
- 될 수 있으면 변수보다 상수를 사용하는 것이 좋다.
- 변수와 상수, 함수의 이름을 식별자(identifier)라고 부른다. JQuery 때문에 $ 표시도 식별자다.
## 리터럴
- 리터럴이라는 단어는 값을 프로그램 안에서 직접 지정한다는 의미이다. 리터럴은 값을 만드는 방법이다. 자바스크립트는 우리가 제공한 리터럴 값을 받아 데이터를 만든다.
- 리터럴과 식별자의 차이를 이해해야한다.
```javascript
let room = "conference_room_a"; // "conference_room_a" (따옴표 안)이 리터럴, room은 식별자
```
## 원시 타입과 객체
- 자바스크립트의 값은 원시 값 또는 객체이다. 문자열과 숫자 같은 원시 타입은 불변이다. 숫자 5가 5이며 문자열 "alpha"가 "alpha"인 것과 같다.
- 원시 타입의 종류
  1. 숫자
  2. 문자열
  3. 불리언
  4. null
  5. undefined
  6. 심볼
- 원시 타입 외에 객체가 있다. 객체는 여러 가지 형태와 값을 가질 수 있다.
- 자바스크립트에 내장된 객체 타입
  1. Array
  2. Date
  3. RegExp
  4. Map과 WeakMap
  5. Set과 WeakSet
  6. Number
  7. String
  8. Boolean
## 숫자
- JAVAScript의 경우 10진수, 16진수, 지수 등 어떤 리터럴 형식을 사용하더라도 숫자는 더블 형식으로 저장된다.
- "NaN"의 경우 숫자가 안디ㅏ.
## 문자열
- 유니코드 텍스트이다.
- 문자열 리터럴에는 작은 따옴표, 큰따옴표 백틱을 사용하는데, 백틱의 경우 ES6에 도입된 것이며, 템플릿 문자열에서 사용된다.
## 템플릿 문자열
- 값을 문자열 안에 써야할 때 사용한다.
```javascript
let currentTemp = 19.5;
const message = "The current temperature is " + currentTemp + "\u00b0C"; // 이걸 문자열 병합이라하는데, 이러면 귀찮으니 문자열 템플릿이 생겼다.
const messaage2 = `The current temperature is ${currentTemp}\u00b0C`;
```
- 문자열 템플릿 안에서는 달러가 특수문자가 되기 때문에 \를 사용해야한다.
## 여러줄의 문자열
```javascript
const multiline = "line1\n\
line2"; // ES6 이전
const multiline = `line1
line2`; // ES6 이후
```
## 숫자와 문자열
```javascript
const result1 = 3 + '30'; // '330'
const result2 = 3 * '30'; // '90'
```
## 불리언
- true / false만 쓴다.
## 심볼
- 심볼은 유일한 토큰을 나타내기 위해 ES6에서 도입한 새 데이터 타입이다. 심볼은 항상 유일하며 다른 어떤 심볼과도 일치하지 않는다. 이런 면에서 심볼은 객체와 유사하다. (객체 또한 모두 유일하다)
- 항상 유일하다는 점을 제외하면 심볼은 원시 값의 특징을 모두 가지고 있어서 확장성 있는 코드를 만들 수 있다.
```javascript
const RED = Symbol("The color of a sunset!");
const ORANGE = Symbol("The color of a sunset!");
RED === ORANGE // false 심볼은 모두 서로 다르다.
```
- 우연히 다른 식별자와 혼동해서는 안 되는 고유한 식별자가 필요하다면 심볼을 사용하자.
## null과 undefined
- null이 가질 수 있는 값은 null이며, undefined 또한 undefined 하나만 가질 수 있다. 또한, 둘 다 모두 존재하지 않는 것을 나타낸다.
- null은 프로그래머에게 허용된 데이터 타입이며 undefined는 자바스크립트 자체에서 사용한다. (이 규칙이 강제는 아니다)
## 객체
- 원시 타입은 단 하나의 값만 나타낼 수 있고 불변이지만, 이와 달리 객체는 여러 가지 값이나 복잡한 값을 나타낼 수 있으며, 변할 수 있다. 
- 객체의 본질은 컨테이너이다. 내용물이 바뀐다고 컨테이너가 바뀌는 건 아니다. 즉, 여전히 같은 객체이다.
```javascript
const obj = {};
// 객체의 콘텐츠는 프로퍼티 또는 멤버라고 부른다. 프로퍼티는 이름(키)과 값으로 구성된다. 프로퍼티 이름은 반드시 문자열 또는 심볼이어야 하며, 값은 어떤 타입이든 사관없고 다른 객체여도 괜찮다.
obj.color = "yellow"; // key => color, value => yellow
// 프로퍼티 이름에 유요한 식별자를 써야 (.)을 사용할 수 있다.
// 그게 아니면 []를 사용해야한다.
// 이들을 멤버 접근 연산자라고 한다.
obj["not an identifier"] = 3;
obj["not an identifier"]; // 3
obj["color"]; // "yellow"
// 심볼 프로퍼티에 접근할 때도 대괄호를 사용
const SIZE = Symbol();
obj[SIZE] = 8;
obj[SIZE]; // 8

// 참고로 obj.SIZE는 obj["SIZE"]를 한 것으로 obj[SIZE]와는 다릅니다.
```
- 원시 값과 객체의 차이는 변수 obj에 저장된 객체를 수정했지만, obj는 항상 같은 객체를 가리키고 있다. 즉, obj는 계속 같은 객체를 가리키고, 바뀐 것은 객체의 프로퍼티이다.
```javascript
const sam1 = {
  name: 'Sam',
  age: 4,
};

const sam2 = { name: 'Sam', age: 4 }; // 한 줄도 가능
// sam1과 sam2는 프로퍼티가 같지만, 서로 다른 객체이다. 원시 값은 서로 같은 원시 값을 가리킨다.

cosnt sam3 = {
  name: 'Sam',
  classification: { // 프로퍼티 값이 객체가 될 수 있따.
    kingdom: 'Anamalia',
    class: 'Mamalia'
  }
}
// sam3.classification.kingdom
// sam3.classification["kingdom"]
// sam3.["classification"].kingdom
// sam3["classification"]["kingdom"]
// 모두 같은 표현
```
- 객체에 함수도 담을 수 있지만, 이건 추후에 알아보자
```javascript
sam3.speak = function() { return "Meow" };
sam3.speak(); // "Meow" // 함수 호출 시엔 괄호가 필요
```
- 객체에서 프로퍼티를 제거할 때는 delete 연산자를 이용한다.
```javascript
delete sam3.classification;
delete sam3.speak;
```
## Number, String, Boolean 객체
- 숫자와 문자열, 불리언에는 각각 대응하는 객체 타입 Number, String, Boolean이 있다. 이들이 있는 이유는 Number.INFINITY 같은 특별한 값을 저장하는 것이고, 다른 하나는 함수 형태로 기능을 제공하기 위함이다.
```javascript
const s = "hello";
s.toUpperCase(); // "hello"
```
- s가 분명 원시 문자열 타입이지만, 자바스크립트가 String 객체로 만들어서 이 임시 객체에 toUpperCase 함수가 사용된것이다.
- 함수가 호출 될 때만 임시 객체가 만들어지고, 끝나면 임시 객체가 파괴된다.
```javascript
const s = "hello";
s.rating = 3; // 에러가 없다.
s.rating // undefined;
// 마치 s에 프로퍼티를 할당하는 것처럼 보이지만, 일시적인 String 객체에 프로퍼티를 할당한 것이다. 임시 객체는 바로 파괴되므로, undefined가 출력된다.
```
## 배열
- 자바스크립트 배열은 특수한 객체이다. 항상 순서가 있고, 키는 순차적인 숫자이다.
- 자바스크립트 배열 특징
  1. 배열 크기는 고정되지 않는다. 언제나 추가 혹은 제거가 가능
  2. 요소의 데이터 타입을 가리지 않는다.
  3. 배열 요소는 0으로 시작된다.
```javascript
const a1 = [1,2,3,4];
const a2 = [1, 'two', 3, null]
const a3 = [
  { name: "Ruby", hardness: 9 },
  { name: "Diamond", hardness: 10 },
  { name: "Topaz", hardness: 8 },
]
const a4 = [
  [1,2,3],
  [4,5,6],
];

a1.length; // 4
a1[0]; // 1
a1[a1.length - 1]; // 4, 마지막 요소

a1[3] = 10; // [1,2,3, 10]
```
## 객체와 배열 마지막의 쉼표
- 자바스크립트 문법에서는 마지막 쉼표를 계속 허용했지만, 인터넷 익스플로러 초기 버전은 마지막 쉼표를 쓰면 에러를 냈었다.
- 쓰든 말든 자유다.
## 날짜
- 자바스크립트의 날짜와 시간은 내장된 Date 객체에서 담당. 사실 Date 객체는 원래 자바에서 가져온 것이다.
```javascript
const now = new Date(2016, 9, 31);
now.getFullYear(); // 2016
now.getMonth(); // 9
now.getDate(); // 31
```
## 데이터 타입 변환
### 숫자로 바꾸기
```javascript
const numStr = "33.3";
const num = Number(numStr); // Number 객체의 인스턴스가 아니다.
// 숫자로 바꿀 수 없는 문자열에넌 NaN이 반환된다.

const a = parseInt("16 volts", 10); // "volts"는 무시되고 10진수 16이 된다.
const b = parseInt("3a", 16); // 16진수 3a를 10진수로 바꾼다. 결과는 58이다.
// 두 번째 인자를 넘기지 않는 경우엔 무조건 10진수로 변환된다.
```
- Data 객체를 숫자로 바꿀 떄는 valueOf() 메서드를 사용한다. UTC 1970/01/01 자정으로부터 몇 밀리초가 지났는지 나타내는 숫자이다.
### 문자열로 반환
- 자바스크립트의 모든 객체에는 문자열 표현을 반환하는 toString() 메서드가 있다.
```javascript
const n = 33.5;
const s = n.toString();
const arr = [1, true, "hello"];
arr.toString // "1,true,hello"
```
### 불리언으로 변환
- 부정 연산자(!)를 써서 모든 값을 불리언으로 바꿀 수 있다.
```javascript
const n = 0; // 거짓 같은 값
const b1 = !!n; // false
const b2 = Boolean(n); // false
```
## 요약
- 자바스크립트에는 문자열, 숫자, 불리언, null, undefined, 심볼의 여섯 가지 원시 타입과 객체 타입이 존재한다.
- 자바스크립트의 모든 숫자는 더블이다.
- 배열은 특수한 객체이다.
- 날짜, 맵, 셋, 정규표현식 등 자주 사용할 다른 데이터 타입들은 특수 객체 타입이다.
## 참조형과 원시형
- 원시 값은 불변이고, 원시 값을 복사/전달할 때는 값 자체를 복사/전달한다.
```javascript
let a = 1;
let b = a;
a = 2;
console.log(b) // 1
```
- 값 자체를 전달하므로 함ㄴ수 안에서 변수의 값이 바뀌어도 함수 외부에서 바뀌지 않는 상태로 남는다.
```javascript
function change (a) {
  a = 5;
}

a = 3;
change(a);
console.log(a) // 3
```
- 객체는 가변이고, 객체를 복사/전달할 때는 객체가 아니라 그 객체를 가리키고 있다는 사실을 복사/전달한다. 따라서 원본이 바뀌면 사본도 바뀐다. 이를 참조 타입이라한다.
```javascript
let o = {a: 1};
let p = o;
o.a = 2;
console.log(p) // {a: 2}

let o = {a:1};
let p = 0;
p === o; // true
o = {a: 2}; // 이제 o는 다른 것을 가리킨다. {a: 1}을 수정하지 않음
p == o; // false
console.log(p) // {a:1}
```
- 객체를 가리키는 변수는 그 객체를 가리킬 뿐 절대 객체 자체가 아니다.
```javascript
let p = {a: 1};
q === {a: 1} // false
```
- 참조를 전달하므로 함수 안에서 변경하면 함수 외부에서도 바뀐다.
```javascript
function change_o (o) {
  o.a = 999;
}

let o = {a: 1};
change_o(o);
console.log(o) // {a: 999}
```
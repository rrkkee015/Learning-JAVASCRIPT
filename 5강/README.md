# 표현식과 연산자
- 결과를 반환하는 것이 표현식. 그게 아니면 그저 문이라고 말한다.
## 연산자
```javascript
const s = "5"
const y = 3 + +s; // 8, 단항 연산자가 없었으면 "35"가 됐다.
```
## 비교 연산자
- === => 일치
  - 두 값이 같은 객체를 가리키거나, 같은 타입이고 값도 같다면(원시 타입)
- == => 동등
  - 두 값이 같은 객체를 가리키거나 같은 값을 갖도록 변환할 수 있다면 두 값을 동등
- 예제
  - 33 == "33" => 두 값은 동등 왜냐면 서로 변환이 가능하기에
- 결론 : 동등 연산자 안쓰는 습관을 가지자
## 숫자 비교
- NaN은 그 자신을 포함하여 무엇과도 같지 않음
  - NaN === NaN / NaN == NaN => 모두 false
## 논리 연산자
- Javascript의 거짓
  - undefined / null / false / 0 / NaN / ''
  - 문자열 "false"는 true 이다.
  - 빈 배열도 참이기에, arr.length로 비교 연산을 하자
## 조건 연산자
```javascript
const doIt = false;
const result = doIt ? "Did it!" : "Didn't do it.";
```
## 해체 할당
```javascript
const obj = { b : 2, c : 3, d : 4 };

// 해체 할당
const { a, b, c } = obj;
a; // undefined
b; // 2
c; // 3
d; // ReferenceError "d"가 정의되지 않음

let a, b, c;
{ a, b, c } = obj; // 에러
({ a, b, c} = obj); // 동작함

const arr = [1,2,3,4,5];

let [x, y, ...rest] = arr;
x; // 1
y; // 2
rest; // [3, 4, 5]

let a = 5, b = 10;
[a, b] = [b, a]
a; // 10
b; // 5
```
## 표현식 제어 패턴
```javascript
options = options || {};
// 아래와 같다.
if(!options) options = {};
```
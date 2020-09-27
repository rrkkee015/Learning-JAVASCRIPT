# 배열과 배열 처리
## 배열 요소 조작
```javascript
const arr = ['b', 'c', 'd']
arr.push('e'); // ['b', 'c', 'd', 'e']
arr.pop(); // ['b', 'c', 'd']
arr.unshift('a'); // ['a', 'b', 'c', 'd'] appendleft
arr.shift(); // ['b', 'c', 'd'] popleft
```

```javascript
const arr = [1, 2, 3];
arr.concat(4, 5, 6); // [1, 2, 3, 4, 5, 6]을 반환한다.
```

```javascript
const arr = [1, 2, 3, 4, 5];
arr.slice(3); // [4, 5]를 반환한다.
arr.slice(-2); // [4, 5]를 반환한다.
arr.slice(-2, -1); // [4]를 반환한다.
```

```javascript
// ES6에서 도입한 새 메서드
const arr = new Array(5).fill(1); // [1, 1, 1, 1, 1]
arr.fill("a"); // ["a", "a", "a", "a", "a"]
arr.fill(2, 1); // ["a", 2, 2, 2, 2]
arr.fill(3, 2, 4): // ["a", 2, 3, 3, 2]
```

```javascript
const arr = [1, 2, 3, 4, 5];
arr.reverse(); // arr는 [5, 4, 3, 2, 1]
arr.sort(); // arr는 [1, 2, 3, 4, 5]
```

```javascript
const arr = [
  { name: "Suzanne" },
  { name: "Jim" },
  { name: "Trevor" },
  { name: "Amanda" }
]
arr.sort(); // arr은 바뀌지 않습니다.
arr.sort((a, b) => a.name > b.name); // arr는 name 프로퍼티의 알파벳 순으로 정렬된다.
arr.sort((a, b) => a.name[1] < b.name[1]); // arr 알파벳 역 순으로 정렬된다.
```

```javascript
const arr = [1, 2, 3, 4, 5, 1, 2, 3, 4, 5];
arr.indexOf(2) // 1
arr.lastIndexOf(2) // 6
arr.indexOf(2, 3) // 6
arr.indexOf(6) // -1 없으면 -1 반환

const ar = [
  {id: 5, name: "J"},
  {id: 7, name: "F"}
]
ar.findIndex(o => o.id === 7); // 1
ar.findIndex(o => o.id === 17); // -1
ar.find(o => o.id === 7); // {id: 5, name: "J"}
ar.find(o => o.id === 2); // undefined
```

```javascript
const arr = [1, 17, 16, 5, 4, 16];
arr.find((x, i) => i > 2 && Number.isInteger(Math.sqrt(x))); // x 는 배열 값, i는 인덱스 값
// 값은 4
```

```javascript
class Person {
  constructor(name) {
    this.name = name;
    this.id = Person.nextId++;
  }
}

Person.nextId = 0;
const jamie = new Person("Jamie"),
juliet = new Person("Juliet"),
peter = new Person("Peter"),
jay = new Person("Jay");

const arr = [jamie, juliet, peter, jay];

// id를 직접 비교
arr.find(p => p.id == juliet.id); // juliet 객체 반환

// this 매개변수를 이용
arr.find(function (p) {
  return p.id === this.id
}, juliet); // juliet 객체
```

```javascript
const arr = [5, 7, 12 ,15, 17];
arr.some(x => x % 2 === 0); // true;
arr.some(x => Number.isInteger(Math.sqrt(x))); // false; 제곱수가 없어

const ar = [4, 6, 16, 36];
ar.every(x => x % 2 === 0); // true;
ar.every(x => Number.isInteger(Math.sqrt(x))); // false; 6은 제곱 수가 아니다.
```

```javascript
const cart = [
  {
    name: "Widget",
    price: 9.95
  },
  {
    name: "Gadget",
    price: 22.95
  }
]
const names = cart,map(x => x.name); // ["Widget", "Gadget"]
const prices = cart.map(x => x.price); // [9.95, 22.95]
const discountPrices = prices.map(x => x * 0.8); // [7.96, 18.36]
```

```javascript
const items = ["W", "G"];
const prices = [9.95, 22.95];
const cart = items.map((x, i) => ({ name: x, price: prices[i]}));
// 여기서 객체를 괄호로 감싼 이유는 화살표 표기법에서 객체 리터럴의 중괄호를 블록으로 판단하기 때문
// cart = [{ name: "W", price: 9.95}, { name: "G", price: 22.95}]
```

```javascript
// filter 사본을 반환한다.
const cards = [];
for (let suit of ['H', 'C', 'D', 'S'])
  for (let value = 1; value <= 13; value++)
    cards.push({ suit, value });

// value가 2인 카드
cards.filter(c => c.value === 2);

cards.filter(c => c.value >10 && c.suit === 'H'); // length: 3
```

```javascript
const arr = [5, 7, 2, 4];
const sum = arr.reduce((a, x) => a += x, 0);
// 18
```
- reduce 콜백 함수는 매개변수로 누적값 a와 현재 배열 요소 x를 받았다. 이 예제에서 누적 값은 0으로 시작한다.

```javascript
const arr = [1, null, "hello", "world", true, undefined];
delete arr[3];
arr.join(); // "1,,hello,,true";
arr.join(''); // "1hellotrue";
arr.join(' -- '); // "1 -- -- hello -- -- true -- --"

const attributes = ["Nimble", "Perceptive", "Generous"];
const html = '<ul><li>' + attributes.join('</li><li>') + '</li></ul>';
```

# 요약
- 책 222p를 참고하세요.
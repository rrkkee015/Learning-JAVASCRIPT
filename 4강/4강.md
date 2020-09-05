# 제어문
```javascript
for([initialization]; [condition]; [final-experssion])
  statement

// 위의 코드는 아래와 같다.
[initialization]
while([condition]) {
  statment
  [final-expression]
}

// python 처럼 객체의 프로퍼티에 루프를 실행할 수 있다.
for(variable in object)
  statement

// ES6에서 새로 생긴 반복문으로서 컬렉션의 요소에 루프를 실행한다.
for(variable of object)
  statement
// for ... of 루프는 배열은 물론 이터러블 객체에 모두 사용할 수 있는 루프
```
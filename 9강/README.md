# 객체와 객체지향 프로그래밍
## 객체지향 프로그래밍
- 객체는 데이터와 기능을 논리적으로 묶어 놓은 것이다.
  - 자동차가 객체라면 그 데이터에는 제조사, 모델, 도어 숫자, 차량번호 등이 있을 것이다.
  - 기능으로는 가속, 변속, 문 열기 등이 있다.
- 클래스는 어떤 자동차처럼 추상적이고 범용적인 것, 인스턴스는 특정 자동차처럼 구체적이고 한정적인 것, 기능은 메서드라고 부른다.
  - 클래스에 속하지만 특정 인스턴스에 묶이지는 않는 기능을 클래스 메서드라고 한다.
  - 예를 들어 '시동을 거는' 기능이 되겠다.
  - 클래스를 처음 만들 때 생성자가 실행되면서, 객체 인스턴스를 초기화한다.
- OOP는 클래스를 계층적으로 분류하는 수단도 된다.
  - 자동차보다 더 범용적인 *운송 수단* 클래스가 있다고 가정하면, 운송 수단엔 바퀴가 없는 보트도 있을 것이다. 이런 경우 운송 수단을 *슈퍼 클래스*라 부르고 자동차를 *서브 클래스*라 부른다.
  - 서브 클래스는 서브 클래스를 가질 수 있다. 요트, 카누가 보트에 포함이 되겠다.
### 클래스와 인스턴스 생성
```javascript
// ES6
class Car {
  constructor () {

  }
}

const car1 = new Car();
const car2 = new Car();

car1 instanceof Car // true
car1 instnaceof Array // false
```

```javascript
class Car {
  constructor(make, model) {
    this.make = make;
    this.model = model;
    this.userGears = ['P', 'N', 'R', 'D'];
    this.userGear = this.userGears[0];
  }

  shift(gear) {
    if (this.userGears.indexOf(gear) < 0)
      throw new Error(`Invalid gear: ${gear}`);
    this.userGear = gear;
  }
}
```
- 여기서 this 키워드는 의도한 목적, 즉 메서드를 호출한 인스턴스를 가리키는 목적으로 쓰였다. 클래스를 만들 때 사용한 this 키워드는 나중에 만들 인스턴스의 플레이스 홀더이다.
```javascript
const car1 = new Car("Tesla", "Model S");
const car2 = new Car("Mazda", "3i");
car1.shift('D');
car2.shift('R');
```
### 클래스는 함수다
```javascript
// ES5에서 class를 선언하는 방법
function Car(make, model) {
  this.make = make;
  this.model = model;
  this._userGears = ['P', 'N', 'R', 'D'];
  this._userGears = this.userGears[0];
}

// ES6라고 클래스가 바뀐 것이 아니고, 함수라고 생각하면 된다.
```

### 프로토타입
- 클래스의 인스턴스에서 사용할 수 있는 메서드를 프로토타입 메서드라고 말한다. 위에서 Car의 인스턴스에서 사용한 shift 메서드는 프로토타입 메서드이다.
- 자바스크립트가 *프로토타입* ㅔ인을 통해 어떻게 *동적 디스패치*를 구현하는지 알아보자.
- 함수의 프로토타입 프로퍼티가 중요해지는 시점은 new 키워드로 새 인스턴스를 만들었을 때다. new 키워드로 만든 새 객체는 생성자의 프로토타입 프로퍼티에 접근할 수 있다. 객체 인스턴스는 생성자의 프로토타입 프로퍼티를 `__propto__` 프로퍼티에 저장한다.
- 프로토타입에서 중요한 것은 *동적 디스패치*라는 메커니즘이다. (디스패치는 메서드 호출과 같은 의미이다) 클래스의 인스턴스는 모두 같은 프로토타입을 공유하므로 프로토타입에 프로퍼티나 메서드가 있다면 해당 클래스의 인스턴스는 모두 그 프로퍼티나 메서드에 접근할 수 있다.
- 객체의 프로퍼티나 메서드에 접근하려 할 대 그런 프로퍼티나 메서드가 존재하지 않으면 자바스크립트는 객체의 프로토타입에서 해당 프로퍼티나 메서드를 찾는다.
```javascript
const car1 = new Car();
const car2 = new Car();

car1.shift === Car.prototype.shift; // true
car1.shift('D');
car1.shift('d'); // error
car.userGear; // 'D';
car.shift == car2.shift // true
// car1객체에 shift라는 메서드가 없지만 car1.shift('D')를 호출하면 자바스크립트는 car1의 프로토타입에서 그런 이름의 메서드를 검색한다.

car1.shift == function(gear) { this.useGear = gear.toUpperCase() };
car1.shift === Car.prototype.shift; // false
car1.shift === car2.shift; // false
car1.shift('d');
car1.userGear; // 'D'
// car1의 shift 메서드를 추가하면, 더 이상 프로토타입 메서드는 호출되지 않는다.
```

### 정적 메서드
- 인스턴스에서 사용하게끔 만든 메서드를 주로 봤지만, 메서드는 *정적 메서드(클래스 메서드)*가 존재한다.
- 정적 메서드에서 this는 인스턴스가 아니라 클래스 자체에 묶인다. (일반적으로 정적 메서드에는 this 대신 클래스 이름을 사용하는 것이 좋은 습관이다)
- 정적 메서드는 클래스에 관련되지만 인스턴스와는 관련이 없는 범용적인 작업에 사용된다. 예를들어서 자동차 식별 번호를 붙이는 메서드를 생각하면, 개별 자동차가 자신만의 자동차 식별 번호를 생성한다는 것은 불가능하다. 다른 자동차에서 이미 사용할 수도 있기 때문이다. 즉, 자동차 식별 번호를 할당한다는 것은 자동차 전체를 대상으로 하는 추상적인 개념이므로 정적 메서드를 사용하는게 어울린다.
```javascript
class Car {
  static getNextVin() {
    return Car.nextVin++; // this.nextVin++으로 써도 되지만, Car를 앞에 쓰면 정적 메서드라는 점을 상기하기 좋다.

    constructor(make, model) {
      this.make = make;
      this.model = model;
      this.vin = Car.getNextVin();
    }
    static areSimilar(car1, car2) {
      return car1.make === car2.make && car1.model === car2..model;
    }
    static areSame(car1, car2) {
      return car1.vin === car2.vin;
    }
  }
}

Car.nextVin = 0;

const car1 = new Car("Tesla", "S");
const car2 = new Car("Mazda", "3");
const car3 = new Car("Mazda", "3");

car1.vin; // 0
car2.vin; // 1
car3.vin; // 2

Car.areSimiliar(car1, car2); // false
Car.areSimiliar(car2, car3); // true
Car.areSame(car2, car3); // false
Car.areSame(car2, car2); // false
```
### 상속
- 프로토타입을 이해하면서 우리는 상속의 일면을 봤다. 클래스의 인스턴스는 클래스의 기능을 모두 상속한다. 객체의 프로토타입에서 메서드를 찾지 못하면 자바스크립트는 프로토타입의 프로토타입을 검색한다. *프로토타입 체인*이라한다. (계속해서 거슬러 올라가다가 조건에 맞는 프로토타입을 찾지 못하면 에러가 발생)
- 클래스의 계층 구조를 만들 때 프로토타입 체인을 염두에 두면 효율적인 구조를 만들 수 있다. 즉, 프로토타입 체인에서 가장 적절한 위치에 메서드를 정의하면 좋다.
```javascript
class Vehicle {
  constructor() {
    this.passengers = [];
    console.log("Vehicle created");
  }
  addPassenger(p) {
    this.passengers.push(p);
  }
}

// extends 키워드는 Car를 Vehicle의 서브클래스로 만든다.
// super()는 슈퍼클래스의 생성자를 호출하는 특별한 함수이다. 없으면 에러뜸
class Car extends Vehicle {
  constructor() {
    super();
    console.log("Car created");
  }
  deployAirbags() {
    console.log("BWOOSH!");
  }
}

// 운송수단이 존재한다. 자동차에는 에어백이 있다. 근데 에어백이 있는 보트는 없다. 반면, 운송 수단은 모두 승객을 태울 수 있다. 그래서 Car에서 메소드를 만들었다.

const v = new Vehicle();
v.addPassenger("Frank");
v.addPassenger("Judy");
v.passengers;
const c = new Car();
c.addPassenger("Alice");
c.addPassenger("Cameron");
c.passengers;
v.deployAirbags(); // error
c.deployAirbags();

// 상속은 단방향이다. Car에서 Vehicle에 접근 가능하지만 Vehicle에서 Car는 안된다.
```
### 다형성
- *다형성*이란 단어는 객체지향 언어에서 여러 슈퍼클래스의 멤버인 인스턴스를 가리키는 말이다. 대부분의 객체지향 언어에서 다형성은 특별한 경우에 속한다.
- 얘도 됐다가 쟤도 되는 성질을 가리키는 말이다.
- 키보드인건 같아도 esc와 enter는 다르게 동작하는거 처럼
- 객체가 클래스의 인스턴스인지 확인하는 instanceof 연산자가 있다.
```javascript
class Motorcycle extends Vehicle {}
const c = new Car();
const m = new Motorcycle();
c instanceof Car; // true
c instanceof Vehicle; // true
m instanceof Car; // false
m instanceof Motorcycle; // true
m instanceof Vehicle; // true
```
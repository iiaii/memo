# 생성자 함수 동작 방식 


### new [function명] 했을때

> new를 붙여서 선언하면 기본적으로 생성자 함수로 동작한다

1. 빈 객체 생성

- 빈 객체가 생성 (생성자 함수가 새로 생성하는 객체)
- 빈 객체는 생성자 함수의 prototype 프로퍼티가 가리키는 객체를 자신의 프로토타입 객체로 설정


2. this를 통한 프로퍼티 생성

- 생성자 함수 내에 사용되는 this는 생성된 빈 객체를 가리킴
- 생성된 빈 객체에 this를 사용하여 동적으로 프로퍼티나 메소드를 생성할 수 있음
- this를 통해 생성한 프로퍼티와 메소드는 새로 생성된 객체에 추가됨


3. 생성된 객체 반환

- 반환문이 없는 경우, this에 바인딩된 새로 생성한 객체가 반환됨 (명시적으로 this를 반환하여도 결과는 같음)
- 반환문이 this가 아닌 다른 객체를 명시적으로 반환하는 경우, this가 아닌 해당 객체가 반환되는데 this를 반환하지 않은 함수는 생성자 함수로서의 역할을 수행하지 못하기 때문에 생성자 함수는 반환문을 명시적으로 사용하지 않음



```javascript
function Person(name) {
  console.log('1');
  this.name = name;  
  console.log('2');
}

var me = new Person('Lee');
console.log(me.name);

// 1
// 2
// Lee
```


### 객체 리터럴 방식과 생성자 함수 방식의 차이

```javascript
// 객체 리터럴 방식
var foo = {
  name: 'foo',
  gender: 'male'
}

console.dir(foo);

// 생성자 함수 방식
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}

var me  = new Person('Lee', 'male');
console.dir(me);

var you = new Person('Kim', 'female');
console.dir(you);

// Object
// Person
// Person
```

객체 리터럴 방식과 생성자 함수 방식의 차이는 프로토타입 객체([[Prototype]])에 있다.

- 객체 리터럴 방식의 경우, 생성된 객체의 프로토타입 객체는 Object.prototype이다.
- 생성자 함수 방식의 경우, 생성된 객체의 프로토타입 객체는 Person.prototype이다.


### 생성자 함수에 new 연산자를 붙이지 않고 호출할 경우


일반함수와 생성자 함수에 특별한 형식적 차이는 없으며 함수에 new 연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작한다.
그러나 객체 생성 목적으로 작성한 생성자 함수를 new 없이 호출하거나 일반함수에 new를 붙여 호출하면 오류가 발생할 수 있다. 
일반함수와 생성자 함수의 호출 시 this 바인딩 방식이 다르기 때문이다.
일반 함수를 호출하면 this는 전역객체에 바인딩되지만 new 연산자와 함께 생성자 함수를 호출하면 this는 생성자 함수가 암묵적으로 생성한 빈 객체에 바인딩된다.



---
[javascript this](https://poiemaweb.com/js-this)
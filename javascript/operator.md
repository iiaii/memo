# 자바스크립트

```
1. 자바스크립트는 타입 강제라는 속성을 가진다
2. 타입강제는 데이터 타입을 변환해서라도 작업을 완료하려는 특성이다
```

### 자바스크립트 연산자 종류

- 동등 비교 연산자 : ``==, !=``
- 일치 비교 연산자 : ``===, !==``
- 관계 비교 연산자 : ``<, =<, >, =>``
- 논리 연산자 : ``&&, ||, !``

> `==`, `!=` : 값이 동일한지 다른지 여부를 비교 


> `===`, `!==` : 같은 데이터 타입인지 다른 데이터 타입인지 비교 


##### 예시

`'3' !== 3` -> true
`'3' != 3` -> false (자바스크립는 타입 강제를 통해 '3'을 3으로 바꾼다)


### 자바스크립트의 모든 값들은 true/false 값으로 취급될 수 있다

##### false로 취급될 수 있는 값

- false
- 0 (숫자)
- NaN (ex. 10/'num')
- '' (빈값)
- undefined 
- null

##### true로 취급될 수 있는 값

- true
- 0이 아닌 다른 수 (음수, 계산된 수도 포함)
- 모든 문자 ('true', 'false', ' ', ...)
- 문자열로 표현된 숫자 ('1','0','-1' ...)


### true 와 true로 취급할 수 있는 값은 서로 다르다

##### 값 동일 여부 비교


- `null == 0` : false
- `undefined == 0` : false
- `NaN == null` : false
- `null == false` : false
- `NaN == NaN` : false
- `undefined == false` : false
- `false == 0` : true
- `undefined == null` : true
- `false == ''` : true
- `0 == ''` : true

##### 같은 데이터 타입인지 비교 

- `false === 0` : false
- `false === ''` : false
- `0 === ''` : false
- `undefined === null` : false



---
[자바스크립트 동등 비교연산자](https://jennybeblog.github.io/2017-05-15/JS_1/)

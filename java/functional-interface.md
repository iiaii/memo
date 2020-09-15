# Functional Interface


일급객체, First Class Citizen은 아래의 조건을 만족해야 한다.


1. 모든 요소는 함수의 실제 매개변수가 될 수 있다.
2. 모든 요소는 함수의 반환 값이 될 수 있다.
3. 모든 요소는 할당 명령문의 대상이 될 수 있다.
4. 모든 요소는 동일 비교의 대상이 될 수 있다.

> 로빈 포플스톤의 정의

Java의 경우 어느 클래스에도 속하지 않은 독립적인 함수가 존재할 수 없고 객체에 속한 메서드로 존재하기 때문에 함수 자체를 객체에 할당한다는 것 자체가 불가능하다.
따라서 Java는 위의 조건을 만족하지 않으며 일급객체가 아니고 또한 함수형 프로그래밍 언어가 아니다. 


하지만 Java는 클래스에 메서드가 하나만 존재하는 Funcional Interface (함수형 인터페이스)를 제공하여 실제로는 객체에 속한 메서드이지만 일급객체처럼 다룰 수 있도록 하였다. 
Java의 함수형 인터페이스는 기존의 익명객체로 오버라이딩 하던 1개의 메서드만 존재하는 클래스(or 인터페이스)와도 호환되며 람다표현식으로 불필요한 코드 작성을 최소화 하였다. 

```java
// 함수형 인터페이스
@FunctionalInterface
interface Calculation {
  int calculate(final int num1, final int num2);
}

// 실행
Calculation addition = (n1, n2) -> n1 + n2;
Calculation subtraction = (n1, n2) -> n1 - n2;
System.out.println(addition.calculate(1,2));
System.out.println(subtraction.calculate(3,4));

// 결과
3
-1
```


```java
// 위 코드와 호환되는 기존 익명객체 코드

// Calculation addition = (n1, n2) -> n1 + n2;
Calculation addition = new Calculation() { 
  @Override
  public int calculate(final int num1, final int num2) {
      return num1+num2;
  }
}
```


---
### 대표적인 함수형 인터페이스 종류


- Function<T, R> : T는 파라미터 타입, R은 리턴타입으로 추상메서드 apply()를 가진다
- Predicate<T, Boolean> : boolean을 리턴하는 함수형 인터페이스로 추상메서드 test()를 가진다
- Consumer<T> : 리턴이 없이 소비만 하는 형태이며 추상메서드 accept()를 가진다
- Supplier<R> : 파라미터가 없이 제공하는 형태이며 추상메서드 get()을 가진다 (Lazy Evaluation 가능)





---
[함수형 인터페이스](https://velog.io/@litien/Modern-Java-Java-8-%ED%95%A8%EC%88%98%ED%98%95-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)

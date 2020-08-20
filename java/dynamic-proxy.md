# 다이나믹 프록시

### 개요

```java
// Spring Data jpa
public interface BookRepository extends JpaRepository<Book, Integer> {
}
```

```java
// 동작
@Autowired
BookRepository bookRepository;

bookRepository.findall();
...
```

내부를 구현한적이 없는 인터페이스인데 어떻게 동작하는 것인가?? >> 다이나믹 프록시 (Spring AOP의 프록시 팩토리에서 프록시 객체를 빈으로 등록해 주입이 됨)
 
 
프록시 패턴으로 구현할 때 여러 중복되는 기능을 감싸고자 프록시가 겹겹이 쌓이는 일이 발생할 수 있다. (클라이언트 - 프록시 - 프록시 - 프록시 ... - 리얼서브젝트) 
매번 프록시를 구현하기 보다 동적으로 런타임에서 생성해서 적용하는 것을 다이나믹 프록시라고 한다

---
### 프록시 패턴

![proxy pattern](https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Proxy_pattern_diagram.svg/1200px-Proxy_pattern_diagram.svg.png)

프록시, 말 그대로 대리인의 역할을 하는 프록시를 중간에 두고 관계를 맺는다. [ 클라이언트 - 프록시 - 리얼서브젝트 ]

- 프록시와 리얼 서브젝트가 공유하는 인터페이스가 있고 클라이언트는 해당 인터페이스 타입으로 프록시를 사용
- 클라이언트는 프록시를 거쳐서 리얼 서브젝트를 사용하기 때문에 프록시는 리얼 서브젝트에 대한 접근을 관리하거나 부가기능을 제공하거나 리턴값을 변경할 수 있음
- 부가적인 기능(접근 제한, 로깅, 트랜잭션, Lazy 로딩 등)을 제공할 때 사용함 (리얼 서브젝트는 SRP(단일책임 원칠)를 준수해야 함)

```java
// 아주 간단한 프록시 예제 (AOP)
public class Proxy implements RealSubject {
  
  RealSubject realSubject;
  
  public Proxy(RealSubject realSubject) {
      this.realSubject = realSubject;
  }
  
  @Override
  public void do() {
      System.out.println("start");
      realSubject.do();
      System.out.println("end");
  }
}
```

[gof-design-patterns](https://github.com/iiaii/gof-design-patterns)




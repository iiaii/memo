# OSIV (Open Session In View)

``Open Session In View 는 영속성 컨텍스트를 뷰 렌더링이 끝나는 시점까지 개방한 상태로 유지하는 것이다``


```java
// 다음 코드의 실행 결과는? 
@GetMapping("/member/{memberIdx}") 
public String member(@PathVariable Long memberIdx, Model model) { 
  Member member = memberRepository.findOne(memberIdx); 
  model.addAttribute("name", member.getName()); 
  model.addAttribute("team", model.getTeam().getName()); 
  return "index"; 
}
```

> 위 코드는 `LazyInitializationException`이 발생할 것 같지만 정상적으로 결과가 나온다. (Spring Default가 OSIV를 사용하는 것이기 때문이다)


---
### 영속성 컨텍스트 

하이버네이트는 도메인 객체들이 하부의 데이터 저장소와 세부적인 영속성 메커니즘에 대해 몰라도 되게끔 영속성 컨텍스트라는 추상적인 공간, 개념을 제공한다. 
영속성과 관련된 모든 관심사는 도메인 객체로부터 격리된 채 관리자 객체에 의해 투명하게 처리되는데, 이 관리자 객체의 기능을 담당하는 Hibernate의 객체가 `Session 객체`이고, 이 `Session 객체`는 **영속성 컨텍스트**를 포함한다. 


영속성 컨텍스트는 `Transaction`이라는 쪼갤 수 없는 업무처리의 작업의 원자 단위로 처리되며 1:1로 연결된다. (1개의 트랙잭션에서는 1개의 영속성 컨텍스트가 존재)

하나의 `Transaction`동안 수정된 객체의 모든 상태는 영속성 컨텍스트 내에 저장되어, `Transaction`이 종료될 때 데이터 저장소에 동기화된다. (트랜잭션이 종료될때 flush 되며 이때 DB로 쿼리가 나가게 된다) 
그래서 하이버네이트 Session을 하나의 작업 단위 동안 생성 및 조회, 수정, 삭제되는 객체의 상태를 보관하는 일종의 캐시로 간주하기도 한다. (Session을 1차 캐시로 부르기도 함)

```java
@Transactional
public void moveTeam(Long memberIdx, Team team) {
  Member member = memberRepository.findOne(memberIdx);
  member.moveTeam(team);
}
```

위 코드는 moveTeam 메서드가 실행될 때 `@Transactional`에 의해 자동으로 Session이 열리게 된다. 조회한 Member 객체는 영속성 컨텍스트에 위치하고 (캐시) `Transaction`동안 Member 객체의 변경사항이 추적된다. 
이때 Member 객체는 영속성을 갖는다고 말한다. 영속성을 갖는 객체는 변경사항이 생기면 영속성 컨텍스트가 변경사항을 감지하게 된다. 
`Transaction`이 종료되는 시점에 영속성 컨텍스트가 캐시하고 있는 객체의 변경상태를 저장소와 동기화하며, 이때 update 쿼리가 자동으로 발생하게 된다. (이것을 Dirty Checking이라고 한다)


---
### 영속성을 갖는 객체의 상태 

![영속성 객체의 상태](https://kingbbode.github.io/images/2016/2016_12_28_OPEN_SESSION_IN_VIEW/status.png)

- Persistence

데이터베이스 식별자(Primary Key)를 가지며 작업 단위 내에서 영속성에 의해 관리되는 상태

> Transaction안에서 조회, 저장, 수정 등을 통해 가져온 영속성이 부여된 상태를 Persistence 상태라고 하고, Transient 상태의 객체도 강제로 Persistence 상태로 전환시킬 수 있다.


- Detached

데이터베이스 식별자를 가지지만 영속성 컨텍스트로부터 분리되어 더 이상 데이터베이스와의 동기화가 보장되지 않는 상태

> Session.close() 메소드는 Session을 닫고 영속성 컨텍스트를 포함한 모든 자원을 반환하며, 관리하에 있는 모든 영속 인스털스를 Detached 상태로 변경시킨다. 
> 하이버네이트는 Detached 상태의 객체에 대해서는 변경사항을 추적하지 않기 때문에 데이터베이스와의 동기화도 수행하지 않는다.


- Transient

아무 상태도 가지지 않는 상태 (무)

> new 연산자를 사용하여 생성한 객체는 곧바로 Persistence 상태가 되지않고 Transient 상태를 갖게 된다.


- Removed

제거될 상태

> Transaction이 종료되는 시점에 삭제될 객체는 Remoed 상태를 갖는다.



---
### 지연 로딩

JPA에서 페치 전략은 2가지가 있다.

- EAGER : 데이터를 즉시 가져오는 전략 (즉시 로딩) [@ManyToOne, @OneToOne]
- LAZY : 데이터가 처음 액세스 될 때 가져오는 전략 (지연 로딩) [@OneToMany, @ManyToMany]


LAZY, 지연로딩을 하게되면 지연로딩하게 되는 객체는 초기에 프록시 객체로 채워진다. 
프록시 객체는 실제 엔티티 대신 실제 객체처럼 위장한 객체로 실제 객체의 참조를 보관하며 실제 엔티티의 데이터에 접근할때 영속성 컨텍스트에 엔티티가 생성되어 있지 않으면 영속성 컨텍스트에 객체 생성을 요청하게 된다.
이를 프록시 초기화라 하고 처음 사용될 때 한번만 초기화 된다.

![지연 로딩](https://kingbbode.github.io/images/2016/2016_12_28_OPEN_SESSION_IN_VIEW/proxy_test.png)

위와 같이 지연 로딩을 하게되면 해당 객체를 조회할때 쿼리 1번, 객체의 참조객체를 액세스하는 시점에 쿼리가 1번 더 발생하게 된다. (이는 n+1 문제의 원인이며 JPQL의 `fetch join`과 batch fetch size로 해결한다)


스프링은 서비스 레이어가 애플리케이션의 트랙잭션 경계를 정의하는 역할로 발생하는 문제를 해결하기 위해 등장한 `Open Session In View Pattern`을 기본으로 사용한다. 
즉, 서비스 레이어 밖인 프레젠트 레이어 (컨트롤러)에서 엔티티를 수정하거나, 해당 객체의 내부를 참조하려고 할때 발생하는 `LazyInitializationException`을 방지하기 위해 OSIV를 활용한다.

```java
//Controller @GetMapping("") 
public String home(Model model){ 
  model.addAttribute("teams", teamService.findAll()); 
  return "home"; 
} 

//Service 
@Transactional 
public List<Team> findAll(){ 
  return teamRepository.findAll(); 
}
```

> 위 결과는 LazyInitializationException이 발생한다. findAll 메서드가 종료될때 트랜잭션이 종료되면서 하이버네이트 세션이 종료되고 영속객체는 `Detacted` 상태가 된다. 그래서 종료된 영속객체의 참조객체에 접근할때마다 LazyInitializationException이 발생하는 것을 방지하기 위해 OSIV가 등장하고 Spring default로 적용되게 되었다.

---
### OSIV

뷰 렌더링 시점에 영속성 컨텍스트가 존재하지 않아서 Detached 된 객체의 프록시를 초기화할 수 없는 경우 영속성 컨텍스트를 오픈된 채로 뷰 렌더링 시점까지 유지하는 것이다. 
즉, 작업 단위를 요청 시점부터 뷰 렌더링 완료 시점까지로 확장한다. 

![original osiv](https://kingbbode.github.io/images/2016/2016_12_28_OPEN_SESSION_IN_VIEW/servlet_osiv.png)

전통적인 서블릿 필터의 OSIV 패턴은 JDBC 커넥션을 뷰 렌더링이 완료되어야 풀로 반환하기 때문에 지나치게 JDBC 커넥션을 점유하고, 뷰까지 트랜잭션이 확장되며 모호한 트랜잭션 경계를 가져 문제가 생길 수 있다.
Spring OSIV는 이런 단점을 보완하기 위해 OpenSessionInViewFilter 와 OpenSessionInViewInterceptor를 제공함으로써, 기존 뷰처럼 지연로딩을 가능하게 하는 동시에 서비스 레이어에 트랜잭션 경계를 선언할 수 있다. 

![spring osiv](https://kingbbode.github.io/images/2016/2016_12_28_OPEN_SESSION_IN_VIEW/spring_osiv.png)

> 스프링 OSIV는 영속성 컨텍스트만 뷰까지 연장 가능하다. 트랜잭션이 종료되더라도 컨트롤러 세션이 열려있어서 영속 상태를 유지할 수 있고, 프록시 객체에 대한 Lazy Loding을 수행할 수 있게 된다. (flush 도 제어 가능함) 하지만 결국 DB 커넥션을 오래동안 가지고 있게되는 문제는 해결되지 않는다.


정리하자면
- 프레젠테이션 계층에서 트랜잭션이 없어서 엔티티 변경이 불가능하다
- 영속성 컨텍스트는 열려있어서 지연로딩이 가능하다




---
### OSIV의 문제점과 주의점

스프링 Boot에서는 Open Session In View 패턴을 OpenEntityManagerInViewInterceptor를 통해 default로 지원을 해주고 있다. 
하지만 애초에 OSIV를 이용해 엔티티를 프레젠테이션 계층까지 내리는 것은 지연 로딩에 대한 여지를 주고, 혹여라도 프레젠테이션 계층에서 엔티티를 수정한 직후 트랜잭션을 시장하는 서비스 계층을 만나면 예기치 못한 변경이 발생한다. 


```java
// 애초에 로직적인 부분이 컨트롤러에 위치한것도 문제지만
// 이 코드는 member의 이름이 XXX로 바뀌는 문제가 발생한다.
@Controller
class HelloController{
    @Autowired
    private HelloService helloService;

    @GetMapping("/member/{memberId}")
    public String addMember(@PathVariable Integer memberId, ModelMap modelMap){

        Member member = helloService.logic(memberId);
        member.setName("XXXX"); // 보안상의 이유로 XXXX로 세팅해서 내림

        helloService.biz();

        modelMap.add("member", member);

        return "/member/list";
    }
}

@Service
class HelloService{
    @Transactional
    public void biz(){
        // ...
    }
}
```

영속성 컨텍스트가 프레젠테이션 (컨트롤러) 계층까지 열려있는 것은 결국 DB 커넥션을 유지하는 시간이 늘어나 DB 커넥션이 모자라는 상황이 발생할 수 있다. 
또한 위의 경우와 같이 커넥션을 열어두는 것은 디버깅하기 어려운 오류의 원인이 될수 있다. 따라서 OSIV를 끄고 (끄지 않더라도) service에서 DTO로 변환해서 내려주는 방식이 가장 안전하다.



---
- [OSIV](https://kingbbode.tistory.com/27) 
- [영속성 관리](https://stylishc.tistory.com/150)


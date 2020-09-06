# OSIV (Open Session In View)

``Open Session In View 는 영속성 컨텍스트를 뷰 렌더링이 끝나는 시점까지 개방한 상태로 유지하는 것이다``

> OSIV는 주의할 것이 많다. 따라서 이 글은 영속성 컨텍스트, 트랜잭션과 OSIV의 문제에 대한것을 다룬다.


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


(추가 내용...)




---
- [OSIV](https://kingbbode.tistory.com/27) 
- [영속성 관리](https://stylishc.tistory.com/150)


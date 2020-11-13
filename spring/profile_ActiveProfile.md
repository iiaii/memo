# @Profile / @ActiveProfiles


로컬 개발 환경과 배포 후 서버 환경에서 구동되는 설정이나 코드가 다를 수 있다.

이때 `@Profile` 과 `@ActiveProfiles` 을 활용한다.



### @Profile

```java
// 베이스 코드
public interface HelloService {

  String hello(String name);
}

public class DefaultHelloService implements HelloService {

  @Override
  public String hello(String name) {
    return "hello " + name;
  }
}

public class WorldHelloService implements HelloService {
  @Override
  public String hello(String name) {
    return "hello world " + name + "!";
  }
}
```


- 중첩 클래스

```java
// 중첩 클래스
@Configuration
public class HelloConfig {

  @Configuration
  @Profile("default")
  static class DefaultHelloConfig {
    @Bean
    HelloService helloService() {
      return new DefaultHelloService();
    }
  }
  
  @Configuration
  @Profile({"dev", "prod"})
  static class DevHelloConfig {
    @Bean
    HelloService helloService() {
      return new WorldHelloService();
    }
  }
}
```

- 메서드

```java
// 메서드
@Configuration
public class HelloServiceConfig {
  
  @Bean
  @Profile("default")
  HelloService defaultHelloService() {
    return new DefaultHelloService();
  }
  
  @Bean
  @Profile({"dev", "prod"})
  HelloService worldHelloService() {
    return new worldHelloService();
  }
}
```

- Not

```java
// dev 아닌 경우에만 
@Bean
@Profile("!dev")
HelloService defaultHelloService() {
  return new DefaultHelloServicer();
}
```


### @ActiveProfile

`@ActiveProfile`은 테스트 코드에서 앞서 설정한 프로필을 지정할 수 있다.

```java
@Runwith(SpringRunner.class)
@SpringBootTest
@ActiveProfiles("dev")
public class SpringProfilesTests {
  
  @Autowired
  private HelloService helloServicer;
  
  @Test
  public void profilesTests() {
    assertThat(helloService.hello("iiaii")).isEqualTo("hello world iiaii!");
  }
}

// dev 환경에 주입된 클래스
public class WorldHelloService implements HelloService {
  @Override
  public String hello(String name) {
    return "hello world " + name "!";
  }
}
```



### yml 설정


```yaml

...

---

spring:
  profiles: test
    
---

spring:
  profiles: dev
  
---

spring:
  profiles: prod
```


`---` 구분자는 여러 yml 파일을 생성한것 처럼 구분하게 된다.

결국 위의 파일은 아래파일들을 생성한것과 같은 효과를 낸다.

- application.properties 
- application-test.properties 
- application-dev.properties 
- application-prod.properties

테스트 코드에서 다음과 같이 사용하면 해당 설정에 맞게 코드가 적용된다.

```java
// 

@RunWith(SpringRunner.class)
@ActiveProfiles("test")
@SpringBootTest
@Transactional
public class KafkaFaceTagReceiverTest {
  ...
}
```






---
[@Profile / @ActiveProfiles](http://wonwoo.ml/index.php/post/1933)

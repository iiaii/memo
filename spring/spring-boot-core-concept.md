# Spring Boot 원리


### 의존성 관리


spring boot는 pom.xml에 다음의 코드만 있어도 수많은 의존성을 해결해 라이브러리로 가지고 온다.


```java
// pom.xml의 spring-boot-starter-parent 
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.5.RELEASE</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>
```


계층 구조를 확인해 보면 `pom.xml < spring-boot-starter-parent-2.3.5.RELEASE.pom.xml < spring-boot-dependencies-2.3.5.RELEASE.pom.xml` 과 같다.

spring-boot-dependencies에는 dependencyManagement 영역 내에 spring-boot-starter-web, spring-boot-starter-jdbc 등 여러 의존성들이 버전 호환되어 위치해 있다.

따라서 별다른 설정 없이 pom.xml에서 spring-boot-starter-parent 를 parent로 지정하면 해당 상위 계층에 위치한 의존성들은 pom.xml에서 별도로 버전을 명시하지 않더라도 해당 의존성들을 가져온다.

```java
// pom.xml의 dependency
<dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <!-- 버전을 명시하지 않아도 상위의 spring-boot-dependencies에 위치한 버전으로 호환 -->
    </dependency>
</dependencies>
```

- 만약 특정 parent 만을 상위 계층으로 삼아야 하는 상황이라면?


이러한 경우에는 parent로 삼는 의존성의 parent를 spring-boot-starter-parent 로 지정해서 해결하면 된다.


---
### Auto Configuration

스프링 부트 프로젝트의 메인 클래스에는 `@SpringBootApplication`이 붙어있다.

```java
// 최상위 패키지에 위치한 메인 실행부
@SpringBootApplication
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(SpringBootGettingStartedApplication.class, args);
  }
}
```

`@SpringBootApplication` 는 크게 3가지로 구성되어있다.

- `@SpringBootConfiguration`
- `@ComponentScan`
- `@EnableAutoConfiguration`


> `@SpringBootConfiguration`은 SpringBootConfiguration 이라는 이름을 제외하면 사실상 `@Configuration`과 동일하다.


SpringBootApplication에서 빈은 2단계로 나눠서 읽힌다.

1. `@ComponentScan`
2. `@EnableAutoConfiguration`

`@ComponentScan`은 해당 패키지 이하의 모든 `@Component`를 가진 클래스들을 스캔해서 빈으로 등록한다.

`@EnableAutoConfiguration`은 spring.factories 파일의 `org.springframework.boot.autoconfigure.EnableAutoConfiguration`에 값으로 기본 설정 클래스 파일들이 명시되어 있고, 해당 클래스들
(전부 자바 설정 파일이지만 `@ConditionalOnClass`, `@ConditionalOnMissingBean`와 같은 조건이 붙어 빈으로 등록하기도하고 등록하지 않기도 한다.)

즉, `@EnableAutoConfiguration`을 통해 spring.factories 파일의 수많은 자동 설정이 조건에 따라 자동 생성된다.

> `@Configuration`은 빈을 등록하는 자바 설정 파일


##### ComponentScan을 먼저 하는 이유 (2단계에 걸쳐서 빈을 등록하는 이유)

SpringBootApplication에서 `@ComponentScan`을 먼저 진행하는 이유는 `@EnableAutoConfiguration`에서 자동 설정되는 빈들보다 우선권을 갖기 위함이다. 

만약 현재 프로젝트에서 자동설정에서 만들어지는 빈과 동일한 클래스의 빈을 만들때 Bean의 이름과 Qualifier로 직접 지정해야하는 번거로움이 있다. 

spring.factories 파일의 EnableAutoConfiguration 값으로 잡힌 클래스 파일들을 살펴보면 `@ConditionalOnMissingBean`과 같은 어노테이션이 있는데, 

`@ComponentScan`을 진행하면서 해당 클래스가 빈으로 등록되지 않은 경우에만 빈으로 등록한다는 조건이다. 

정리하자면, 2단계에 걸쳐서 빈을 등록하고 또 `@ComponentScan`을 먼저 진행하는 이유는 

현재 프로젝트 파일에서 `@EnableAutoConfiguration` 과정에서 등록되는 빈들에 대해 우선권을 주거나 조건을 만들수 있게 하기 위함이다. 


> 사실 `@Configuration`과 `@ComponentScan`만 있어도 Spring Application이 구동된다.

```java
// @SpringBootApplication
@Configuration
@ComponentScan
// @EnableAutoConfiguration
public class Application {
  SpringApplication application = new SpringApplication(Application.class);
  application.setWebApplication(WebApplicationType.NONE);
  application.run(args);
}
```


---
### 스프링부트 웹서버


스프링 부트는 서버가 아니다. 

스프링 부트 자동설정에 내장 서블릿 컨테이너 (tomcat, jetty, undertow)가 설정되어 있으며, 기본적으로 spring-boot-starter-web은 tomcat을 사용해 서버를 띄운다.

``spring-boot-autoconfigure > META-INF > spring.factories > ServletWebServerFactoryAutoConfiguration > EmbeddedTomcat...``

1. 톰캣 객체 생성
2. 포트 설정
3. 톰캣에 컨텍스트 추가
4. 서블릿 만들기
5. 톰캣에 서블릿 추가
6. 컨텍스트에 서블릿 맵핑
7. 톰캣 실행 및 대기

[Tomcat, Servlet, JSP](https://github.com/iiaii/memo/blob/master/spring/servlet_jsp.md)




---
### SpringApplication

##### ApplicationListener


- ApplicationStartingEvent : Spring Boot Application 시작 전
- ApplicationStartedEvent : Spring Boot Application 시작 후

```java
public class SampleListener implements ApplicationListener<ApplicationStartingEvent> {
  
  @Override
  public void onApplicationEvent(ApplicationStartingEvent applicationStartingEvent) {
    System.out.println("Application is Starting");
  }
}


// main
public static void main(String[] args) {
  SpringApplication app = new SpringApplication(StpringInitApplication.class);
  app.addListeners(new SampleListener());
  app.run(args);
}
```


##### ApplicationRunner

```java
//  보편적으로 많이 사용되는 ApplicationRunner
@Component
public class SampleRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("foo: " + args.containsOption("foo"));
        System.out.println("bar: " + args.containsOption("bar"));
    }
}
```


---
### 외부설정

`application.properties`, `application.yml` 과 같은 외부설정 파일은 Spring에서 정해진 우선순위에 따라 덮어쓰이며 실행된다. 

완전한 덮어쓰기가 아닌 없는 항목들은 유지되고 중복되는 값들은 우선순위에따라 덮어쓰여진다.


- `application.properties` 우선순위 (높은게 낮은것을 덮어씀)

1. file: ./config/ (현재 프로젝트 루트의 config 디렉토리)
2. file: ./ (프로젝트 루트, jar 파일의 경우 실행 위치)
3. classpath:/config/
4. classpathe:/


- 프로퍼티 우선 순위

1. 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties
2. 테스트에 있는 @TestPropertySource
3. @SpringBootTest 애노테이션의 properties 애트리뷰트
4. 커맨드 라인 아규먼트
5. SPRING_APPLICATION_JSON (환경 변수 또는 시스템 프로티) 에 들어있는 프로퍼티
6. ServletConfig 파라미터
7. ServletContext 파라미터
8. java:comp/env JNDI 애트리뷰트
9. System.getProperties() 자바 시스템 프로퍼티
10. OS 환경 변수
11. RandomValuePropertySource
12. JAR 밖에 있는 특정 프로파일용 application properties
13. JAR 안에 있는 특정 프로파일용 application properties
14. JAR 밖에 있는 application properties
15. JAR 안에 있는 application properties
16. @PropertySource
17. 기본 프로퍼티 (SpringApplication.setDefaultProperties)


##### @ConfigurationProperties


```java
// application.properties의 iiaii prefix의 값을 모두 객체 프로퍼티로 접근 가능
@Component
@ConfigurationProperties("iiaii")
@Getter
public class iiaiiProperties {

  private String name;

  private int age;

  private String fullName;
}
```

`@Component`를 통해 빈으로 등록되어 주입받아 사용할 수 있다.

대신에 다음과 같은 설정을 해주어야 한다.

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```


이외에도 융통성있는 바인딩을 자동 지원해준다.

- context-path (케밥)
- context_path (언더스코어)
- contextPath (캐멀)
- CONTEXTPATH

또한 스프링이 타입캐스팅을 지원해서 자동으로 타입 컨버징이 된다. (properties의 값을 int, long 에 맞게 변환)

> `@Value`는 SpEL 을 사용할 수 있지만 융통성있는 바인딩이 불가능하다.

- `@DurationUnit`

```yaml
iiaii:
  sessionTimeout = 25
```


```java
// Duration 타입으로 캐스팅된다
@DurationUnit(ChronoUnit.SECONDS)
private Duration sessionTimeout = Duration.ofSeconds(30);
```

하지만 설정파일에서 `s`를 붙이면 자동으로 변환해준다.

```yaml
iiaii:
  sessionTimeout = 25s
```


- `@Validated`

`@Validated` 어노테이션으로 내용을 검증할 수 있다.

```java
@Component
@ConfigurationProperties("iiaii")
@Validated
@Getter
publci class iiaiiProperties {
  
  @NotEmpty
  String name;
  
  @Min(20)
  int age;
}
```

---
### 프로파일

```java
// prod configuration

@Profile("prod")
@Configuration
public class ProdConfiguration {

    @Bean
    public String hello() {
        return "hello prod";
    }
}
```

```java
// test configuration

@Profile("test")
@Configuration
public class TestConfiguration {

    @Bean
    public String hello() {
        return "hello test";
    }
}
```

```yaml
spring:
  profiled:
    active:
      prod
```

```
// 결과
hello prod
```

외부설정 우선순위를 사용해서 커맨드라인이나 외부 설정파일로 실행 프로퍼티를 변경할 수 있다.

이외에도 `spring.profile.include`를 통해 다른 properties(yml)을 호출해서 외부설정파일을 활성화 할 수 있다.

---
### 로그설정

```
spring.output.ansi.enabled=always
logging.file.path=logs
```

> 루트의 logs 디렉토리로 로그가 쌓이고 특정 주기만큼 아카이빙하도록 설정도 가능하다

slf4j가 PSA 처럼 다양한 로거들의 파사드로 위치해서 감싸도록 구성되어있고 스프링부트는 기본적으로 logback을 사용하도록 되어있다.


---
### 테스트

- 테스트 종류 (webflux 테스트 추천)

```java
@RunWith(SpringRunner.class)
//@SpringBootTest(webEnvironment = WebEnvironment.MOCK)
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
public class SampleServiceTest {

    @Autowired
    MockMvc mockMvc;

    @Autowired
    TestRestTemplate testRestTemplate;

    @MockBean
    SampleService mockSampleService;

    @Autowired
    WebTestClient webTestClient;

    // static 메서드 자동완성이 안되서 불편
    @Test
    public void mock_test1() throws Exception {
        mockMvc.perform(get("/hello"))
            .andExpect(status().isOk())
            .andExpect(content().string("hello iiaii"))
            .andDo(print());
    }

    // RANDOMPORT
    // restTemplate 헷갈려서 추천하지 않음
    @Test
    public void restTemplate_test2() throws Exception {
        String result = testRestTemplate.getForObject("/hello", String.class);
        assertThat(result).isEqualTo("hello iiaii");
    }

    // RANDOMPORT
    // restTemplate 헷갈려서 추천하지 않음
    @Test
    public void restTemplate_mockSampleService_test3() throws Exception {
        when(mockSampleService.getName()).thenReturn("iiaii123");

        String result = testRestTemplate.getForObject("/hello", String.class);
        assertThat(result).isEqualTo("hello iiaii123");
    }

    // RANDOMPORT
    // 가장 추천! - webflux
    @Test
    public void webflux_test4() throws Exception {
        when(mockSampleService.getName()).thenReturn("iiaii123");

        webTestClient.get().uri("/hello").exchange()
            .expectStatus().isOk()
            .expectBody(String.class).isEqualTo("hello iiaii123");
    }
}
```


##### 슬라이스 테스트 

테스트할때 모든 빈을 등록하지 않고 빠르게 테스트하고 데이터 롤백과 같은 부가 기능을 사용하려고 할때, 즉 레이어 별로 잘라서 테스트하고자 할때 슬라이스 테스트를 할수 있다.

- `@JsonTest` : Json 결과에 특화된 테스트
- `@WebMvcTest` : 웹과 관련된 것만(컨트롤러) 빈으로 등록해서 테스트 (웹계층 밑의 것은 `@MockBean`으로 주입받아야함)
- `@WebFluxTest` 
- `@DataJpaTest` : repository 관련 테스트를 할 수 있고, 기본적으로 테스트가 종료되면 롤백을 한다

> 슬라이스 테스트는 통합테스트인 `@SpringBootTest`와 달리 빠르게 일부분만을 등록해서 테스트 가능하다.



##### OutputCapture

logger를 통한 로그나 System.out.println()으로 출력되었는지 확인하는 것으로 테스트를 할 수있다.


```java
// 선언
@Rule
public OutputCaptureRule outputCaptureRule = new OutputCaptureRule();

// 테스트
@Test
public void logOutputCature_test() throws Exception {
    when(mockSampleService.getName()).thenReturn("iiaii123");

    mockMvc.perform(get("/hello"))
        .andExpect(content().string("hello iiaii123"));

    assertThat(outputCaptureRule.toString())
        .contains("holoman")
        .contains("skip");

}
```


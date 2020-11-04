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


```java
// 최상위 패키지에 위치한 메인 실행부
@SpringBootApplication
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(SpringBootGettingStartedApplication.class, args);
  }
}
```

`@SpringBootApplication` 이 위치한 메인부터 빈으로 등록할 컴포넌트들을 찾아 등록하고 빈

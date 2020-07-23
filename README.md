# memo

### Vue

- [Vue Reactivity System](https://vuejs.org/v2/guide/reactivity.html)
- [Render Function](https://vuejs.org/v2/guide/render-function.html)
- [State Management](https://vuejs.org/v2/guide/state-management.html), [Vuex 튜토리얼](https://joshua1988.github.io/web-development/vuejs/vuex-start/)
- [Server Side Rendering](https://vuejs.org/v2/guide/ssr.html)


### 서비스 운영

- [웹 사이트 최적화 기법(http://book.naver.com/bookdb/book_detail.nhn?bid=4587095)
- [대규모 서비스를 지탱하는 기술](http://book.naver.com/bookdb/book_detail.nhn?bid=6468636)
- [서버/인프라를 지탱하는 기술](http://book.naver.com/bookdb/book_detail.nhn?bid=6010115)

[기억보단기록을](https://jojoldu.tistory.com/284?category=689637)


### Spring Validation

- Spring Validation은 크게 3가지 계층에서 가능하다 (Controller, Service, Repository)

##### 검증 방법

1. 검증이 필요한 빈의 필드에 @NotBlank, @NotNull 등의 어노테이션을 추가한다
2. @Validated 를 클래스 레벨의 Controller, Service에 추가한다.(Repository의 경우 하이버네이트가 CRUD 할때 검증한다)
3. 메서드의 파라미터 앞이나, 리턴 값 앞에 @Valid 나 @NotBlank, @NotNull 등을 추가하면 해당 구문을 지날때 검증한다.
(**파라미터 뿐만 아니라 리턴 값 앞에도 @Valid를 붙일 수 있다.**)

List<@Valid Dto>, @Valid Dto 와 같은 형태도 가능 (파라미터 리턴 모두 가능)

[Validation](https://jongmin92.github.io/2019/11/18/Spring/bean-validation-1/)


### SSR

- SPA 전환
![spa](https://d2.naver.com/content/images/2020/06/step1.png)

- SSR 전환
![ssr](https://d2.naver.com/content/images/2020/06/step2.png)

CSR은 페이지 로드후 동적으로 컨텐츠를 생성하기 때문에 페이지가 노출되는 과정이 길다. 

SSR은 서버에서 사용자에게 보여줄 페이지를 모두 구성하여 사용자에게 페이지를 보여주는 방식으로 JSP/Servlet의 아키텍처 역시 이 방식을 사용했다.

페이지를 구성하는 속도는 늦지만 컨텐츠 구성이 완료되는 시점은 빨라지고, SEO(search engine optimization) 또한 쉽게 구성할 수 있다. 

##### 클라이언트 사이드 랜더링 / 서버사이드 랜더링 (CSR / SSR)

CSR은 SPA의 처음 모습으로 필요한 부분만 서버로 부터 JSON 같은 포맷으로 받아 트래픽이 감소한다.
또한 작은 데이터 변경마다 페이지를 새로고침할 필요없이 네이티브 앱과 비슷한 수준의 인터렉티브한 사용자 경험을 제공한다.
하지만 자바스크립트 엔진이 돌아가지 않으면 원하는 정보를 표시해주지 못하기 때문에 검색엔진 크롤러가 데이터를 제대로 수집하지 못한다.
(SEO - 검색엔진최적화)

SSR은 SEO가 가능하며 첫 랜더링된 html을 클라이언트에 전달해주기 때문에 초기 로딩 속도를 줄일수 있다.
하지만 SPA에서 SSR을 적용하는 것은 복잡하고 성능악화 여지가 있다. (렌더링하는 동안 이벤트루프가 막힘 - 동기적으로 실행)
메타태그만 넣어주거나, prerender를 통해 이 문제를 해결할 수도 있다.

[서버사이드랜더링](https://d2.naver.com/helloworld/7804182)
[CSR / SSR](https://velog.io/@rjs1197/SSR%EA%B3%BC-CSR%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
[CSR / SSR 2](https://velog.io/@zansol/%ED%99%95%EC%9D%B8%ED%95%98%EA%B8%B0-%EC%84%9C%EB%B2%84%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81SSR-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81CSR)


### OAuth2

인증을 위한 오픈 스탠다드 프로토콜로 외부의 사용자 정보나 서비스 기능에 접근할 수 있다.

![OAuth2](http://innovationm.co/wp-content/uploads/2018/06/Oauth-architecture.png)

[ref](http://innovationm.co/spring-security-with-oauth2/) 

##### 용어

- Resource Server : Google, Facebook 같은 사용자 정보를 가지고 있는 서비스
- Authorization Server : Google, Facebook 같은 서비스의 인증 서버 (Resource Server와 같은 security domain)
- Resource Owner : 사용자 (자원 소유자)
- Client : 웹, 앱 (현재 서비스 혹은 개발하고자 하는 서비스)


##### 진행 순서

1. Resource Server(Google ...)로 부터 Client (웹, 앱)가 Client Id, Client Secret 발급 (developer console 에서 발급)
2. Resource Owner(사용자)가 Client에서 권한이나 정보가 필요한 서비스에 접근
3. Authorization Server(인증서버, 로그인 페이지)로 Redirect (redirect - 서버가 브라우저에게 다른 주소로 요청할것을 지시)
4. Resource Owner가 인증을 마치면 Client가 Auth Code 수신
5. Client가 Client Id, Client Secret, Auth Code를 가지고 Resource Server에 인증 토큰 요청
6. Resource Server가 정보 확인 후 Client에게 인증 토큰 발급
7. Client는 Resource Server로 부터 사용자 정보나 서비스 기능에 접근


### CSRF (Cross Site Request Forgery)

웹사이트 취약점을 이용해서 이용자가 의도하지 않은 요청을 통한 공격을 말한다.
Http의 Stateless 특성으로 쿠키 정보만 이용해서 여러 공격이 발생할 수 있다. (로그인 상태에서 ~/logout을 호출하면 의도하지 않더라도 로그아웃이 됨)
form data를 사용하여 공격하려고 할 때 이를 방지하기 위해서 csrf 토큰을 사용하여 방지한다.

![form token](https://media.vlpt.us/images/max9106/post/fea74261-de2a-40ea-a4da-c85beb78b669/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-04-26%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%202.03.29.png)

Thymeleaf로 form을 만들면 hidden으로 csrf 토큰 값이 생성된다. 인증된 상태이더라도 안전하지 않은 정보를 받아들이지 않는다.

csrf 토큰 정보를 헤더에 포함시켜 서버요청을 하면 이를 해결할 수 있다 (로그아웃과 같은 행위도 GET보다 POST를 사용)

Spring Security Config 에서 configure 메서드에 다음 구문을 추가하면 쿠키가 함께 전송된다. 
(기본적으로 GET은 CSRF 토큰 정보를 넘기지 않더라도 작동한다)

`.csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())`

![CookieCsrfTokenRepository](https://github.com/cheese10yun/blog-sample/raw/master/assets/CSRF-Meber-filed.png)

발급 받은 쿠키의 키 값은 `XSRF-TOKEN`으로 `X-XSRF-TOKEN` 와 차이가 있다. (DEFAULT_CSRF_COOKIE_NAME 이 XSRF-TOKEN)
중간에 토큰 값을 변조하거나 넘기지 않으면 `Http Status Code403 Forbidden` 이 발생한다.


[Spring Securtiy CSRF](https://cheese10yun.github.io/spring-csrf/#null)
[csrf](https://velog.io/@max9106/Spring-Security-csrf)


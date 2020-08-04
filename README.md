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

##### 실제 구현한 SPA(Vue) + Spring OAuth2 API

SPA와 API로 분리되어 구동되는 경우 나와았는 Spring OAuth2 예제들로는 부족했다. [이거는 도움이 될수도?](https://www.baeldung.com/rest-api-spring-oauth2-angular)

실제 구현했던 순서는 (완전히 틀린 방법일 수 있음)

1. (vue-google-oauth2)[https://www.npmjs.com/package/vue-google-oauth2] 모듈을 사용해서 팝업으로 구글 로그인 폼으로 Auth Code 까지 Vue에서 받는다. 
2. Auth Code는 유효기간이 짧기 때문에 백으로 바로 Auth Code를 전송한다. (Auth Code가 공개되는 것도 보안상 좋을것이 없지만, 유효기간이 짧고 절대 공개되면 안되는 Client Secret이 백에만 존재함)
3. Auth Code를 수신한 백에서 [ Auth Code + Client Id + Client Secret ] 을 가지고 구글 API로 요청해서 액세스 토큰을 발급 받는다. (발급 받은 토큰은 한시간 정도 유효)
4. 발급 받은 토큰으로 구글에 유저 프로필 정보를 요청하고 수신한 정보들을 토대로 User 객체를 생성해 영속한다. (UserDetails, UserDetailsService 모두 구현함)
5. 생성된 유저 id를 반환하고 Set-Cookie로 구글에서 발급받은 토큰 정보를 넣어준다.
6. 프론트(Vue)에서는 토큰으로 백엔드에 유저정보를 요청한다.
7. 백엔드에서 토큰으로 구글에 유저 프로필 정보를 요청한다. (토큰이 유효한지, 그리고 나머지 정보가 갱신되었을 수 있기 때문에 갱신하기 위해서 요청한다)
8. 유저 정보와 영속된 User 객체를 비교하며 email(절대 바뀌지 않는 정보)을 제외한 나머지 요소 중 다른 값이 있으면 방금 구글에서 받아온 유저 프로필 정보로 갱신하고 UserDto로 반환한다. 
8. 유저 정보를 수신한 Vue에서 Store에 현재 로그인한 유저 정보를 갱신하고 화면에 로그인된 상태를 표시한다.

추가적으로 토큰이 쿠키로 저장되어 있기 때문에 로그아웃하지 않은 상태로 창을 다시 열면 Vue Created에서 쿠키로 유저 정보가 있는지 요청해 있으면 Store에 유저 정보를 채워 로그인된 상태를 유지한다.

[OAuth2 와 JWT](https://www.sauru.so/blog/basic-of-oauth2-and-jwt/)
[JWT 로그인 구현](https://webfirewood.tistory.com/m/115?category=694472)

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


### custom video control

브라우저 별로 지원하는 html video 기능 및 ui 등이 상이하다. 
일관된 서비스 환경을 제공하고 더 편리한 영상 서비스를 제공하기 위해 커스터 마이징이 필요하다.

##### 아이디어

비디오 태그의 controls 를 해제하고 div, span, input 태그 등에 css와 js로 액션을 입한다.
이벤트의 경우 키보드, 마우스 별로 세부적으로 구분되는 것을 활용하고 비디오 태그의 프로퍼티를 조작하여 구성한다.


### @ElementCollection

해당 어노테이션이 붙으면 Jpa 구현체는 모든 컬렉션의 데이터를 모두 삭제하고 다시 모든 데이터를 추가한다. PK가 존재하지 않아 발생하는 현상이지만, @OrderColumn 을 사용하면 순서값을 같이 저장해서 조회할때 사용하기 때문에 모두 삭제하는 문제를 방지할 수 있다고 한다. (확실한지 확인 필요)


@ElementCollection 을 사용하면 DB에서는 별도의 테이블이 구성된다. (논리적으로는 한 엔티티 안이지만 DB에서는 별개의 테이블이 생성되어 관리된다. 코드상(논리적으로) 분리되어 있지만 한 테이블에 관리되는 Embedded 타입과 상반된 경우이다) 별도의 테이블에 의해 관리되는 것으로 인한 성능 문제가 신경쓰인다면 실제 테스트를 해봐야겠지만 `String.join()` 를 이용해서 `,` 같은 구분자로 구분하는 전용 컨버터 클래스를 만들어서 String 형태로 저장 가능하다. (이 방식 역시 변환에서 발생하는 오버헤드를 고려해야 한다)


### Vue $mount vs el 차이점

```
new Vue({
    data () {
        return {
            text: 'Hello, World'
        };
    }
}).$mount('#app')
```

```
new Vue({
    el: '#app',
    data () {
        return {
            text: 'Hello, World'
        };
    }
})
```

위의 2가지는 동일하게 동작하지만 약간의 차이점이 있다.

$mount 의 경우 필요할때 Vue 인스턴스를 명시 적으로 마운트 할 수 있지만 el 방식은 불가능하다.
(페이지에 특정 요소가 존재하거나 일부 비동기 프로세스가 완료 될 때까지 vue 인스턴스의 마운트를 지연시킬 수 있다)
또한 요소 선택기없이 호출되고 이로 인해 Vue 모델이 문서와 별도로 렌더링되서 나중에 추가 할 수 있다.

- $mount()을 사용하면 필요한 요소를 마운트 할 수있는 유연성이 향상됨
- 인스턴스가 마운트 될 요소 (동적으로 생성 된 요소 일 수도 있음)를 실제로 알기 전에 인스턴스를 인스턴스화해야하는 경우 나중에 vm.$mount()을 사용하여 마운트 할 수 있음


### @Schedule

스케줄링을 사용하면 자바스크립트의 setTimeout, interval 처럼 코드에 딜레이를 주거나 주기적으로 실행 시키는 것이 가능하다.
예를 들어 지속적으로 데이터를 쌓거나 분석하는 경우에 사용하기도 하고 일정 시간이 지나면 만료 시키는 등에 활용 가능하다.

```
@Scheduled(fixedRateString = "60000")   // 1분 마다 실행
public void scheduledTask() {
}
```

동적으로 스케줄링을 하고 제거하는 코드도 가능하다.

```
private Map<String, ScheduledFuture<?>> scheduledTasks = new ConcurrentHashMap<>();

/**
 * 스케줄 등록 
 * @param token 
 * @param expiresIn
 */
 public void register(String token, Long expiresIn) {
    tokenExpireCountMap.put(token, 0);     
    ScheduledFuture<?> tokenExpireTask = taskScheduler.scheduleWithFixedDelay(
            () -> {               
            if (tokenExpireCountMap.get(token) == 1) {
                        userService.logout(token);                 
                        this.remove("loginUser"+token);
            }                
            tokenExpireCountMap.put(token, tokenExpireCountMap.get(token)+1);
        }, expiresIn * 1000);   
        scheduledTasks.put("loginUser"+token, tokenExpireTask);
    }
    
/** 
  * 스케줄 삭제 
  * @param id 
  */
  public void remove(String id) {    
      scheduledTasks.get(id).cancel(true);
  }
```

[Spring Schedule](https://jeong-pro.tistory.com/186)


### Spring Security

크게 2가지가 있다.

- Web Security (Filter Security Interceptor)
- Method Security (Method Security Interceptor)

이 2가지 모두 Security Interceptor를 사용하며 리소스 접근을 허용할것인가를 결정하는 로직이 포함되어 있다.
Security Interceptor는 Security Context Holder 라는 ThreadLocal(한 스레드 내에서 공유하는 자원, Java sdk 표준)을 마치 DB처럼 사용하며
Authentication Manager(인증 관리, 로그인 관리)와 Access Decision Manager(인가 관리, 접근 권한 관리) 역할을 한다.

Security Context Holder에서는 유저의 인증 정보를 저장하며 요청을 가로챈 Security Interceptor에서는 인증된 유저인지 Security Context Holder에서 찾는다. 
유저 인증 정보가 없으면 새로 인증이 필요한 사용자로 판단하여 Authentication Manager를 통해 로그인을 하게 된다. 
Authentication Manager에서 인증할때 사용하는 주요한 인터페이스로 UserDetailsService, PasswordEncoder가 있다.

##### Authentication 예시
- Basic Authentication  

인증 요청 헤더에 Authentication, Basic, username, password 를 합쳐서 인코딩한 문자열을 가지고 판단하며 password는  PasswordEncoder를 사용해서 검사를 한다. 인증이 완료되면 Authentication 객체를 생성해서 Security Context Holder에 저장을 해놓는다. 이후에는 요청에 대한 권한을 UserRole을 사용해서 확인한다.




##### ThreadLocal

ThreadLocal을 사용하면 스레드 영역에 변수를 설정할 수 있기 때문에 특정 스레드에서 실행하는 모든 코드에서 스레드에 설정된 변수 값을 사용할 수 있게 된다. (마치 DB혹은 컬렉션 처럼 사용가능하다)

```java
// 현재 쓰레드와 관련된 로컬 변수를 하나 생성한다.
ThreadLocal<UserInfo> local = new ThreadLocal<UserInfo>();

// 로컬 변수에 값 할당
local.set(currentUser);

// 이후 실행되는 코드는 쓰레드 로컬 변수 값을 사용
UserInfo userInfo = local.get();

// 삭제
local.remove(currentUser);
```


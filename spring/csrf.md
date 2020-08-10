# CSRF (Cross Site Request Forgery)

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

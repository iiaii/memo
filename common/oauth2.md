# OAuth2

인증을 위한 오픈 스탠다드 프로토콜로 외부의 사용자 정보나 서비스 기능에 접근할 수 있다.

![OAuth2](http://innovationm.co/wp-content/uploads/2018/06/Oauth-architecture.png)

[ref](http://innovationm.co/spring-security-with-oauth2/) 

### 용어

- Resource Server : Google, Facebook 같은 사용자 정보를 가지고 있는 서비스
- Authorization Server : Google, Facebook 같은 서비스의 인증 서버 (Resource Server와 같은 security domain)
- Resource Owner : 사용자 (자원 소유자)
- Client : 웹, 앱 (현재 서비스 혹은 개발하고자 하는 서비스)


### 진행 순서

1. Resource Server(Google ...)로 부터 Client (웹, 앱)가 Client Id, Client Secret 발급 (developer console 에서 발급)
2. Resource Owner(사용자)가 Client에서 권한이나 정보가 필요한 서비스에 접근
3. Authorization Server(인증서버, 로그인 페이지)로 Redirect (redirect - 서버가 브라우저에게 다른 주소로 요청할것을 지시)
4. Resource Owner가 인증을 마치면 Client가 Auth Code 수신
5. Client가 Client Id, Client Secret, Auth Code를 가지고 Resource Server에 인증 토큰 요청
6. Resource Server가 정보 확인 후 Client에게 인증 토큰 발급
7. Client는 Resource Server로 부터 사용자 정보나 서비스 기능에 접근

### 실제 구현한 SPA(Vue) + Spring OAuth2 API


SPA와 API로 분리되어 구동되는 경우 나와았는 Spring OAuth2 예제들로는 부족했다. [이거는 도움이 될수도?](https://www.baeldung.com/rest-api-spring-oauth2-angular)

![my-oauth2-implementation](https://github.com/iiaii/memo/blob/master/images/oauth2-implementation.PNG?raw=true)

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

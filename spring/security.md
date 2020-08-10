# Spring Security

크게 2가지가 있다.

- Web Security (Filter Security Interceptor)
- Method Security (Method Security Interceptor)

이 2가지 모두 Security Interceptor를 사용하며 리소스 접근을 허용할것인가를 결정하는 로직이 포함되어 있다.
Security Interceptor는 Security Context Holder 라는 ThreadLocal(한 스레드 내에서 공유하는 자원, Java sdk 표준)을 마치 DB처럼 사용하며
Authentication Manager(인증 관리, 로그인 관리)와 Access Decision Manager(인가 관리, 접근 권한 관리) 역할을 한다.

Security Context Holder에서는 유저의 인증 정보를 저장하며 요청을 가로챈 Security Interceptor에서는 인증된 유저인지 Security Context Holder에서 찾는다. 
유저 인증 정보가 없으면 새로 인증이 필요한 사용자로 판단하여 Authentication Manager를 통해 로그인을 하게 된다. 
Authentication Manager에서 인증할때 사용하는 주요한 인터페이스로 UserDetailsService, PasswordEncoder가 있다.

### Authentication 예시
- Basic Authentication  

인증 요청 헤더에 Authentication, Basic, username, password 를 합쳐서 인코딩한 문자열을 가지고 판단하며 password는  PasswordEncoder를 사용해서 검사를 한다. 인증이 완료되면 Authentication 객체를 생성해서 Security Context Holder에 저장을 해놓는다. 이후에는 요청에 대한 권한을 UserRole을 사용해서 확인한다.




### ThreadLocal

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

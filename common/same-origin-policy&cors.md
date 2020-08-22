# Same Origin Policy & CORS (Cross-Origin Resource Sharing)

웹은 기본적으로 Same Origin Policy, 동일한 출처 상의 요청만 가능하도록 한다. 
어떤 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용하는 것을 제한하기 위한 보안 방식으로 잠재적으로 해로울 수 있는 자원을 분리해 공격받을 수 있는 경로를 줄인다.
정리하자면, 웹 브라우저 보안을 위해 프로토콜, 호스트, 포트가 동일한 서버로만 ajax 요청을 주고 받도록 한 정책이다. 
(https://google.com - 프로토콜: https, 호스트: google.com, 포트: 443(https))
 
 
 
  
- same origin

```
http://www.same-domain.com/ --> http://www.same-domain.com
```

- cross origin

```
http://www.same-domain.com -/-> http://www.cross-domain.com
```


하지만 실제 개발을 하다보면 클라이언트와 서버를 분리하여 개발하거나 서비스 내에서도 외부 API를 사용하는 경우가 많다. 
이를 해결하기 위해 등장한 것이 바로 CORS (Cross-Origin Resource Sharing) 이다.
 

 
  
---
### CORS (Cross-Origin Resource Sharing)

HTTP 헤더를 사용해 브라우저가 한 출처에서 실행중인 웹 애플리케이션에 선택된 액세스 권한을 부여하도록 한다. 
다른 출처의 자원을 요청할때 Cross-Origin HTTP요청을 실행하기 때문에 출처가 다른 도메인의 요청도 서버에서 접근권한을 허용하기 때문에 Same Origin Policy를 우회한다.
  
 
CORS가 가능하게 하기 위해서는 2가지 방법이 있다
- 서버측에서 해결
- 클라이언트측에서 해결

##### 서버측에서 해결 

- Access-Control-Allow-Origin Response Header 

Access-Control-Allow-Origin 응답 헤더는 응답이 주어진 origin으로부터의 요청 코드와 공유될 수 있는지를 나타내는데 서버측 응답에서 접근 권한을 주는 헤더를 추가해 해결한다.

- 특정 도메인, 요청 접근 허용

요청을 필터링하여 서버측에서 미리 설정 해놓은 설정에 따라 허용할 것인지 거부할 것인지 판단하여 해결한다.
 
 
##### 클라이언트측에서 해결

- 프록시 서버를 두어 Access-Control-Allow-Origin 헤더를 추가하여 해결한다
- [JSONP](https://github.com/iiaii/memo/blob/master/common/jsonp.md)로 해결

---
- [same origin, cors](https://velog.io/@yejinh/CORS-4tk536f0db)






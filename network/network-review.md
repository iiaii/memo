# 네트워크 (HTTP, TCP/IP)

### IP 

노드의 주소로서 패킷 단위로 데이터가 전달 됨



##### 한계점

- 비연결성 : 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
- 비신뢰성 : 패킷이 사라지거나 순서대로 오지 않을 수 있음
- 프로그램 구분 : 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 2가지 이상이면? -> 포트 구분

> TCP, UDP 를 통해 해결


---
### TCP, UDP

애플리케이션 계층 - HTTP, FTP
전송 계층 - TCP, UDP
인터넷 계층 - IP
네트워크 인터페이스 계층


##### IP 패킷 정보

- 출발지 IP
- 목적지 IP
- 기타

##### TCP 세그먼트 정보

- 출발지 port
- 목적지 port
- 전송제어, 순서, 검증 정보 …
- 전송 데이터


##### TCP (전송 제어 프로토콜) 특징

- 연결지향 - TCP 3way handshake (가상 연결)
- 데이터 전달 보증
- 순서 보장
- 신뢰할 수 있는 프로토콜
- 현재는 대부분 TCP 사용


##### TCP 3 way handshake

1. SYN: 접속 요청
2. SYN + ACK: 요청 수락
3. ACK와 함께 데이터 전송 가능

> 물리적인 연결이 아닌 가상 연결


##### 순서 보장

TCP 세그먼트 정보에서 전송 제어, 순서 정보를 포함하기 때문에 가능


##### UDP (사용자 데이터그램 프로토콜) 특징

- 하얀 도화지에 비유 (기능이 거의 없음)
- 연결지향 - TCP 3 way handshake X
- 데이터 전달 보증 X
- 순서 보장 X
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름

> IP와 거의 같지만 PORT, 체크섬 정도만 있어서 애플리케이션에서 추가 작업 필요


---
### 포트

같은 IP 내에서 프로세스 구분하는 역할

0 ~ 65535 할당 가능
0~1023 잘 알려진 포트, 사용하지 않는 것이 좋음


---
### DNS


IP는 기억하기 어렵고 변경 될 수 있다..

-> 전화번호부 같은 DNS 사용 



---
### URI 

URI 안에 URL, URN이 있다
-> URI가 가장 큰 개념

- Uniform: 리소스 식별하는 통일된 방식
- Resource: 자원, URI로 식별할 수 있는 모든것 (제한 없음)
- Identifier: 다른 항목과 구분하는데 필요한 정보

##### URL - Locator: 리소스가 있는 *위치*를 지정

`scheme://[userinfo@]host[:port][/path][?query][#fragment]` 
-> `https://www.google.com:443/search?q=hello&hl=ko`


- userinfo는 거의 사용하지 않음
- host는 주소
- port는 보통 생략
- path는 리소스가 있는 경로로서 계층적 구조로 구성
- query는 key=value 형태로 구성 (query = query parameter, query string)
- fragment는 내부 북마크 등에 사용 (서버에 전송하는 정보는 아님)


---
### 웹 브라우저 요청 흐름


1. 웹브라우저에 `https://www.google.com:443/search?q=hello&hl=ko` 입력
2. `www.google.com` DNS 조회, IP와 포트 정보 찾아냄
3. HTTP 요청 메시지 생성

- HTTP 요청 메시지

``` 
GET /search?q=hello&hlko HTTP/1.1
Host: www.google.com
```

4. SOCKET 라이브러리를 통해 전달

[image:40DE24FF-614B-4747-BBFB-4ACFD2689182-88164-0000834E828572F6/스크린샷 2021-03-23 오후 6.03.25.png]

웹 브라우저와 TCP/IP 사이의 소켓 라이브러리를 통해 전달

5. TCP/IP 패킷 생성

[image:22799032-A752-4177-8D5B-8AACCCB8394B-88164-00008377FDF30E6C/스크린샷 2021-03-23 오후 6.06.23.png]
HTTP 메시지 (웹브라우저가 만든 전송 데이터) + (출발지 IP, PORT / 목적지 IP, PORT)

6. 서버로 요청 전송 및 패킷 해석 (각 계층에서 분해)

[image:76AFB1A4-A25B-4A90-8A4B-E1A5DB461C7C-88164-0000837E1853C8CD/스크린샷 2021-03-23 오후 6.06.50.png]

7. 서버에서 요청 응답 (서버도 똑같이 패킷을 생성)

[image:2D72A45D-F26F-4DFB-9258-4B5FA6F7E7AC-88164-000083928EBBE779/스크린샷 2021-03-23 오후 6.08.19.png]

8. 클라이언트에 요청 결과 전송 및 패킷 분해하여 브라우저가 랜더링


---
### 모든 것이 HTTP

하이퍼 텍스트 전송 프로토콜 

HTML, TEXT, 이미지, 음성, 영상, 파일, Json, XML 등
거의 모든 형태의 데이터 전송 가능

서버간 데이터를 주고 받을 때도 대부분 HTTP 사용

> 지금 거의 대부분 HTTP를 사용


##### 역사


- HTTP/0.9 (1991년): GET 메서드만 지원, 헤더 X
- HTTP/1.0 (1996년): 메서드, 헤더 추가
- **HTTP/1.1** (1997년) : 가장 많이 사용하고 중요한 버전
- HTTP/2 (2015년): 성능 개선 (증가하는 중)
- HTTP/3 (진행중): TCP 대신 UDP 사용하여 성능 개선 (증가하는 중)


##### 특징

- 클라이언트 서버 구조
- 무상태 프로토콜 

서버가 상태를 보존하지 않음 

-> 문맥이 없기 때문에 확장성 높음 
-> 어떤 다른 서버도 일처리가 가능하기 때문에 무한 서버 증설 가능, 서버 장애에도 강함
-> 상태를 유지해야하는 경우(로그인) 어쩔수 없이 상태를 유지해야 함 (최소한으로 사용)
-> 클라이언트가 매번, 많은 데이터 전송 필요

- 비연결성

서버는 연결을 유지할 필요가 없기 때문에 최소한의 자원을 사용할 수 있음 
(응답하고 바로 연결을 종료)

-> 서버 자원을 효율적으로 사용할 수 있음
-> 매번 TCP/IP (3 way handshake) 연결을 새로 맺어야함 (지연 시간 추가)
-> 웹 브라우저로 사이트를 요청하면 수 많은 자원이 함께 다운로드 되는데 매번 연결하고 끊으면 비효율적이기 때문에 지속 연결(Persistent Connections)로 문제 해결 (2.0, 3.0에서는 더 개선됨)

- HTTP 메시지

[image:D1EBA69F-5DA1-415A-855B-F71D00B3603E-88164-000086EAE690CA9A/스크린샷 2021-03-23 오후 7.42.17.png]
[image:D992D2B8-8F3C-4368-A8E3-37AEF3070686-88164-000086F88939C4F4/스크린샷 2021-03-23 오후 7.43.15.png]



- 단순함, 확장가능 

---
### HTTP 메서드

- GET : 리소스 조회
- POST :  요청 데이터 처리, 주로 신규 등록, 변경, 프로세스 처리에 사용

(컨트롤 URI라고해서 리소스가 동사로 표현되는 경우도 있음)
`POST /orders/{orderId}/delivery-start`

- PUT : 리소스를 완전히 대체, 해당 리소스가 없으면 생성

POST와 다르게 리소스가 명확함 (POST는 바디로 데이터가 넘어감)

- PATCH : 리소스 부분 변경

PUT이 완전히 대체하기 때문에 부분적인 변경에 PATCH를 사용
하지만 지원이 안되는 경우가 있기 때문에 이런경우는 POST 사용

- DELETE : 리소스 삭제
- … 

##### 멱등

1번 2번 100번 호출 모두 결과가 똑같아야 함 
(중간에 다른 요청으로 인한 변경은 고려하지 않음)

- GET
- PUT
- DELETE
- ~~POST~~ : 멱등하지 않음


##### 캐시 가능

응답 결과 리소스를 캐시해서 사용해도 되는가?

GET, HEAD, POST, PATCH 캐시 가능
-> 실제로는 GET, HEAD 정도만 캐시로 사용


---
### HTTP 메서드 활용

클라이언트에서 서버로 데이터 전송

- 쿼리 파라미터를 통한 데이터 전송
	- GET
	- 주로 정렬 필터 (검색어)

- 메시지 바디를 통한 데이터 전송
	- POST, PUT, PATCH
	- 회원 가입, 상품 주문, 리소스 등록, 리소스 변경


1. 정적 데이터 조회 (추가적인 데이터 필요 없이 리소스 경로만으로 조회 가능)
2. 동적 데이터 조회 (쿼리 파라미터 사용)
3. HTML Form 데이터 전송 (POST 전송) -> 파일도 전송 가능, GET도 가능
4. HTTP API 데이터 전송 -> 모든 메서드 사용 가능 (웹, 앱, 서버 등 대부분에서 사용)


---
### API 설계 예시

- 회원 목록 조회

`GET /users`

- 회원 조회

`GET /users/{id}`

- 회원 추가

`POST /users/{id}`

- 회원 수정 (PATCH, PUT 이 애매할때 사용)

`POST /users/{id}`

- 회원 완전히 대체

`PUT /users/{id}`

- 회원 부분 변경 (개념적으로는 이것을 사용하는것이 바람직함)

`PATCH /users/{id}`

- 회원 삭제

`DELETE /users/{id}`



##### POST로 등록하는 경우 생성된 리소스를 응답으로 넘겨준다

`POST /members` -> 서버가 알아서 만들어주고 관리한다 (리소스를 모름)

`Location: /users/100` 혹은 바디에 넣어서 리소스 정보를 제공해준다

> 컬렉션 방식 (리소스를 서버가 관리하는 방식, **대부분 서버가 사용하는 방식**)

##### PUT으로 등록하는 경우 덮어쓰기 때문에 클라이언트가 리소스 URI를 알고 있어야 한다.

`PUT /files/100` -> 클라이언트가 리소스를 관리하는 형태

> 스토어 방식 (리소스를 클라이언트가 관리)

##### HTML FORM 사용하는 경우 GET, POST 만 사용 가능

수정/삭제를 POST로만 해야 함

컨트롤 URI를 사용해야 함 -> `POST /members/{id}/delete`

실무에서는 컨트롤 URI를 사용할 경우가 생김


##### 참고하면 좋은 URI 설계 개념

- 문서 (document)

	- 단일 개념 (파일 하나, 객체 인스턴스, 데이터베이스 row)
	- ex) /members/100, /files/star.jpg

- 컬렉션 (collection)

	- 서버가 관리하는 리소스 디렉터리
	- 서버가 리소스의 URI를 생성하고 관리
	- ex) /members

- 스토어 (store) - 거의 사용하지 않음

	- 클라이언트가 관리하는 자원 저장소
	- 클라이언트가 리소스의 URI를 알고 관리
	- ex) /files/100

- 컨트롤러 (controller), 컨트롤 URI

- 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
- 동사를 직접 사용
- ex) /mebers/{id}/delete



---
### HTTP 상태코드

- 1xx (Informational): 요청이 수신되어 처리중 (거의 사용하지 않음)
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행동이 필요 
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함


##### 2xx (Successful)

- 200 OK -> 요청 성공
- 201 Created -> 요청 성공해서 새로운 리소스가 생성됨, 생성된 리소스는 응답 Location 헤더 필드로 식별 (없을수도 있음)
- 202 Accepted -> 요청이 접수 되었으나 처리되지 않음 (거의 사용하지 않음)
- 204 No Content -> 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음 (웹 문서 편집기에서 save 버튼 결과로 아무 내용이 없어도 된다)



##### 3xx (Redirection)

요청을 완료하기 위해 유저 에이전트의 추가 조치 필요 

> 유저 에이전트란 클라이언트의 브라우저를 말함

웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동 (리다이렉트)


1. 요청

```
GET /event HTTP/1.1
Host: localhost:8080
```

2. 응답

```
HTTP/1.1 301 Moved Permanently
Location: /new-event
```

3. 자동 리다이렉트 (이후 /new-event 로 진행)



리다이렉션에는 3가지 종류가 있음

-  영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동

-> 301 Moved Permanently

리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음 (MAY)

-> 308 Permanent Redirect

301과 기능은 같고, 리다이렉트시 요청 메서드와 본문 유지 (처음 POST를 보내면 리다이렉트도 본문과 메서드를 유지해서 재요청)

> 그렇게 많이 사용하지 않음 308은 특히

- 일시 리다이렉션 - 일시적인 변경 (주문 완료후 내역화면 이동, Post/Redirect/Get)

-> 302 Found

리다이렉트시 요청 메서드가 GET으로 변하고 본문이 제거될 수 있음 (MAY)

-> 307 Temporary Redirect

302와 기능은 같고 리다이렉트시 요청 메서드와 본문 유지 (요청 메서드를 변경하면 안된다. MUST NOT)

-> 303 See Other

302와 기능은 같고 리다이렉트시 요청 메서드가 GET으로 변경 (MUST)

> 골고루 쓰이지만 은근 302도 많이 쓰임

**PRG: Post/Redirect/Get**

POST로 주문후에 웹브라우저를 새로고침하면?

새로고침은 다시 요청 -> 중복 주문이 될 수 있다.

POST로 주문 후에 주문 결과 화면을 GET 메서드로 리다이렉트 함으로써 새로고침해도 결과 화면을 GET으로 조회 한다. (중복 주문 대신 결과 화면만 GET으로 다시 요청)

정리하면, POST 이후에 리다이렉션과 결과 헤더에 Location을 넘겨서 GET 으로 조회하도록 한다

> PRG는 많이 사용함


- 특수 리다이렉션 (결과 대신 캐시를 사용)

-> 304 Not Modified (많이 씀)

캐시를 목적으로 사용한다. 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용한다. (캐시로 리다이렉트)
그래서 304 응답은 메시지 바디를 포함하면 안된다. (로컬 캐시를 사용해야 하므로)
(조건부 GET, HEAD 요청시 사용함)



##### 4xx (Client Error)

- 클라이언트의 요청에 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 오류의 원인이 클라이언트에 있음
- 클라이언트가 이미 잘못된 요청, 데이터를 보내기 때문에 **재시도해도 실패**한다


-> 400 Bad Request

클라이언트가 잘못된 요청을 해서 서버가 요청으로 처리할 수 없음
(요청 구문, 메시지, 파라미터, api 스펙 오류)

-> 401 Unauthorized

클라이언트가 해당 리소스에 대한 인증이 필요함
(인증되지 않았기 때문에 WWW-Authenticate 헤더와 함께 인증 방법을 설명)

> 인증(Authentication)이란 본인이 누구인지 확인하는 것이고 (로그인), 인가(Authorization)는 권한 부여를 의미한다 (어드민 권한). 인증이 있어야 인가가 있게된다. Unauthorized 메시지 이름이 아쉬움

-> 403 Forbidden

서버가 요청을 이해했지만 승인을 거부함 (접근 권한이 불충분한 경우)

-> 404 Not Found

요청 리소스를 찾을 수 없음 (권한이 부족한 사용자에게 숨기고 싶을때도 사용)



##### 5xx (Server Error)

서버에 문제로 오류가 발생 (재시도하면 성공할 수도 있음)


-> 500 Internal Server Error

서버 문제로 오류 발생, 애매하면 500 오류

-> 503 Service Unavailable

서비스 이용 불가 (서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음)

> 5xx 에러는 웬만하면 만들면 안된다. (오남용 금지, 진짜 에러일때만 사용해야 함, 즉 예외처리가 되어야 함)



---
# HTTP 헤더

- 헤더 예시

```
// 리퀘스트 
GET /search?q=hello&hl=ko HTTP/1.1
Host: www.google.com // 헤더
```

```
// 리스폰스
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8 // 헤더
Content-Length: 3423					 // 헤더

<html>
	<body>...</body>
</html>
```


- HTTP 전송에 필요한 모든 부가정보
- 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리정보
- 표준 헤더가 상당히 많음
- 필요시 임의의 헤더 추가 가능


### HTTP BODY (message body - RFC7230 최신)

start line + 표현 헤더 + 표현 데이터 (= 메시지 본문 = 페이로드)

- 메시지 본문을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드
- 표현은 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공 (html, json, 데이터 길이, 압축 정보)

> 표현이라고 이야기하는 이유는 html로 json으로 표현하기 때문


### 표현

```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip
Content-Language: ko
Content-Length: 521

<html>
안녕하세요
<h1>lkj123k1</h1>
</html>
```

- Content-Type: 표현 데이터의 형식
- Content-Encoding: 표현 데이터의 압축 방식
- Content-Language: 표현 데이터의 자연언어
- Content-Length: 표현 데이터의 길이 (Transfer-Encoding 사용하면 Content-Length 사용하면 안됨)

> 표현 헤더는 전송, 응답 둘다 사용 



### 콘텐츠 협상 (content negotiation)

```
// 리퀘스트
GET /event
Accept-Language: ko
```

```
// 리스폰스
Content-Language: ko

안녕하세요
```

클라이언트가 선호하는 표현 요청

- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

> 협상 헤더는 요청시에만 사용


##### 협상과 우선순위

```
// q 값이 클수록 높은 우선순위 (0~1)
GET /event
Accept-Language:ko-KR,ko;q=0.9,en-US;q=0.8,en=;q=0.7
```

1. 한국어
2. 영어


```
// 구체적인 것이 우선
GET /event
Acccept: text/*, text/plain, text/plain;format=flowed, */*
```

1. `text/plain;format=flowed`
2. `text/plain`
3. `text/*`
4. `*/*`



### 전송 방식

- 단순 전송 (Content-Length)
- 압축 전송 (Content-Encoding: gzip)
- 분할 전송 (Transfer-Encoding: chunked) -> Content-Length는 사용하면 안됨
- 범위 전송 (Content-Range - 어느정도 까지 받은지 기억하고 실패할때 다시 그부분 부터 전송 요청)


### 일반 정보


- From: 유저 에이전트의 이메일 정보 (잘 사용되지 않음, 검색엔진에서 어뷰징 방지로 사용)
- Referer: 이전 웹 페이지 주소 (유입 경로 분석가능, referer는 사실 오타임)
- User-Agent: 유저 에이전트 애플리케이션 정보 (특정 브라어저 등에서 오류인것을 확인 가능)
- Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보 (프록시가 아닌 실제 요청 처리한 서버 정보, 응답에서 사용)
- Date: 메시지가 발생한 날짜와 시간 (응답에서 사용)



### 특별한 정보

- Host: 필수 헤더로서 요청한 호스트 정보 (도메인)


```
GET /hello HTTP/1.1
Host: aaa.com
```

서버에서는 같은 IP에서도 여러 도메인을 가질 수 있기 때문에 명시해주어야 정확한 요청이 된다.


- Location: 페이지 리다이렉션 (로케이션 위치로 자동 이동)


- Allow: 허용 가능한 HTTP 메서드 (거의 사용 X)


- Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간 (거의 사용 X)



##### 인증

- Authorization: 클라이언트 인증 정보를 서버에 전달

`Authorization: Basic xxxx`

- WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의

401 Unauthorized 응답과 함께 사용



##### 쿠키


- Set-Cookie: 서버에서 클라이언트로 쿠키 전달 (응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달


> Stateless

- HTTP는 무상태, 비연결 프로토콜
- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다
- 클라이언트와 서버는 서로 상태를 유지하지 않는다


```
set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 GMT; path=/; domain=.google.com; Secure
```


- 사용처: 사용자 로그인 세션 관리, 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송되기 때문에 네트워크 트래픽 추가 유발함 (최소한의 정보만 사용 - 세션 id, 인증 토큰)
- 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage)
- 보안에 민감한 데이터는 저장하면 안된다 (주민번호, 신용카드 번호 등)


> 쿠키 - 생명주기


- Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT
	- 만료일이 되면 쿠키 삭제
- Set-Cookie: max-age=3600 (3600초)
	- 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지


> 쿠키 - 도메인

- `domain=example.org`
- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
	- domain=example.org를 지정해서 쿠키 생성
		- example.org, dev.example.org 도 쿠키 접근
	- 생략하면 example.org 에서만 쿠키 접근 가능


> 쿠키 - 경로

- `path=/home`
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 path=/ 루트로 지정
- path=/home 지정
	- /home 하위는 모두 가능 (이외에는 쿠키 전달 X)


> 쿠키 - 보안

- Secure
	- 쿠키는 http, https를 구분하지 않고 전송
	- Secure를 적용하면 https인 경우에만 전송
- HttpOnly
	- XSS 공격 방지
	- 자바스크립트에서 접근 불가
	- HTTP 전송에만 사용
- SameSite (잘 사용 안해서 지원확인해야함)
	- XSRF 공격방지



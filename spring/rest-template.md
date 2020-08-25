# RestTemplate


스프링 프레임 워크에서 REST로 구성된 서비스의 Endpoint를 호출할 수 있도록 2가지 방법을 제공한다.

- RestTemplate (동기 방식)
- RestClient (비동기 방식)

> RestTemplate은 Spring 3 부터 지원 되었고 REST API 호출 이후 응답을 받을 때까지 기다리는 동기 방식이다. (WebClient로 nonblock, 리엑티브 웹 클라이언트로 동기, 비동기 방식을 지원)

RestTemplate은 스프링에서 제공하는 다른 여러 Template 클래스(JdbcTemplate, RedisTemplate ...)와 동일한 원칙에 따라 설계되어 단순한 방식의 호출로 복잡한 작업을 쉽게 하도록 제공한다. 
RestTemplate은 REST 서비스를 호출하도록 설계되어 HTTP 프로토콜의 메서드(GET, POST ...)에 맞게 여러 메서드를 제공한다.
 
 
정리하자면 Rest Template은

- 기계적이고 반복적인 코드를 줄여준다
- RESTful 형식에 맞춘다
- JSON, XML을 쉽게 응답 받는다


---
### 기존의 HTTP 통신 방법

##### URLConnection

jdk 1.2 부터 (java.net) URL 내용을 읽어오거나, URL 주소에 GET, POST로 데이터를 전달할 때 사용한다. (http 이외에도 사용)
응답을 받기 위한 과정이 복잡하고, 응답 코드가 4xx, 5xx 인 경우 IOException이 발생한다. 또한 타임아웃 설정이나 쿠키 제어가 불가하다는 단점이 있다.


##### HttpClient

URLConnection와 달리 모든 응답코드를 읽을 수 있고, 타임아웃 설정과 쿠키제어가 가능하다. URLConnection 보다 과정이 간결하긴하지만, 여전히 반복적이고 코드가 길다. 
그리고 스트림 처리 로직을 구성하거나 응답 컨텐츠 타입에 따라 별도 로직이 필요한 불편함이 있다.
 
 
---
### RestTemplate 동작원리

RestTemplate은 `org.springframework.http.client` 패키지에 있다. 
HttpClient는 HTTP를 사용하여 통신하는 범용 라이브러리이고 RestTemplate은 HttpClient를 추상화(HttpEntity의 json, xml 등)해서 제공해준다. 
즉, RestTemplate는 json, xml 라이브러리를 사용해서 변환해 주는 것 까지 처리해주기 때문에 기존 방법들 보다 더 편리하다.

![rest template principle](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F99300D335A9400A52C16C1)


1. 어플리케이션이 RestTemplate을 생성하고, URI, HTTP메소드 등의 헤더를 담아 요청한다.
2. RestTemplate 은 HttpMessageConverter 를 사용하여 requestEntity 를 요청메세지로 변환한다.
3. RestTemplate 은 ClientHttpRequestFactory 로 부터 ClientHttpRequest 를 가져와서 요청을 보낸다.
4. ClientHttpRequest 는 요청메세지를 만들어 HTTP 프로토콜을 통해 서버와 통신한다.
5. RestTemplate 은 ResponseErrorHandler 로 오류를 확인하고 있다면 처리로직을 태운다.
6. ResponseErrorHandler 는 오류가 있다면 ClientHttpResponse 에서 응답데이터를 가져와서 처리한다.
7. RestTemplate 은 HttpMessageConverter 를 이용해서 응답메세지를 java object(Class responseType) 로 변환한다.
8. 어플리케이션에 반환한다.


---
### 주요 메서드

- getForObject : 주어진 URL 주소로 HTTP GET 메서드로 결과 객체를 수신
- getForEntity : 주어진 URL 주소로 HTTP GET 메서드로 결과로 ResponseEntity 수신
- postForLocation : POST 요청을 보내고 결과로 헤더에 저장된 URI를 결과로 수신
- postForObject : POST 요청을 보내고 객체로 결과를 수신
- postForEntity : POST 요청을 보내고 결과로 ResponseEntity 수신
- delete : 주어진 URL 주소로 HTTP DELETE 메서드를 요청
- headForHeaders : 헤더의 모든 정보를 얻을 수 있으면 HTTP HEAD 메서드를 사용
- put : 주어진 URL 주소로 HTTP PUT 메서드를 요청
- patchForObject : 주어진 URL 주소로 HTTP PATCH 메서드를 요청
- optionsForAllow : 주어진 URL 주소에서 지원하는 HTTP 메서드를 조회 (OPTIONS)
- **exchange** : HTTP 헤더를 새로 만들수 있고 어떤 HTTP 메서드도 사용 가능
- execute : Request / Response 콜백 수정 가능
 
 
---
### 사용 방법

```java

// 도메인(entity) 객체로 수신
Employee employee = restTemplate.postForObject(BASE_URL + "/employee", request, Employee.class);


// ResponseEntity 객체로 수신
ResponseEntity<Employee> responseEntity = restTemplate.getForEntity(BASE_URL + "/{name}/{country}", Employee.class, params);

```

위 예제와 달리 실전에서는 VO, DTO를 사용해서 객체를 수신하는 것이 바람직하다. 특히 실제 서비스에서는 도메인 영역, 즉 엔티티를 안정된 상태로 유지하는 것이 중요하기 때문이다. 
검증 로직이 엔티티에 위치해도 큰 문제는 없지만, REST 요청을 수신하는 Controller 영역이나 RestTemplate을 사용해 수신 객체를 생성하는 책임을 가진 영역에서 
Spring Validation(@Valid)을 통해 미연에 방지할 수 있다. 또한 불변 객체인 엔티티와 달리 요청 수신부나 외부 API 요청을 통해 생성되는 객체는 자주 변경될 여지가 있기 때문에 이러한 구조를 가져가는 것이 바람직하다.
즉, DTO에 @NotNull, @NotEmpty 등의 Validation 로직을 추가하고, RestTemplate으로 요청하여 생성된 객체를 비즈니스 로직적으로 검증한 후 엔티티로 변환하여 영속하거나 처리하는 형태로 해결 가능하다.
 
 
ResponseEntity를 사용하면 Http 응답에 대한 추가 정보를 받을 수 있어서 실제 데이터 뿐만 아니라 요청에 대한 응답 코드와 같은 정보도 수신 가능하다.
 
 
 
  
---
- [spring rest template](https://advenoh.tistory.com/46)
- [rest template](https://sjh836.tistory.com/141)


# Web Cache 

클라이언트는 웹사이트에 접속할때 html, css, js, 이미지와 같은 정적 컨텐츠를 다운받는다.
정적인 컨텐츠는 대부분 변경이 잦지 않은 파일들이기 때문에 매번 파일을 내려받는 것이 아니라 특정 위치에 복사본을 저장하고 동일한 url 요청에서 리소스를 다시 내려받지 않는다. 
즉, 캐시를 통해 더 빠른 서비스를 제공하기 위함이며 이는 서버의 부하와 응답시간이 단축되기도 하기 때문에 클라이언트와 서버측 모두 이점이 있다.

> 매번 정적인 컨텐츠들을 다운받게 되면 반응성, 사용성이 떨어질수 있기 때문에 해당 웹사이트에서는 미리 어떤것을 얼마나 캐시할지 정한다. 브라우저는 웹사이트가 정한대로 정적 컨텐츠를 캐시함으로써 불필요한 요청을 제거한다. 


### 웹 캐시 종류

- 브라우저 캐시

브라우저 또는 클라이언트 어플리케이션에 의해 내부 디스크에 캐시하며 캐시된 리소스를 공유하지않는한 한 개인에 한정된 캐시이다. 
방문한 페이지를 재방문 하는 경우 효율이 좋다. (뒤로가기)

- 프록시 캐시

브라우저 캐시와 원리는 동일하지만 클라이언트나 서버가 아닌 네트워크 상에서 동작한다. IPS(침입차단시스템)의 방화벽에 설치되어 대기시간, 트래픽 감소, 접근정책, 제한 우회, 사용률 기록 등을 수행한다.
한정된 수의 클라이언트를 위해 무한대의 웹서버 컨텐츠를 캐시한다.

- 게이트웨이 캐시 (리버스/서로게이트 프록시)

서버 앞단에 설치되어 요청에 대한 캐쉬 및 효율적인 분배를 통해 가용성, 신뢰성, 성능등을 향상 시킨다. 
(Encryption / SSL acceleration, Load balancing, Serve/cache static content, Compression등을 수행)
무한대의 클라이언트들에게 한정된 수의 웹서버 컨텐츠를 제공하도록 할수도 있다.



### 캐시 컨트롤 (HTTP Header)

파일이 이전과 비교해 변경되었는가를 체크하는 validation과 캐쉬의 만료 여부를 체크하는 freshness로 구성된다. 
request와 response에 따라 서로 사용될 수 있는 값이 다르고 http1.1로 전환되면서 약간의 변화가 있다. (중복으로 선언되면 1.1에 우위, 호환도 가능)

```
REQUEST 

- validation : If-None-Match
- freshness : Cache-Control

RESPONSE

- validation : Entity Tag (Etag)
- freshness : Cache-Control
```

Cache-Control은 다양한 지시자를 사용해 값을 전달할 수 있다. 

![cache-control](https://github.com/iiaii/memo/blob/master/images/web-cache-CacheControl.png?raw=true)

[Cache-Control 선택지](https://t1.daumcdn.net/cfile/tistory/271252475582250020)



### 캐시 동작 로직

1. 브라우저가 서버에 index.html 요청
2. 서버는 index.html 파일을 찾고 파일에 포함되는 데이터를 header 값과 함께 응답
3. 브라우저는 응답 받은 내용을 브라우저에 표시하고 응답 헤더의 내용에 따라 캐쉬 정책을 수행 
(응답 헤더에 Last-Modified, Etag, Expires, Cache-Control:max-age 항목이 존재한다면 복사본을 생성하고 값을 저장)


- LAST-MODIFIED

1. 브라우저는 최초 응답 시 받은 Last-Modified 값을 If-Modified-Since 헤더에 포함 시켜 페이지를 요청한다
2. 서버는 요청 파일의 수정 시간을 If-Modified-Since 값과 비교하여 동일하면 304 Not Modified로 응답하고 다르다면 200 OK와 함께 새로운 Last-Modified 값을 응답 헤더에 전송한다
3. 브라우저는 응답 코드가 304인 경우 캐쉬에서 페이지를 로드하고 200이라면 새로 다운받은 후 Last-Modified 값을 갱신한다

- ETAG (Entity Tag)

1. 브라우저는 최초 응답시 받은 Etag 값을 If-None-Match 라는 헤더에 포함 시켜 페이지를 요청한다
2. 서버는 요청 파일의 Etag 값을 If-None-Match 값과 비교하여 동일하면 304 Not Modified로 응답하고 다르면 200 OK와 함께 새로운 Etag 값을 응답 헤더에 전송한다
3. 브라우저는 응답 코드가 304인 경우 캐쉬에서 페이지를 로드하고 200이라면 새로 다운받은 후 Etag 값을 갱신한다

- Expires

브라우저는 최초 응답 시 받은 Expires 시간을 비교해 기간내면 서버를 거치지 않고 바로 캐쉬에서 페이지를 로드한다. 만약 기간이 만료되었다면 validation 작업을 수행한다

- Cache-Control

1. 브라우저는 최초 응답 시 받은 Cache-Control 중 max-age 값(초 단위)을 GMT와 비교해 기간내라면 서버를 거치지 않고 캐시에서 페이지를 로드한다.



### 캐시 전략

캐시가 잘 적용되려면 

1. 일관된 URL 사용
2. 자주 바뀌는 파일과 그렇지 않은 파일을 분리해 리소스의 최적 freshness 설정
3. 다운 가능한 파일의 내용이 바뀌면 이름(URL)을 변경 
4. SSL을 최소화 (암호화된 페이지는 캐시되지 않음)

> 3에서 언급한것과 같이 파일을 갱신해주어야하는 경우 URL을 다르게 하면되는데, 이때 URL뒤에 `?version=1` 과 같은 방식으로 버저닝을 통해 캐시를 우회하기도 한다.


실제 서비스에서는 캐시를 사용할때 주의할 점이 많다. 앞서 설명된 속성도 주의해서 사용하지 않으면 서비스가 업데이트 되어 배포되더라도 일관되지 않게 동작하거나 변경된 사항이 적용되지 않을 수 있다.
자세한것은 하단의 링크를 통해 파악!





---
[웹 캐시](https://hahahoho5915.tistory.com/33)

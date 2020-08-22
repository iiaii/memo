# JSONP (JSON with Padding)

웹은 기본적으로 Same-Origin Policy, 동일한 출처 상의 요청만 가능하도록 한다. [Same-Origin Policy & CORS](https://github.com/iiaii/memo/blob/master/common/same-origin-policy%26cors.md) 
하지만 실제 개발을 하거나 서비스 내에서도 외부 API 요청이 필요한 경우가 많기 때문에 Same-Origin Policy를 우회하기 위해 JSONP라는 것이 탄생하게 되었다. 
즉, 각기 다른 도메인에 상주하는 서버로부터 데이터를 요청하기 위해 사용된다. **하지만 여러 보안 문제로 인해 현재 JSONP는 CORS로 대체되었다.**


---
### 원리

Javascript에서 직접적인 요청을 하는 경우에 Cross-Origin 요청이 차단되지만, script 태그에 JSON 데이터를 직접적으로 삽입하면 Cross-Origin Policy에 관계 없이 호출이 가능하다.
하지만 웹 브라우저의 Javascript 엔진은 변수, 상수 정의 등의 특정한 상황 없이 나오는 중괄호 문법을 block으로 해석하기 때문에 Javascript 문법 오류가 발생한다. 
JSONP는 이러한 웹브라우저의 특성을 이용해 JSON 데이터를 클라이언트가 지정한 콜백 함수를 호출하는 유효한 JavaScript 문법으로 감싸 클라이언트에 전송한다. 
따라서 JSONP 요청의 콜백 함수로 지정하는 경우 서버측에서는 JSON 데이터를 padding 하여 클라이언트에 보내기 때문에 콜백함수로 처리가 가능하다.

> script 태그 src에 callback 함수 지정

```html
<script type="application/javascript"
        src="http://server.example.com/Users/1234?callback=parseResponse">
</script>
```

> JSONP를 통해 callback 함수가 다음과 같은 결과를 받는다

```javascript
parseResponse({"Name": "Foo", "Id": 1234, "Rank": 7});
```
 
 
 
 
---
- [wiki JSONP](https://ko.wikipedia.org/wiki/JSONP)
- [jquery jsonp](http://www.codejs.co.kr/jquery-jsonp/)



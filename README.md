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

[서버사이드랜더링](https://d2.naver.com/helloworld/7804182)

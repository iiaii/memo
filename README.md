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

# Spring Validation

- Spring Validation은 크게 3가지 계층에서 가능하다 (Controller, Service, Repository)

### 검증 방법

1. 검증이 필요한 빈의 필드에 @NotBlank, @NotNull 등의 어노테이션을 추가한다
2. @Validated 를 클래스 레벨의 Controller, Service에 추가한다.(Repository의 경우 하이버네이트가 CRUD 할때 검증한다)
3. 메서드의 파라미터 앞이나, 리턴 값 앞에 @Valid 나 @NotBlank, @NotNull 등을 추가하면 해당 구문을 지날때 검증한다. (파라미터 뿐만 아니라 리턴 값 앞에도 @Valid를 붙일 수 있다.)
List<@Valid Dto>, @Valid Dto 와 같은 형태도 가능 (파라미터 리턴 모두 가능)

[Validation](https://jongmin92.github.io/2019/11/18/Spring/bean-validation-1/)

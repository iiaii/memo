# MVC, MVP, MVVM

---
### MVC

![mvc](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMjVfMjUw/MDAxNDkwNDM4NzI4MTIy.4ZtITJJKJW_Nj1gKST0BhKMAzqmMaYIj9PobYJMFD4Ig.xTHT-0qyRKXsA4nZ2xKPNeCxeU2-tLIc-4oyrWq5WBgg.PNG.jhc9639/mvc_role_diagram.png?type=w800)

- Model : Application에서 사용되는 데이터와 데이터 처리 영역 
- View : 객체의 입력, 사용자에게 보여지는 UI 영역
- Controller : 사용자와 데이터 사이에서 입력을 받고 처리하는 영역


**Sequence**

1. 사용자 Action이 Controller로 입력
2. Controller는 사용자의 Action 확인 및 Model 업데이트 (비즈니스 로직)
3. Controller는 Model(데이터)을 보여줄 View를 선택
4. 사용자에게 View를 출력


**Summary**

![complex MVC](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FALrHe%2FbtqBTMSuHfN%2FZlW9i9ET34e90APgCRChk1%2Fimg.png)

View와 Model이 많지 않은 경우 각 영역의 분할로 인해 분업이 가능하고 설계가 잘된다면 확장성, 유연성, 유지보수성이 증가한다. 
하지만 복잡한 화면과 여러 데이터가 사용되는 경우, Model이 다수의 View로 표현될수 있고 View 역시 다수의 Model과 연관 될 수 있기 때문에 Model과 View가 복잡하게 얽히는 상황이 발생한다. 
Model과 View간의 복잡도지만 둘 사이에서 중재하는 것은 Controller이기 때문에 Controller가 기형적으로 비대해지게 된다. 또한 Controller는 View와 라이프 사이클이 강하게 연결되기 때문에 추후에 분리하기도 어렵고 유지보수와 테스트도 힘들어진다.











---
[MVC, MVP, MVVM](https://beomy.tistory.com/43)

[MVC](https://m.blog.naver.com/jhc9639/220967034588)

[MVC, MVP, MVVM 비교](https://magi82.github.io/android-mvc-mvp-mvvm/)

[Flux](https://beomy.tistory.com/44)

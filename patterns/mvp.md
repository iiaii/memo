# MVP (Model - View - Presenter)

![mvp](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSyR4u%2FbtqxC5uVCde%2F61g0uS9KxPRfNGA7IHnZTk%2Fimg.png)

MVP는 MVC에서 파생된 디자인 패턴으로 안드로이드에서 주로 사용한다.
MVC에서 Model과 View의 수가 증가함에 따라 관계가 복잡해지게 되는데, View와 Model을 분리해서 사용하고자 MVP 패턴을 사용한다.
(MVP에서는 비즈니스 로직인 Model을 독립적으로 테스트할 수 있다고 함)

- Model : Application에서 사용되는 데이터와 데이터 처리 영역 (View, Presenter에 의존적이지 않은 독립적인 영역)
- View : 객체의 입력, 사용자에게 보여지는 UI 영역 (발생하는 이벤트를 Presenter에 위임, Presenter는 View의 인터페이스를 가짐)
- Presenter : Controller의 역할과 동일하지만, View와 인터페이스를 통해 연결되기 때문에 뷰와 모델 사이에서 입력을 받고 처리하면서도 Model - View 간의 결합도를 낮춤
 
 
### Sequence

1. 사용자 Action이 View로 입력
2. View는 Presenter에게 데이터 요청
3. Presenter는 Model에게 데이터 요청
4. Model은 Presenter에게 요청받은 데이터를 응답
5. Presenter는 View에게 데이터 응답
6. View가 받은 데이터로 화면 출력


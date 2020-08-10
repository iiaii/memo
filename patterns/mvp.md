# MVP (Model - View - Presenter)

![mvp](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSyR4u%2FbtqxC5uVCde%2F61g0uS9KxPRfNGA7IHnZTk%2Fimg.png)

MVP는 MVC에서 파생된 디자인 패턴으로 안드로이드에서 주로 사용한다.
MVC에서 Model과 View의 수가 증가함에 따라 관계가 복잡해지게 되는데, View와 Model을 분리해서 사용하고자 MVP 패턴을 사용한다.
(MVP에서는 비즈니스 로직인 Model을 독립적으로 테스트할 수 있다고 함)

액티비티와 프래그먼트가 View의 일부로 간주되기 때문에 View는 발생한 이벤트를 Presenter로 전달하는 역할을 한다. Presenter는 Model과 View 사이에서 상호작용을 하면서도 View와 인터페이스로 UI에 필요한 데이터만 전달하게 된다. 데이터를 수신한 View는 스스로 UI를 업데이트 하게 된다.


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


### Summary

Model - View 의 결합도를 낮추기 때문에 새로운 기능 추가 및 변경시 해당 코드만 수정하면 되기 때문에 확장성이 좋아지고 테스트 코드 작성이 수월해진다.
하지만 Application의 복잡도가 올라감에 따라 View와 Presenter 사이가 복잡해지고, Presenter에 비즈니스 로직이 집중되는 경향이 있다.


---
[MVP 패턴](https://beomseok95.tistory.com/212)

[안드로이드 MVP](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-MVP%EA%B0%80-%EB%AD%98%EA%B9%8C)

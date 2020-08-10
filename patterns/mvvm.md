# MVVM (Model - View - View Model)


![mvvm flow](https://res.cloudinary.com/software-crafters/image/upload/v1544443712/posts/patron-mvvm-xamarin-forms/mvvm.png)
![mvvm detail flow](https://media.vlpt.us/images/jojo_devstory/post/5d3e1aa5-28bc-45d3-964f-36e60e4e9088/%EC%BA%A1%EC%B2%98.PNG)

View Model은 View를 표현하기 위해 만들어진 View를 위한 Model 같은 존재다. 
View는 입력을 [Command 패턴](https://github.com/iiaii/gof-design-patterns/tree/master/java-design-patterns/src/%ED%96%89%EB%8F%99%ED%8C%A8%ED%84%B4/command)을 통해 
View Model에 명령을 전달하고 Data Binding으로 View Model의 값이 변화하면 View 정보 역시 변경된다.


- Model : Application에서 사용되는 데이터와 데이터 처리 영역 (View는 View Model을 관찰하고 있다가 상태 변화가 감지되면 화면을 갱신한다)
- View : 객체의 입력, 사용자에게 보여지는 UI 영역 (발생하는 이벤트를 Presenter에 위임, Presenter는 View의 인터페이스를 가짐)
- View Model : View - Model 사이의 매개체로서 View의 비즈니스 로직이 포함되고, View가 Data Binding을 통해 변경이 잘 적용될 수 있도록 Model의 데이터를 가공하여 들고 있는다.


### Sequence

1. 사용자 Action이 View로 입력
2. View는 [Command 패턴](https://github.com/iiaii/gof-design-patterns/tree/master/java-design-patterns/src/%ED%96%89%EB%8F%99%ED%8C%A8%ED%84%B4/command)으로 View Model에 Action 전달
3. View Model은 Model에게 데이터 요청
4. Model은 View Model에게 요청받은 데이터 응답
5. View Model은 응답 받은 데이터를 가공하여 저장
6. View는 View Model과 Data Binding 되어있으므로 변화에 따라 화면에 출력

### Summary 

![mvvm](https://blog.outsider.ne.kr/attach/1/1397658037.png)

View와 View Model은 MVP와 달리 1:N 관계가 될수 있다. (여러개의 Fragment(Component)가 하나의 View Model을 가질 수 있음) 
MVP 패턴의 복잡도가 증가될수록 View와 Presenter의 복잡도 역시 증가하지만, MVVM 패턴은 Command 패턴과 Data Binding을 통해 
View와 View Model 사이의 의존성, View와 View Model 사이의 의존성까지 느슨한 결합을 유지한다. 따라서 각 부분이 매우 독립적이고 모듈화하여 개발이 가능하다. 
하지만 설계하기 어렵고, View가 변수와 표현식 모두에 Binding 될 수 있기 때문에 Presentation 로직이 증가할수 있다. (XML 코드로 보완하지만 이 역시도 유지보수가 어려울 수 있음)



---
[안드로이드 MVVM](https://velog.io/@jojo_devstory/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-MVVM%EC%9D%B4-%EB%AD%98%EA%B9%8C)

[mvc-mvp-mvvm](https://magi82.github.io/android-mvc-mvp-mvvm/)

[mvc mvp mvvm](https://beomy.tistory.com/43)

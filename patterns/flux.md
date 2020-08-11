# Flux

![complex mvc](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile27.uf.tistory.com%2Fimage%2F99A0B5505B5BDFFC169FE1)

MVC 패턴을 사용한 프로젝트에서 프로젝트 규모가 커지고 Model과 View를 재사용하다보면, View와 Model이 복잡하게 얽혀 사실상 데이터 흐름이 뒤엉킨 양방향 관계를 가지게 된다. 
이는 추후 수정이나 확장을 할 때 예측하지 못한 곳에서 같이 변경이 일어나고, 문제가 생겼을 때 어디서 문제가 시작되었는지 예측이 어려워진다.


![flux](https://media.vlpt.us/images/jongsunpark88/post/10f9f95a-1f15-4d34-9702-128d26c87578/flux-architecture.png)

![server flux](https://media.vlpt.us/images/jongsunpark88/post/60102e9c-5bbf-4b83-b523-db9d62e6e68d/flux-architecture-server.png)

Flux는 MVC 패턴이 확장되면서 발생하는 복잡한 데이터 흐름을 예측가능하도록 만들기 위해 Action을 통해서만 데이터의 변경이 일어나도록 단방향 데이터 흐름을 만들었다. 

0. 사용자가 View로 부터 어떠한 입력을 한다.
1. Action이 Application의 상태변경을 위한 데이터 묶음을 만들어 Dispatcher에게 전달한다. (Action 생성자를 통해 메시지(데이터 묶음)를 생성)
2. Dispatcher는 전달받은 Action 객체에 맞게 정의된 콜백함수를 통해 Store로 전달한다. 
3. Store는 Application의 상태를 변경한다.
4. View는 변경된 상태를 감지하고 변경된 상태를 화면에 출력한다. (구체적으로는 Store와 View 사이에 있는 Controller View가 상태 변경 사실을 Store로 부터 전달 받고 View에게 변경된 상태를 넘김)

### 용어 정리

![flux flow](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile10.uf.tistory.com%2Fimage%2F995759505B5BDFFC0CBC09)

- Action(s) : 데이터를 변경하기 위한 시작점으로 데이터 변경을 위한 메시지를 생성하여 Dispatcher에게 전달한다.
- Dispatcher : 전달 받은 Action 객체와 맞는 콜백함수를 통해 메세지를 스토어에 전달한다 (전체 Application에서 한개의 인스턴스만 사용)
- Store : Store는 Application의 상태(state)를 저장하며 모든 상태변경은 Store에 의해 결정된다. 상태변경을 위한 요청을 스토어에 직접 요청할 수 없고 Action, Dispatcher를 통해야만 상태변경이 가능하다.
- View : 변경된 상태를 감지하고 변경된 상태를 화면에 출력한다.


### Flux 구현체 

- Redux (React)

![redux](https://media.vlpt.us/images/jongsunpark88/post/8ffed5ae-b7e0-4adf-ab34-da31c28a2bbb/%EC%A0%9C%EB%AA%A9%20%EC%97%86%EC%9D%8C.png)


- Vuex (Vue)

![vuex](https://vuex.vuejs.org/vuex.png)

Actions (dispatch) > Mutations (commit) > State / Getters (mutate) > Vue Components (Render)


---
[Flux](https://lemontia.tistory.com/637)
[Redux / Flux](https://velog.io/@jongsunpark88/Redux%EC%99%80-Flux-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98)

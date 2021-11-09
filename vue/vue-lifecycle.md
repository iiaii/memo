# Vue 라이프 사이클

![vue-lifecycle](https://kr.vuejs.org/images/lifecycle.png)


### 1. Creatrion

- 컴포넌트가 돔에 추가되기 전의 상태

##### beforeCreate

- data와 events가 세팅되지 않은 상태

##### created

- data와 events 활성화
- 템플릿과 가상돔 마운트
- 렌더링되지 않은 상태


---
### 2. Mounting (DOM 삽입 단계)

- 초기 렌더링 직전 컴포넌트에 직접 접근할 수 있고 서버 렌더링에서 지원 X
- 초기 렌더링 직전에 돔을 변경하고자 할 때 사용
- 컴포넌트 초기 세팅 되어야할 데이터 페치는 created 단계를 사용하는 것이 좋음

##### beforeMount

- 템플릿과 렌더 함수들이 컴파일된 후에 첫 렌더링이 일어나기 직전에 실행
- 대부분 사용하지 않는 것이 좋음 
- SSR 시 호출 X

##### mounted

- 컴포넌트, 템플릿, 렌더링된 돔에 접근 할 수 있음
- 모든 하위 컴포넌트가 마운트된 상태를 보장하지 않음
- SSR 시 호출 X

> 부모 Created -> 자식 Created -> 자식 Mounted -> 부모 Mounted


---
### 3. Updating (Diff 및 재 렌더링 단계)

- 컴포넌트에서 사용되는 반응형 속성들이 변경되거나 재 렌더링되면 실행
- 디버깅이나 프로파일링 등을 위해 컴포넌트 재 렌더링 시점을 알고 싶을때 사용

##### beforeUpdate

- 데이터가 변해서 업데이트 사이클이 시작될때 실행 (돔이 재 렌더링되고 패치되기 직전 실행)
- 재 렌더링 전의 새 상태의 데이터를 얻을 수 있고 더 많은 변경이 가능 (이때의 변경으로 인한 재 렌더링은 없음)

##### updated

- 컴포넌트 데이터가 변해 재 렌더링 후 DOM이 업데이트 완료된 상태이므로 DOM 종속적인 연산이 가능
- 여기서 상태 변경을 하면 무한루프에 빠질 수 있음 (모든 자식 컴포넌트 재 렌더링을 보장하지 않음)


---
### 4. Destruction (해체 단계)


##### beforeDestroy

- 뷰 인스턴스가 제거 되기 직전에 호출
- 이벤트 리스너나 reactive subscription 제거 시 사용
- SSR 시 호출 X

##### destroyed

- 뷰 인스턴스 제거 된 후 호출
- 뷰 인스턴스의 모든 디렉티브가 바인딩 해제
- 모든 이벤트 리스너가 제거
- 모든 하위 뷰 인스턴스도 제거
- SSR 시 호출 X


---
### beforeRouteEnter, beforeRouteUpdate, beforeRouteLeave

- 페이지 이동(진입, 진출)이나 페이지 내의 이동 (파라미터 변경) 시에 동작하도록 하는 라이프사이클
- 페이지 이동이 있을때 마다 데이터를 페치해서 최신성을 유지해줘야 하는 경우에 사용


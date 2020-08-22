# Vue의 단방향 / 양방향 바인딩

### 단방향 데이터 바인딩

Vue는 기본적으로 Vue Instance (= <script> = javascript) >> DOM (= \<template> = html) 으로 향하는 단방향 바인딩이다. 
따라서 `<template>`, `<script>`, `<style>` 형태를 가지는 Vue 싱글 파일 컴포넌트에서 `<script>`의 데이터가 변경되면 `<template>`에 바인딩된 데이터가 같이 변경된다.
단방향 바인딩되어 있기 때문에 반대로 `<template>`에서 데이터가 변경되더라도 `<script>`의 데이터는 변경되지 않는다. 데이터 변경을 위해서는 `<script>`를 통해야만 한다.


```vue
<template>
  <div>
    <button @click="hello">hello</button>
    <button @click="bye">bye</button>
    <div>{{ text }}</div>
  </div>
</template>

<script>
export default {
  data: {
    text: 'hello',
  },
  methods: {
    // bye 버튼 클릭하면 text bye로 변경
    bye() {
      this.text = 'bye';
    },
    // hello 버튼 클릭하면 text hello로 변경
    hello() {
      this.text = 'hello';
    }
  }
}
</script>

<style>

</style>
```

위의 예제의 경우 <template>(뷰)에서 click이 일어나면 method를 통해서 <script>의 data가 변경되고, 단방향으로 데이터 바인딩된 `<div>{{ text }}</div>` 부분이 갱신되게 된다.

- v-bind

```html
<v-flex 
xs12 sm6 
v-for="(content, cardIdx) in contents" 
v-bind:key="content.idx" 
>
    <v-layout wrap>
        <v-flex 
        xs12 
        v-show="content.media"
        >
            
            <Card 
            v-bind:cardIdx="cardIdx"
            />

        </v-flex>
    </v-layout>
</v-flex>
```

기존 html 태그의 속성에 `<Card v-bind:cardIdx="cardIdx"/>`와 같이 `v-bind`혹은 `:`로 해당 속성의 값을 <script>의 data, computed, methods 등과 단방향으로 바인딩 가능하다.
html 태그에서 `v-`로 시작하는 태그 속성도 기본적으로 단방향 바인딩 된다.  

- 컴포넌트 간의 단방향 데이터 바인딩 (props)

Vue는 컴포넌트간의 상위, 하위 속성 사이에서도 단방향 바인딩을 형성한다. 모든 props는 상위 속성이 업데이트되면 하위로 흐르지만 그 반대는 안된다. 
이로써 데이터 흐름을 예측 가능하도록 만들 수 있다. 하위에서 상위로 데이터를 보내기 위해 emit과 같은 eventUp이나 이벤트 버스를 사용하는 방법이 있지만, 
재사용이 많아지는 복잡한 컴포넌트 구조에서는 수정과 확장, 유지보수 모두 어렵게 된다. 
따라서 [Flux 패턴](https://github.com/iiaii/memo/blob/master/patterns/flux.md)과 유사한 구조의 Vuex를 사용해 컴포넌트간 통신을 단방향 흐름으로 제어하도록 만들어야 한다.




### 양방향 데이터 바인딩

- v-model

```html
<v-text-field 
class="questrial" 
height="45px" 
append-icon="fas fa-sign-in-alt"
@click:append="addCommentClicked"
@keyup.enter="addCommentClicked"
v-model="newComment"
background-color="grey lighten-2" 
:placeholder="isUserLoggedIn ? '댓글을 입력하세요...' : '로그인이 필요합니다'" 
rounded
:label="textInputMetaLabel"
>
</v-text-field>
```

v-model은 v-bind와 v-on의 조합으로 양방향 데이터 바인딩을 가능하게 한다. 
예제 코드와 같이 입력창, Radio 버튼, Check box 등에 주로 사용되며 `<script>`에서 변경하더라도 데이터가 변경되고 `<template>`에서 변경하더라도 데이터가 변경된다.



---
- [컴포넌트 단방향 데이터 흐름](https://kr.vuejs.org/v2/guide/components.html#%EB%8B%A8%EB%B0%A9%ED%96%A5-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%9D%90%EB%A6%84)
- [Vue data-binding](https://adrian0220.tistory.com/176)
- [Vue Two-way-binding](https://medium.com/@hozacho/%EC%96%91%EB%B0%A9%ED%96%A5-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B0%94%EC%9D%B8%EB%94%A9-v-model-two-way-binding-vuejs-directive-%EB%A7%A8%EB%95%85%EC%97%90-vuejs-43b37de2633f)

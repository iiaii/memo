# Vue $mount vs el 차이점

```
new Vue({
    data () {
        return {
            text: 'Hello, World'
        };
    }
}).$mount('#app')
```

```
new Vue({
    el: '#app',
    data () {
        return {
            text: 'Hello, World'
        };
    }
})
```

위의 2가지는 동일하게 동작하지만 약간의 차이점이 있다.

$mount 의 경우 필요할때 Vue 인스턴스를 명시 적으로 마운트 할 수 있지만 el 방식은 불가능하다.
(페이지에 특정 요소가 존재하거나 일부 비동기 프로세스가 완료 될 때까지 vue 인스턴스의 마운트를 지연시킬 수 있다)
또한 요소 선택기없이 호출되고 이로 인해 Vue 모델이 문서와 별도로 렌더링되서 나중에 추가 할 수 있다.

- $mount()을 사용하면 필요한 요소를 마운트 할 수있는 유연성이 향상됨
- 인스턴스가 마운트 될 요소 (동적으로 생성 된 요소 일 수도 있음)를 실제로 알기 전에 인스턴스를 인스턴스화해야하는 경우 나중에 vm.$mount()을 사용하여 마운트 할 수 있음

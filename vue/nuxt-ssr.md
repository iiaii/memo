# Nuxt.JS (SSR)


###  SSR (Server Side Rendering)

SSR은 고전적으로 사용되던 웹데이터의 서빙 방식인데 모든 웹 정보들을 서버에서 처리해서 보내주기 때문에 브라우저는 요청 받은 데이터를 그리기만 하면 된다. 
따라서 초기 구동 속도가 빠르고 완성된 HTML이 넘어오기 때문에 SEO 역시 가능하다.

하지만 SSR은 페이지 이동이나 웹페이지에서 일부 기능이 처리될때 페이지가 새로 로딩되어야 할 수도 있다. 
이는 서버에 불필요하고 반복되는 템플릿 연산을 야기할 수 있고 사용자 경험도 만족스럽지 못하다. 
또한 모바일과 달리 템플릿팅 작업이 추가적으로 필요하며 프론트에서 의미하는 재사용 가능한 컴포넌트(html, css, js 가 결합되어 재사용 가능한 형태)로 구성하여 개발 생산성을 높이는 것이 어렵다. 



### CSR (Client Side Rendering)

CSR에서 SPA (Single Page Application) 는 초기 렌더링 시 내용이 없는 HTML을 받고 이후 자바스크립트로 웹 페이지를 동작시킨다. 
필요한 부분만 동적으로 처리가 가능하기 때문에 자연스러운 사용자 경험을 주고 서버가 템플릿 연산을 하지 않아도 된다. 
또한 서버는 모바일과 웹에 동일하게 API로 지원하면 되고 프론트에서는 컴포넌트로 구분지어 개발 생산성을 높일 수 있다.

하지만 CSR의 SPA는 브라우저에서 처리되는 만큼 초기 구동 속도가 느릴 수 있고 SEO(검색엔진최적화)가 어려운 단점이 있다. 
웹 페이지 검색엔진이 자료를 수집하고 검색결과의 상위에 노출되어야하는 페이지는 매우 치명적일 수 있다.



### 프리랜더링과 하이드레이션


SPA의 단점을 보완하고자 SSR의 장점을 활용하는데 이때 프리렌더링과 하이드레이션이라는 과정이 필요하다.



프리렌더링이란 기존 SPA를 서버에서 브라우저로 전송할 수 있도록 어느정도 완성된 HTML 파일을 만드는 동작을 의미한다.
라우팅 시 자바스크립트로만 동작하는 기존 SPA와 달리 빌드 결과물에 서버가 요청에 맞는 페이지 컴포넌트들을 프리랜더링해서 브라우저에게 제공하는 방식으로 라우팅을 하게한다.



하이드레이션이란 프리랜더링 과정을 마치고 브라우저로 전달된 HTML 파일에 남은 자바스크립트 코드를 실행하는 동작을 뜻한다. 
하이드레이션으로 인해 SSR 앱은 기존의 SPA와 동일한 동작과 반응성을 보장할 수 있다. 용어 그대로 불완전한 HTML이라는 마른 땅에 자바스크립트라는 물을 뿌리는 일이다.


> 프리랜더링과 하이드레이션을 거치는 SPA는 브라우저가 접근해서 실행시킬 수 있는 자원이 아니기 때문에 SSR과 같이 항상 대기하고 있는 서버가 필요하다.



### Nuxt.js


Nuxt 는 간편하게 vue를 ssr로 제공하도록 도와주는 프레임워크이다. 
폴더를 통해 편리하게 라우팅되는 부분이나 seo를 위한 헤더 추가가 간편하지만, 백엔드로 사용하는 Node 쪽을 커스텀하기 어려운 문제가 있다. 
캐싱이나 외부 api를 통해 데이터를 조합하는 로직 추가가 자유롭지 못하기 때문에 실서비스에서는 잘 사용하지 않는 상황이다. 
(추후 개선이 될수도..)

typescript + node + vue 조합으로 개발하는 경우가 많은데, node 쪽을 엔드포인트를 커스텀 할 수 있고 다른 데이터와 vue의 번들링 된 파일을 조합해서 컨트롤이 가능하다.
또한 프론트와 백에서 사용하는 도메인 같은 부분을 공유할수도 있다. 이외에도 node와 typescirpt의 이점을 모두 가져갈 수 있다. 



##### 프로젝트 세팅 및 실행


- 프로젝트 세팅 : `npx create-nuxt-app [프로젝트명]`
- 프로젝트 실행 : `npm run dev' 로 실행



##### pages

pages 하위 폴더에서 폴더를 생성하고 해당 폴더에서 `index.vue` 파일을 생성하면 루트부터 라우팅이 적용된다. (.nuxt 의 `router.js`에서 디렉토리 구조에 맞게 라우팅된 코드가 생성된다)


- example.com/hello/word 의 페이지 구성이라면
```
pages폴더 (/) 
  > hello 폴더 
    > index.vue 파일 (/hello)
    > world 폴더 
      > index.vue 파일 (/hello/world)
```


##### nuxt-link


`<nuxt-link to="/blog">Blog</nuxt-link>`


nuxt-link 태그는 SPA 동작처럼 페이지 이동을 하도록 하는 태그이다. 화면의 깜빡임과 지연시간 없이 SPA 처럼 즉시 페이지 이동이 이루어진다. 
이때 즉시 페이지 이동이 이루어질 수 있도록 nuxt-link의 to에 해당 페이지에 필요한 자바스크립트 코드를 미리 로딩하는데, 현재 브라우저 화면에 보이는 데이터만 미리 로딩하도록 되어있다. [Prefetching]
(스크롤으로 노출되는 순간 해당 페이지의 자바스크립트 코드가 로딩됨) -> Bandwidth 절약

> methods 안에 함수를 넣고 이벤트를 걸어서 `this.$router.push('/blog');`를 통해 페이지 이동이 이루어지도록 할 수도 있다.



##### Html Head

`nuxt.config.js`의 head 안에 내용을 추가하면 모든 HTML 헤더에 내용이 추가되고
vue 파일 안의 script에서 head 를 추가해도 동작하며 vue 파일에서 지정하는 것이 우선순위가 더 높다.

```vue
export default {
  head: {
    title: '여기는 메인 페이지'
  },
  ...
 }
```


##### Validate Method

vue 파일 중에서 라우팅이 필요없는 작은 단위의 컴포넌트 파일도 존재한다. 
이때 vue 파일안에서 validate 메서드의 반환 값을 false로 하면 브라우저에서 렌더링하지 않는다.

```vue
export default {
  validate() {
    console.log('on the server');
    return false;
  }
  ...
 }
```

validate 메서드는 최초 접근시 브라우저(클라이언트) 상에서 딱 한번 실행되고 이후에는 SSR이 이루어지는 서버에서만 실행된다.
따라서 브라우저 콘솔에서 `on the server` 가 한번 출력되고 리프레시하거나 다시 접근하더라도 해당 메서드는 다시 실행되지 않는다.
(즉, validate 메서드는 최초 접근시 브라우저에서 한번만 실행되고 이후 접근에는 SSR을 제공하는 서버 측에서만 실행된다.)









---
- [SSR vs CSR](https://medium.com/aha-official/%EC%95%84%ED%95%98-%ED%94%84%EB%A1%A0%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EA%B8%B0-1-spa%EC%99%80-ssr%EC%9D%98-%EC%9E%A5%EB%8B%A8%EC%A0%90-%EA%B7%B8%EB%A6%AC%EA%B3%A0-nuxt-js-cafdc3ac2053)
- [Nuxt SSR](https://maxkim-j.github.io/posts/nuxt-ssr)
- [google ssr, csr, prerendering, (re)hydration](https://developers.google.com/web/updates/2019/02/rendering-on-the-web?hl=en)
- [Nuxt Tutorial](https://www.youtube.com/watch?v=UDUP5NfX7FU)
- [Nuxt.js-vs-Vue.js-SSR-시작하기](https://velog.io/@bluestragglr/Nuxt.js-vs-Vue.js-SSR-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)
- [Nuxt.js ssr](https://www.youtube.com/watch?v=8o-TVh6AiZY)

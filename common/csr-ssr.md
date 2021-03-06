# CSR / SSR

- SPA 전환
![spa](https://d2.naver.com/content/images/2020/06/step1.png)

- SSR 전환
![ssr](https://d2.naver.com/content/images/2020/06/step2.png)

CSR은 페이지 로드후 동적으로 컨텐츠를 생성하기 때문에 페이지가 노출되는 과정이 길다. 

SSR은 서버에서 사용자에게 보여줄 페이지를 모두 구성하여 사용자에게 페이지를 보여주는 방식으로 JSP/Servlet의 아키텍처 역시 이 방식을 사용했다.

페이지를 구성하는 속도는 늦지만 컨텐츠 구성이 완료되는 시점은 빨라지고, SEO(search engine optimization) 또한 쉽게 구성할 수 있다. 


---
### 클라이언트 사이드 랜더링 / 서버사이드 랜더링 (CSR / SSR)

CSR은 SPA의 처음 모습으로 필요한 부분만 서버로 부터 JSON 같은 포맷으로 받아 트래픽이 감소한다.
또한 작은 데이터 변경마다 페이지를 새로고침할 필요없이 네이티브 앱과 비슷한 수준의 인터렉티브한 사용자 경험을 제공한다.
하지만 자바스크립트 엔진이 돌아가지 않으면 원하는 정보를 표시해주지 못하기 때문에 검색엔진 크롤러가 데이터를 제대로 수집하지 못한다.
(SEO - 검색엔진최적화)

SSR은 SEO가 가능하며 첫 랜더링된 html을 클라이언트에 전달해주기 때문에 초기 로딩 속도를 줄일수 있다.
하지만 SPA에서 SSR을 적용하는 것은 복잡하고 성능악화 여지가 있다. (렌더링하는 동안 이벤트루프가 막힘 - 동기적으로 실행)
메타태그만 넣어주거나, prerender를 통해 이 문제를 해결할 수도 있다.


---
- [서버사이드랜더링](https://d2.naver.com/helloworld/7804182)
- [CSR / SSR](https://velog.io/@rjs1197/SSR%EA%B3%BC-CSR%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
- [CSR / SSR 2](https://velog.io/@zansol/%ED%99%95%EC%9D%B8%ED%95%98%EA%B8%B0-%EC%84%9C%EB%B2%84%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81SSR-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%EC%82%AC%EC%9D%B4%EB%93%9C%EB%A0%8C%EB%8D%94%EB%A7%81CSR)

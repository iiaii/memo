# Template Engine

템플릿 양식과 특정 데이터 모델에 따른 입력 자료를 합성해 결과 문서를 출력하는 소프트웨어(혹은 컴포넌트)를 말한다. 그중 웹 템플릿 엔진은 웹 문서가 출력되는 엔진을 말하며 웹 템플릿 엔진은 웹 템플릿들과 웹 컨텐츠 정보를 처리하기 위해 설계되었다. 

![Template Engine](https://gmlwjd9405.github.io/images/etc/template-engine-system.png)

### 종류

- 레이아웃 템플릿 엔진 (Apache Tiles, Sitemesh)

중복되는 코드를 사용하지 않고 지정된 페이지 레이아웃에 따라 페이지 타일을 조합해 완전한 페이지로 만들어준다. 

- 텍스트 템플릿 엔진 (Freemarker, Thymeleaf, Mustache, JSP)

템플릿 양식에 적절한 특정 데이터를 넣어 결과 문서를 출력한다. 

-> 역할이 다르며 섞어서 사용한다 (서로 배타적인 것이 아님)

---
- 서버 사이드 템플릿 엔진

![서버 사이드 템플릿 엔진](https://gmlwjd9405.github.io/images/etc/template-engine-server-side.png)

서버에서 DB 혹은 API에서 가져온 데이터를 미리 정의된 Template에 넣어 html을 그리고 클라이언트에 전달하며 클라이언트는 그대로 그린다.
고정적인 부분을 제외하고 동적으로 생성되는 부분만 템플릿 특정 장소에 끼워 넣어 데이터를 생성하고, 클라이언트는 그리기만 한다.

Javascript : EJS, Jade, Handlebars
Java : Freemarker, Thymeleaf, Mustache ...

- 클라이언트 사이드 템플릿 엔진

![클라이언트 사이드 템플릿 엔진](https://gmlwjd9405.github.io/images/etc/template-engine-client-side.png)

html 형태로 코드를 작성할 수 있으며, 동적으로 DOM을 그리게 해주는 역할을 한다. 클라이언트가 데이터를 받아서 DOM 객체에 동적으로 그려주는 프로세스를 담당한다. 
랜더링이 끝난뒤 서버 통신 없이 화면 변경이 필요할 경우나 단일 화면에서 특정 이벤트를 따라 화면이 계속 변경되어야하는 경우에 필요하다.


Spring 템플릿 엔진은 Java Object에서 데이터를 생성하여 Template에 넣어주면 템플릿 엔진에서 Template에 맞게 변환하여 html 파일을 생성하는 역할을 한다.
Spring Template Engine : JSP, Thymeleaf, Freemarker, Mustache ...
(Freemarker는 업데이트가 안되고 있어서 Spring Boot에서는 권장하지 않는다고 함)

[템플릿 엔진](https://gmlwjd9405.github.io/2018/12/21/template-engine.html)

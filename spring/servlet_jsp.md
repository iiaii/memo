# Apache Tomcat, Servlet, JSP


### 서버 구성


- 단일 WEB 구성

![web](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwyvXB%2FbtqBXGdDIxJ%2F6DJ2vWwEL3HZB0yKsVLdQk%2Fimg.png)

초기의 웹에서는 정적인 컨텐츠를 주로 소비했기 때문에 웹서버는 요청에 따라 html, css, javascript를 보내주고 브라우저가 이를 보여주는 역할을 했다.


- WEB - WAS 구성

![web - was](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdTAp1P%2FbtqBWZ5EcFb%2FihoaBluuLg5dKKGh4qx6K0%2Fimg.png)


시간이 지나 복잡하고 다양한 요구의 페이지 수요가 생기면서 복잡한 로직을 구성해서 모든 페이지를 미리 만들어 놓아야 했고, 요청에 따라 동적으로 응답해줄 필요가 생겼다. 

이를 해결하기 위해 웹서버 뒤에 WAS를 구성하여 복잡하고 동적인 로직을 WAS에서 처리할 수 있도록 하고, 기존 처럼 정적인 컨텐츠는 WEB에서 처리하도록 역할을 분담했다.

- WEB - WAS -DB 구성

![web - was - db](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDYzzI%2FbtqBXFMA24b%2FbzghXZdkQ7R7KVU5KGBpbK%2Fimg.png)

사용자와 서버간의 데이터를 주고 받는 일이 많아지며 서버측에서 데이터를 저장해야하는 일이 많아졌다. 

WAS 뒤에 DB를 구성함으로써 보안도 강화하고 역할을 분담하여 복잡한 요청을 빠르게 처리할 수 있도록 하였다.



### Apache Tomcat

- Apache : WEB 서버
- Apache Tomcat : 경량 WEB + WAS (Web Application Server)

![tomcat](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCdcOO%2FbtqBYeufaVN%2FrsRTfgKQUPp7zJGkId8hrk%2Fimg.png)

Tomcat은 WEB/WAS 기능을 가진 자바 어플리케이션이다. (Java EE 기반으로 만들어 졌다) WAS는 자바로 만들어진 JSP와 Servlet을 구동하기 위한 서블릿 컨테이너 역할을 수행한다. 

컨테이너란 JSP를 서블릿으로 바꿔서 실행해주는 역할과 서블릿의 생명주기를 관리하며 웹 환경에서 서블릿이 구동될 수 있도록 해주는 프로그램이다. 

WAS의 컨테이너는 웹서버에서 보내준 요청을 가지고 스레드를 생성한 후, 필요한 jsp나 servlet 파일을 구동해서 로직을 수행하게 한 뒤 결과로 생성된 응답 객체를 웹서버에 보내주는 역할을 한다.


### JSP, Servlet

##### Servlet

- .java 파일
- 자바의 일반적인 클래스와 동일한 개념
- 웹을 다룰 수 있도록 해주는 HttpServlet 클래스를 상속받은 클래스를 의미

```java
// 서블릿으로 출력하는 html 코드
writer.println("<html>");
		writer.println("<head>");
		writer.println("</head>");
		writer.println("<body>");
		writer.println("<h1>helloWorld~</h1>");
		writer.println("name : " + request.getParameter("name") + "<br/>");
		writer.println("id : " + request.getParameter("id") + "<br/>");
		writer.println("pw : " + request.getParameter("pw" + "<br/>"));
		writer.println("major : " + request.getParameter("major") + "<br/>");
		writer.println("protocol : " + request.getParameter("protocol") + "<br/>");
		writer.println("</body>");
		writer.println("</html>");
		writer.close();
```

위의 코드 처럼 html 코드를 생성할 수 있지만, 생성이라기 보다 출력에 가깝다. 

또한 복잡한 레이아웃을 가진 페이지 구성에도 맞지 않기 때문에 html 형태에 java 코드를 삽입하는 JSP가 탄생하게 된다. 



##### JSP

- .jsp 파일
- Java Server Page (Thymeleaf, Freemarker, Mustache)
- html 문서 형태에 자바 언어를 삽입해 사용할 수 있음


```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>

<%-- 자바 코드 삽입 --%>
<% 
	for(int i = 0; i < 10; i++) {
		out.println(i);
	}
%>

</body>
</html>
```

![jsp 변환과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbB2QJ9%2FbtqBVLG2TJd%2FM2ed9929KE673khLjvpYV1%2Fimg.png)

JSP는 Servlet 파일로 변환되고 컴파일을 통해 클래스파일이 된 후 최종적으로는 html로 변환되어 사용자에게 전송된다. 


---
[Tomcat(톰캣), JSP, Servlet(서블릿)의 기본 개념 및 구조](https://codevang.tistory.com/191)

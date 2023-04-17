---
title: Spring Container
date: 2023-04-10 09:00:00 +0800
categories: [spring]
tags: [spring]
---

[Servlet](https://sangwoong12.github.io/posts/servlet/)

# Dispatcher Servlet

- Spring MVC 프레임워크의 핵심 구성 요소중 하나로, 클라이언트 요청을 처리하고 적절한 Spring Controller로 라우터하는 역할을 한다.


```java
public class MyServlet extends HttpServlet {
  @Override
  protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  }
}
```
- JavaEE 웹서비스를 직접 구현할때에는 위와 같이 Servlet 인터페이스를 구현하여 직접 만들어 사용하였다.
- 하지만 spring 에서는 Dispatcher Servlet 이라는 모든 요청을 당담하는 서블릿을 두고 컨트롤러에 위임을 하여 요청을 처리한다.
- 그렇기 때문에 프론트 컨트롤러 디자인 패턴이 적용된 SpringMVC 를 통해 개발자는 별도의 서블릿 개발없이, Controller 의 구현만으로도 동적인 response 를 클라이언트에게 줄 수 있다.

## DispatcherServlet.java

<img src="/images/spring-container/dispatcher-servlet-cycle.png">

- HandlerMapping : request 와 매핑된 Controller 가 있는지 확인 만약 없을 경우 정적 자원을 찾아 리턴
- HandlerAdapter : 매핑 대상 Controller 에게 Request 처리요청을 보낸다.
- ViewResolver : Controller 가 view 를 return 했을때 해당 view 를 찾아 반환


## 프론트 컨트롤러 패턴
- 서블릿의 URL 맵핑을 하기 위해 web.xml 을 통해 등록하고 사용하였다.
- 위에서 말했듯 Dispatcher Servlet 이 모든 요청을 받아 핸들링하고 공통작업을 처리하기 때문에 프론트 컨트롤러 패턴이다.

# Spring Container

<img src="/images/spring-container/spring-container-cycle.png">

- Servlet Context
  - Servlet 단위로 생성되는 context
  - Spring에서 servlet-context.xml 파일은 DispatcherServlet 생성 시에 필요한 설정 정보를 담은 파일 (Interceptor, Bean생성, ViewResolver등..)
  - URL설정이 있는 Bean을 생성 (@Controller, Interceptor)
  - Application Context를 자신의 부모 Context로 사용한다.
  - Application Context와 Servlet Context에 같은 id로 된 Bean이 등록 되는 경우, Servlet Context에 선언된 Bean을 사용한다.
- Application Context (Root Context)
  - Web Application 최상단에 위치하고 있는 Context
  - Spring에서 ApplicationContext란 BeanFactory를 상속받고 있는 Context
  - Spring에서 root-context.xml, applicationContext.xml 파일은 ApplicationContext 생성 시 필요한 설정정보를 담은 파일 (Bean 선언 등..)
  - Spring에서 생성되는 Bean에 대한 IoC Container (또는 Bean Container)
  - 특정 Servlet설정과 관계 없는 설정을 한다 (@Service, @Repository, @Configuration, @Component)
  - 서로 다른 여러 Servlet에서 공통적으로 공유해서 사용할 수 있는 Bean을 선언한다.
  - Application Context에 정의된 Bean은 Servlet Context에 정의 된 Bean을 사용할 수 없다.

# 정리

<img src="/images/spring-container/spring-container.png">

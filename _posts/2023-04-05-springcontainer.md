---
title: Spring Container
date: 2023-04-10 09:00:00 +0800
categories: [spring]
tags: [spring]
---

[Servlet](https://sangwoong12.github.io/posts/servlet/)

# Dispatcher Servlet
- 디스패처 서블릿은 HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)라고 할수있다.

---
<img src="/images/spring-container/was.png">

- JavaEE 웹서비스를 직접 구현할때에는 Servlet 인터페이스를 구현하여 직접 만들어 사용하였다.

<img src="/images/spring-container/spring-container.png">

- 하지만 spring 에서는 위와 같이 Dispatcher Servlet 이라는 모든 요청을 당담하는 서블릿을 두고 컨트롤러에 위임을 하여 요청을 처리한다.
- 그렇기 때문에 프론트 컨트롤러 디자인 패턴이 적용된 SpringMVC 를 통해 개발자는 별도의 서블릿 개발없이, Controller 의 구현만으로도 동적인 response 를 클라이언트에게 줄 수 있다.

## DispatcherServlet.java
- HandlerMapping : request 와 매핑된 Controller 가 있는지 확인 만약 없을 경우 정적 자원을 찾아 리턴
- HandlerAdapter : 매핑 대상 Controller 에게 Request 처리요청을 보낸다.
- ViewResolver : Controller 가 view 를 return 했을때 해당 view 를 찾아 반환

## 동작방식

<img src="/images/spring-container/dispatcher-servlet.png">

## 프론트 컨트롤러 패턴
- 서블릿의 URL 맵핑을 하기 위해 web.xml 을 통해 등록하고 사용하였다.
- 위에서 말했듯 Dispatcher Servlet 이 모든 요청을 받아 핸들링하고 공통작업을 처리하기 때문에 프론트 컨트롤러 패턴이다.

# Spring Container

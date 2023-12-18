---
title: 면접 질문
date: 2022-12-18 18:00:00 +0800
categories: [database]
tags: [database]
---

## 형식적인 질문

<details>
  <summary>REST Api 란?</summary>
<p>
REST(RESTful, Representational State Transfer, RESTful, 레스트풀) API는 REST 아키텍처 스타일의 제약 조건을 준수하고 RESTful 웹 서비스와 상호 작용할 수 있도록 하는 애플리케이션 프로그래밍 인터페이스(API 또는 웹 API)이다.

자원(Resource):

URI 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.

행위(Verb):

HTTP Method HTTP 프로토콜의 Method를 사용한다.
HTTP 프로토콜은 GET, POST, PUT, DELETE 와 같은 메서드를 제공한다.

표현(Representation of Resource)

Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
</p>
</details>
<details>
  <summary>ACID란?</summary>
<p>
원자성(Atomicity): 하나의 트랜잭션에 속한 모든 작업이 전부 성공하거나 전부 실패해서 결과를 예측할 수 있어야 한다.

일관성(Consistency): 트랜잭션 이후의 데이터베이스의 상태는 이전과 같이 유효(데이터베이스의 제약/규칙 (not null 등)을 만족해야 한다는 뜻)해야 한다.

고립성(Isolation): 모든 트랜잭션은 다른 트랜잭션으로부터 독립되어야 한다는 뜻이다.

지속성(Durability): 수행을 성공적으로 완료한 트랜잭션은 변경한 데이터를 영구히 저장해야 한다. 실패하더라고 로그는 영구적으로 남아있어야 한다.

</p>
</details>

<details>
  <summary>MVC 패턴이란?</summary>
<p>
MVC 패턴은 디자인 패턴중 하나로 Modal, View, Controller 세 가지 역할로 구분한 개발 방법론입니다

모델: 데이터와 비즈니스 로직을 관리합니다.

뷰: 레이아웃과 화면을 처리합니다.

컨트롤러: 모델과 뷰로 명령을 전달합니다.</p>
</details>

<details>
  <summary>객체지향 프로그래밍이란?(OOP란?)</summary>
<p>
객체 지향 프로그래밍 (Object-Oriented Programming, OOP)은 프로그래밍에서 필요한 데이터를 추상화 시켜 상태와 행위를 가진 객체로 만들고, 객체들간의 상호작용을 통해 로직을 구성하는 프로그래밍 방법이다.

특징으로, 추상화, 상속, 다형성, 캡슐화 가 있다.

또는,

절차지향 프로그래밍은 말 그대로 실행하는 절차를 정해 프로그램하는 방법으로 객체 지향은 상태 및 행위를 가진 객체를 통해 상호작용하여 로직을 구성하는 프로그래밍 방법이다.

</p>
</details>

<details>
  <summary>객체란?</summary>
<p>
객체는 프로그램에서 사용되는 데이터 또는 식별자에 의해 참조되는 공간을 의미하며 값을 저장 할 변수와 작업을 수행 할 메소드를 서로 연관된 것들끼리 묶어서 만든 것을 객체라고 할 수 있다.
</p>
</details>

<details>
  <summary>SOLID란?</summary>
<p>
단일 책임 원칙 (Single responsibility principle) 한 클래스는 하나의 책임만 가져야 한다.

개방-폐쇄 원칙 (Open/closed principle) 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

리스코프 치환 원칙 (Liskov substitution principle) 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

인터페이스 분리 원칙 (Interface segregation principle) 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.

의존관계 역전 원칙 (Dependency inversion principle) 추상화에 의존해야지, 구체화에 의존하면 안된다.
</p>
</details>

<details>
  <summary>트랜잭션이란?</summary>
<p>
트랜잭션이란 질의(query)를 하나의 묶음 처리해서 Rollback, commit을 하는 실행 단위를 의미합니다.즉, 한 번 질의가 실행되면 질의가 모두 수행되거나 모두 수행되지 않는 작업수행의 논리적 단위입니다.
</p>
</details>

<details>
  <summary>DOM 이란?</summary>
  <p>
DOM이란 Document Object Model의 약어이다. DOM은 HTML, XML 문서의 프로그래밍 인터페이스라고 할 수 있다. 문서의 구조화된 표현을 제공함으로써 프로그래밍 언어가 DOM 구조에 접근할 수 있도록 해주는 매우 중요한 역할이다. 개발자는 DOM 구조에 접근하여 문서 구조를 바꾸거나 스타일과 내용 등을 변경하고 이벤트를 연결시키는 등 다양한 작업을 수행할 수 있다.
</p>
</details>

<details>
  <summary>Fragment란?</summary>
<p>
Fragment는 DOM(Document Object Model)의 일부분을 나타내는 작은 단위를 말합니다. 이는 여러 노드들을 담고 있는 "가상"의 컨테이너로, 실제 DOM에 연결되지 않은 상태로 미리 조작하고, 한 번에 실제 DOM에 추가함으로써 효율적인 작업을 수행할 수 있게 해준다.
</p>
</details>

<details>
  <summary>가상 돔이란?</summary>
<p>
실제 DOM의 추상화된 버전으로, 실제 DOM의 경우 변경 사항이 있을때 마다 직접 업데이트를 했지만, 가상 돔을 통해 변경사항을 전체 DOM에 즉시 업데이트 하지않고 이전 상태와 차이를 비교하여 최소한의 변경만을 실제 DOM에 적용시켜 성능을 향상시킨다.
</p>
</details>

<details>
  <summary>렌더링이란?</summary>
<p>
</p>
</details>

## Java

<details>
  <summary>Atomic 객체란?</summary>
  <p>atomic 변수는 멀티 스레드 환경에서 원자성을 보장하기 위해 나온 개념이다. synchronized와는 다르게 blocking이 아닌 non-blocking하면서 원자성을 보장하여 동기화 문제를 해결한다. atomic의 핵심 동작 원리는 CAS(Compare And Swap) 알고리즘이다.</p>
</details>

<details>
  <summary>blocking vs non-blocking</summary>
  <p>blocking은 요청한 작업을 마칠 때까지 계속 대기한다. non-blocking은 요청한 작업을 즉시 마칠 수 없다면 즉시 return한다.</p>
</details>

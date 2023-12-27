---
title: 면접 질문
date: 2022-12-18 18:00:00 +0800
categories: [database]
tags: [database]
---

## 형식적인 질문

<details>
  <summary>spring 삼각형</summary>
<p>
DI , AOP , PSA
AOP : 스프링은 관심사 분리 기능을 제공한다. AOP , Interceptor
DI : 의존성 주입으로 스프링은 생성자, 필드, setter 주입 방식을 지원한다.
이런 DI 작업을 개발자가 하는 것이 아닌 스프링이 하기 때문에 Ioc Di 라고 부른다.
PSA : 환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하는 추상화 구조
</p>
</details>
<details>
  <summary>REST Api 란?</summary>
<p>
REST(Representational State Transfer API)란 HTTP URI를 통해서 자원(resource)을 명시하고, HTTP Method(GET, POST, PUT, DELETE, PATCH 등등)을 통해서 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.
이러한 REST 원리를 따르는 API를 RESTful API라고 한다.<br/>

자원(Resource):<br/>
URI 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.<br/>
자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.<br/>
Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.<br/>
행위(Verb):<br/>
HTTP Method HTTP 프로토콜의 Method를 사용한다.<br/>
HTTP 프로토콜은 GET, POST, PUT, DELETE 와 같은 메서드를 제공한다.<br/>
표현(Representation of Resource)<br/>
Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.<br/>
REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.<br/>
JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.
</p>
</details>

<details>
  <summary>ACID란?</summary>
<p>
원자성(Atomicity): 하나의 트랜잭션에 속한 모든 작업이 전부 성공하거나 전부 실패해서 결과를 예측할 수 있어야 한다.<br/>
일관성(Consistency): 트랜잭션 이후의 데이터베이스의 상태는 이전과 같이 유효(데이터베이스의 제약/규칙 (not null 등)을 만족해야 한다는 뜻)해야 한다.<br/>
고립성(Isolation): 모든 트랜잭션은 다른 트랜잭션으로부터 독립되어야 한다는 뜻이다.<br/>
지속성(Durability): 수행을 성공적으로 완료한 트랜잭션은 변경한 데이터를 영구히 저장해야 한다. 실패하더라고 로그는 영구적으로 남아있어야 한다.

</p>
</details>

<details>
  <summary>MVC 패턴이란?</summary>
<p>
MVC 패턴은 디자인 패턴중 하나로 Modal, View, Controller 세 가지 역할로 구분한 개발 방법론입니다<br/>
모델: 데이터와 비즈니스 로직을 관리합니다.<br/>
뷰: 레이아웃과 화면을 처리합니다.<br/>
컨트롤러: 모델과 뷰로 명령을 전달합니다.
</p>
</details>

<details>
  <summary>객체지향 프로그래밍이란?(OOP란?)</summary>
<p>
객체 지향 프로그래밍 (Object-Oriented Programming, OOP)은 프로그래밍에서 필요한 데이터를 추상화 시켜 상태와 행위를 가진 객체로 만들고, 객체들간의 상호작용을 통해 로직을 구성하는 프로그래밍 방법이다.<br/>
특징으로, 추상화, 상속, 다형성, 캡슐화 가 있다.<br/>
또는,<br/>
절차지향 프로그래밍은 말 그대로 실행하는 절차를 정해 프로그램하는 방법이고, 객체 지향은 상태 및 행위를 가진 객체를 통해 상호작용하여 로직을 구성하는 프로그래밍 방법이다.

</p>
</details>

<details>
  <summary>객체란?</summary>
<p>
객체는 프로그램에서 사용되는 데이터 또는 식별자에 의해 참조되는 공간을 의미하며 값을 저장 할 변수와 작업을
수행 할 메소드를 서로 연관된 것들끼리 묶어서 만든 것을 객체라고 할 수 있다.
</p>
</details>

<details>
  <summary>SOLID란?</summary>
<p>
단일 책임 원칙 (Single responsibility principle) 한 클래스는 하나의 책임만 가져야 한다.<br/>
개방-폐쇄 원칙 (Open/closed principle) 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.<br/>
리스코프 치환 원칙 (Liskov substitution principle) 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.<br/>
인터페이스 분리 원칙 (Interface segregation principle) 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.<br/>
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
DOM이란 Document Object Model의 약어이다. DOM은 HTML, XML 문서의 프로그래밍 인터페이스라고 할 수 있다.
문서의 구조화된 표현을 제공함으로써 프로그래밍 언어가 DOM 구조에 접근할 수 있도록 해주는 매우 중요한 역할이다.
개발자는 DOM 구조에 접근하여 문서 구조를 바꾸거나 스타일과 내용 등을 변경하고 이벤트를 연결시키는 등
다양한 작업을 수행할 수 있다.
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
렌더링이란 HTML,CSS, 자바스크립트 등 개발자가 작성한 문서가 브라우저에서 출력되는 과정을 말한다.<br/>
DOM트리와 CSSOM트리를 만들고 이들을 결합하여 렌더링 트리 생성후 이를 모든 상대적인 값을 픽셀로 변환후 페인팅한다.
</p>
</details>

<details>
<summary>Wrapper Class란?</summary>
<p>
기본 자료형(Primitive data type)에 대한 객체 표현을 Wrapper class라고 합니다.<br/>
기본 자료형 → Wrapper class로 변환하는 것을 Boxing이라 하며,<br/>
Wrapper class → 기본 자료형으로 변환하는 것을 UnBoxing이라 한다.
</p>

</details>

<details>
  <summary>Atomic 객체란?</summary>
  <p>atomic 변수는 멀티 스레드 환경에서 원자성을 보장하기 위해 나온 개념이다. synchronized와는 다르게 blocking이 아닌 non-blocking하면서 원자성을 보장하여 동기화 문제를 해결한다. atomic의 핵심 동작 원리는 CAS(Compare And Swap) 알고리즘이다.</p>
</details>

<details>
  <summary>blocking vs non-blocking</summary>
  <p>blocking은 요청한 작업을 마칠 때까지 계속 대기한다. non-blocking은 요청한 작업을 즉시 마칠 수 없다면 즉시 return한다.</p>
</details>

<details>
  <summary>JAR VS WAR</summary>
<p>
Jar 는 자바 아카이브로 JAVA 어플리케이션이 동작할 수 있도록 자바 프로젝트를 압축한 파일로 jre만 있다면 실행가능하다 war 는 웹어플리케이션 아카이브로 웹 관련 자원(html JavaScript jsp)등 웹 지원을 포함하고 was에서만 실행가능한 파일 형태이다
</p>
</details>

<details>
  <summary>Header 에 토큰을 넣어야하는 이유</summary>
<p>
HTTP 표준이 Authorization라는 헤더를 제공하여 여기에 넣는 것을 권장하고 본안 측면에서 바디에 넣을 경우 보다 노출될 가능성이 낮다.
</p>
</details>

<details>
  <summary>promise await 차이</summary>
<p>
Promise의 경우 여러 개의 비동기 작업을 연속으로 처리할때 .then .catch 비동기 작업마다 처리를 해줘야해서 코드가 중첩되고 복잡해진다<br/>
async/await 는 비동기 코드를 동기식으로 작성할 수 있어 가독성이 향상되고 중첩된 구조가 사라지는 등의 이점이 있다.
</p>
</details>

<details>
  <summary>CallBack 과 Promise 차이는?</summary>
<p>

</p>
</details>

<details>
  <summary>클로저란?</summary>
<p>
클로저란 어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상<br/>
함수가 속한 렉시컬 스코프를 기억하여, 함수가 렉시컬 스코프 밖에서 함수가 종료된 이후에도 이 스코프에 접근할 수 있게 해주는 기능이다.
</p>
</details>

<details>
  <summary>콜백 함수란?</summary>
<p>
콜백(callback) 또는 콜백 함수(callback function)는 다른 코드의 인자로서 넘겨주는 실행 가능한 코드를 말한다. 콜백함수을 넘겨받는 코드는 이 콜백을 필요에 따라 즉시 실행할 수도 있고, 아니면 나중에 실행할 수도 있다.
</p>
</details>

<details>
  <summary>자바스크립트는 싱글 쓰레드인데 비동기처리가 가능한 이유</summary>
<p>
이벤트 루프는 자바스크립트가 아닌 브라우저에 내장되어 있는 기능 중 하나다. 즉, 자바스크립트는 싱글 스레드이지만, 브라우저에서는 이벤트 루프 덕분에 멀티 스레드로 동작하여 비동기 작업이 가능하다.<br/>
자바스크립트가 멀티 쓰레드로 실행되는 언어였다면 웹페이지에서 발생하는 동시성 문제에 대해 해결해야 했다.
</p>
</details>

<details>
  <summary>'' , "" , `` 차이점은?</summary>
<p>
큰 따옴표와 작은 따옴표는 차이가 없고 google은 HTML에서 작성하기 간단한 작은 따옴표를 권장한다. 백틱은 Escape 문자를 사용하지 않고 작성할 수 있는 장점이 있다.
</p>
</details>

<details>
  <summary>Hash 란?</summary>
<p>임의의 크기를 가진 데이터를 고정된 크기의 데이터로 변환시켜 저장하는 방식</p>
</details>

<details>
<summary>Hash 충돌이란?</summary>
<p>
해시 코드 충돌은 2개 이상의 Key 객체가 동일한 최종 해시값을 생성해서 동일한 버킷 위치 또는 배열 인덱스를 가리키는 상황
</p>
</details>

<details>
<summary>Hash 충돌 해결방법은?</summary>
<p>
개방 주소법으로 고정폭으로 이동하여 빈 공간을 찾는 방법과 분리 연결법으로 같은 해시값을 가질경우 LinkedList로 연결하는 방법이 있다.
</p>
</details>

<details>
  <summary>Webpack</summary>
<p>
Webpack은 여러 개의 JavaScript 파일 및 다른 종속성들을 하나로 묶어주는 번들러로 리스소를 관리할 때 사용합니다.
</p>
</details>

<details>
  <summary>BABEL</summary>
<p>
최신 자바스크립트를 브라우저 호환되는 버전으로 변환하는 역할을 하고 JSX파일을 JS로 변환하는 역할도 한다.
</p>
</details>

<details>
  <summary>OLTP 와 OLAP란?</summary>
<p>
온라인 트랜잭션 처리(Online transaction processing) 주로 실시간으로 발생하는 트랜잭션 데이터를 처리하고 관리를 정의한 말로 이에 RDBMS가 특화되어 있다.<br/>
온라인 분석 처리(Online Analytical Processing)는 데이터 분석 및 의사결정 지원을 위한 환경을 정의한 말로 데이터를 집계, 분석하여 사용자에게 정보를 제공하는 데 사용됩니다.
</p>
</details>

<details>
  <summary>Database에서 LOCK이란?</summary>
<p>
공유(Shared) Lock으로 데이터를 읽기위한 Lock으로 공유 Lock은 공유 Lock 끼리 서로 동시에 접근이 가능하다.<br/>
베타(Exclusive) Lock은 데이터를 변경하고자 할 때 사용되며 Lock이 해제 이전 까지는 다른 트랜잭션은 해당 리소스에 접근할 수 없다
</p>
</details>

<details>
  <summary>교착 상태란?</summary>
  <p>
트랜잭션들이 잠금이 걸린 자원을 무기한 대기하는 현상으로 이를 해결하기 위해서는 필요한 데이터에 미리 Lock을 걸어두거나, 타임 스탬프 기법(트랜잭션 간의 순서를 미리 선택)을 적용하여 데이터를 시간 순서대로 제어하는 방법이 있다.
</p>
</details>

<details>
  <summary>서브 쿼리의 종류는?</summary>
<p>
스칼라 서브쿼리로 SELECT절에 위치해 하나의 값을 반환하는 서브쿼리<br/>
인라인 뷰로 FROM 절에 위치하는 서브쿼리로 결과는 반드시 하나의 테이블로 리턴<br/>
중첩 서브쿼리로 WHERE 절에 위치하며 결과집합을 한정하기 위한 서브쿼리이다.
</p>
</details>

<details>
  <summary>VIEW 란?</summary>
<p>
 뷰는 사용자에게 접근이 허용된 자료만을 제한적으로 보여주기 위해 하나 이상의 기본 테이블로부터 유도된, 이름을 가지는 가상 테이블이다.
</p>
</details>

<details>
  <summary>HTTP HTTPS 차이?</summary>
<p>
HTTP 메시지는 일반 텍스트이므로, 권한이 없는 당사자가 인터넷을 통해 쉽게 액세스하고 읽을 수 있다. 반면, HTTPS는 모든 데이터를 암호화된 형태로 전송하여 3자가 읽을 수 없다.
</p>
</details>

<details>
  <summary>UNION VS UNION ALL</summary>
<p>
여러개의 쿼리의 합집합으로 여러개의 SQL문을 합쳐서 하나의 SQL문으로 만들어주는 방법
UNION : 여러개의 쿼리의 중복값을 제거한 결과
UNION ALL : 여러개의 쿼리결과가 중복이 되더라도 전부 결과에 반영
</p>
</details>

<details>
  <summary>이벤트 버블링 , 이벤트 캡쳐링</summary>
<p>
이벤트 버블링이란 이벤트가 발생한 요소 부터 최상단 document 까지 이벤트를 전파
이벤트 캡쳐링은 버블링과 반대로 동작
</p>
</details>

<details>
  <summary>call, apply, bind 차이</summary>
<p>
call, apply, bind 모두 this를 바인딩하기 위해 사용하는데
call 의 경우 this, parameter를 순서대로 받지만 apply는 배열의 형태로 받고 둘다 즉시 실행한다.
bind는 call, apply와 다르게 즉시 실행하지않고 함수로 반환한다.
</p>
</details>

<details>
  <summary>번들링이란?</summary>
  <p></p>
</details>

<details>
  <summary>불변객체란?</summary>
<p>불변객체란 한번 할당하면 내부 데이터를 변경할 수 없는 객체를 뜻하며, 객체의 신뢰성이 높아지며 멀티스레드 환경에서 동기처리 없이 객체를 공유할 수 있다.</p>
</details>

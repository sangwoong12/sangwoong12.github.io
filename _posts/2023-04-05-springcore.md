---
title: Spring Core
date: 2023-04-10 09:00:00 +0800
categories: [spring]
tags: [spring]
---

# Spring Framework Overview

## Framework
- 정해진 매뉴얼, 룰을 제공한다. 개발 시 필수적인 코드와 알고리즘 같은 기능을 제공하는데 이 룰을 지켜야 한다.
- 클래스와 라이브러리가 합쳐진 구조이며, 이러한 협업 형태를 제공하는 것이다.
- 내 code를 호출하여 사용

## Library
- 어떤 특정한 기능을 구현하기 위해 미리 만들어진 함수들의 집합이다. 필요할 때만 자유롭게 사용할 수 있는 일종의 도구이다.
- 라이브러리를 호출하여 code에 사용

# Spring Framework 특징

## 경량 컨테이너로서, Spring Bean 을 직접 관리한다.

- Spring Bean 객체의 라이프 사이클을 관리한다.
  - Spring Bean : Spring Container 가 관리하는 중요 객체
- Container - Spring Bean 객체의 생성, 보관, 제거에 관한 모든일을 처리한다.

## POJO(Plain Old Java Object) 기반의 프레임 워크.
- POJO : 특정 기술에 종속되지 않는 순수한 자바 객체
- 일반적인 J2EE 프레임워크와 비교하여, 특정한 인터페이스를 구현하거나 상속을 받을 필요가 없다.
- 기존에 존재하는 라이브러리를 사용하기 편리하다.

## 제어 역전 (IoC : Inversion of Control)
- IoC : 객체의 생성, 생명 주기의 관리까지 모든 객체에 대한 제어권이 바뀌었다는 것을 의미
- 컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어서, 필요에 따라 Spring 에서 사용자의 코드를 호출한다.
- 의존성 주입(DI : Dependency Injection)
- DDD, TDD와 같은 프로그래밍 개발론에도 적합한 프레임워크이다.
-
### IoC 컨테이너
- 스프링 프레임워크도 객체에 대한 생성 및 생명주기를 관리할 수 있는 기능을 제공하고 있다.
- 역할
  - 객체의 생성을 책임지고, 의존성을 관리한다.
  - POJO의 생성, 초기화, 서비스, 소멸에 대한 권한을 가진다.
  - 직접 POJO 를 생성 및 생명주기를 관리해도 되지만 컨테이너에게 위임힌다.

### 분류
- DL (Dependency Lookup) : 저장소에 저장되어 있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup 하는 것
- DI (Dependency Injection) : 각 클래스간의 의존관계를 빈 설정(Bean Definition) 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것

## 관점 지향 프로그래밍 (AOP : Aspect-Oriented Programming)을 지원
- 복잡한 비지니스 영역의 문제와 공통된 지원 영역의 문제를 분리할 수 있음.
- 문제 해결을 위한 집중.

## BeanFactory
- 가장 기본적인 컨테이너로서, 객체를 생성하고 관리한다.

## ApplicationContext
- BeanFactory 를 상속받은 확장된 컨테이너로서, 메시지 번들 처리, 이벤트 발행, AOP 등 다양한 기능을 제공한다.


# Dependency Injection
- IoC 패턴 중에 하나
- Object 간의 의존성을 낮춘다.
  - 객체내에서 집적 생성하지않고 외부로 부터 주입을 받는다.
- 외부에서 객체를 생성하고 전달한다.

## 종류
- 정리하기

## AOP (Aspect-Oriented Programming)
- 관점지향 프로그래밍
- 관점은 다양한 타입과 객체에 걸친 트랜잭션 관리같은 관심을 모듈화할 수 있게 합니다.
  - crosscutting concerns : 횡단 관심사
  - core concerns : 주요 관심사

<img src="/images/spring-core/concerns.png">

## Advice
- 타겟에 제공할 부가기능을 담은 모듈
- 특정 Join Point 에서 Aspect가 취하는 행동
- Ex) around, before, after

## Pointcut
- Advice를 적용할 Joint Point 를 선별하는 작업 또는 그 기능을 적용한 모듈
- Advice는 Pointcut 표현식과 연결되고 Pointcut이 매치한 Join Point에서 실행된다.

## Target Object
- 부가기능을 부여할 대상
- 하나 이상의 Aspect 로 어드바이된 객체
- advised Object 라고 부르기도 함

## AOP Proxy
- 클라이언트와 타겟 사이에 투명하게 존재하면서 부가기능을 제공하는 오브젝트
- aspect 계약(어드바이스 메서드 실행 등)을 위해 AOP에 의해 생성된 객체

## Advisor
- Pointcut 과 Advice 를 하나씩 갖고 있는 객체
- 스프링 AOP 에서만 사용되는 용어

## Weaving
- 다른 어플리케이션 타입이나 어드바이즈된 객체를 생성하는 객체와 관점을 연결하는 행위를 의미


## AspectJ 사용 요약
```java
@Aspect
@Component
public class LoggingAspect {
    @Around("execution(public * *(..))")
    private Object loggingAlPublicOperation(){
    }
}
```

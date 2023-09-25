---
title: 스프링 부트
date: 2023-09-25 18:00:00 +0800
categories: [Spring Boot Up & Running]
tags: [spring]
---

## 스프링 부트의 핵심 기능

스프링 부트의 세 가지 핵심 기능은 **의존성 관리 간소화**, **배포 간소화**, **자동 설정**이다.

### 의존성 관리 간소화 - 스타터

스프링 부트는 BOM인 spring-boot-starter-web 을 통해 단일 스타터에 포함된 여러 의존성 안에 들어 있는 각 의존성 내의 **여러 라이브러리 버전**이 모든
의존성에 맞게 동기화하고 관리한다.

> POM : 빌드 도구에서 의존성을 가져오고 프로젝트 빌드에 사용하는 정보와 프로젝트 구성이 담긴 파일
>
> BOM : 프로젝트 아티팩트(라이브러리 등의 구성 요소)의 정보, 버전과 의존성 관리를 포함한 특수 POM

### 배포 간소화 - 실행 가능한 JAR

스프링 부트는 번거로운 배포 프로세스의 많은 부분을 한 단계로 간소화했으며, 단일 파일을 원하는 곳까지 복사하거나 클라우드 파운드리를 진행하더라도, 두 단계로 줄일 수 있다.

스프링 부트 설계자는 **셰이딩방식 대신 중첩된 JAR**를 사용하여 많은 잠재적 문제를 완화했다.

> 셰이딩 방식 : 모든 종속성 JAR 파일에서 클래스 및 리로스 파일을 추출하여 하나로 만드는 방식 (JAR 내부에 JAR없음)
>
> 중첩된 방식 : 모든 종속성 JAR 파일을 그대로 유지한 채로 내부에 중첩 시키는 방식 (JAR 내부에 JAR존재)

중첩된 JAR 방식을 사용하는 이유는 셰이딩 방식의 문제점인 의존성 JAR A와 의존성 JAR B가 각각 다른 버전의 라이브러리 C를 사용할 때 발생 가능한 버전충돌이 없기
때문이다.

이렇게 모든 의존성이 포함된 단일 JAR(Über JAR)는 배포할 때 ```Java -jar <SpringBootAppName.jar>```처럼 배포할 수 있다.

> Über JAR (Fat JAR) : JAR 방식중 하나로 자바 런타임 환경에서 전체 애플리케이션 실행에 필요한 모든 것을 포함하는 패키징 방식.

### 자동 설정 - 스프링 부트의 '마법'

스프링 부트는 자동 설정을 지원함으로서 이전에 유사한 기능을 구현하면서 매우 간단한 작업을 위해 수백 개의 엄청난 configuration 코드를 반복 작성하지 않아도 된다.

스프링은 **설정보다 관습**이라는 정신을 추구하며, 수많은 설정 작업이 아닌 비지니스 로직을 만드는데 집중할 수 있도록 한다.
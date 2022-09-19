---
layout: post
title: JPA를 위한 어노테이션 정리
date: 2022-09-17 19:00:00 +0800
last_modified_at: 2022-09-17 19:30:00 +0800
tags: [maven]
toc:  true
---

# JPA를 위한 어노테이션 정리

## @Entity
JPA가 관리할 객체임을 명시하는 것
## @Table
맵핑할 DB 테이블 명을 지정하는 것
## @Id
PK값 설정
## @Colume
필드와 컬럼 맵핑 할때 작성 만약 같을 경우 생략해도 된다.
## @GeneratedValue(starategy = GenerationType.*)
- IDENTITY  
기본키 생성을 데이터베이스에 위임한다. 즉 @Id값을 null로 하면 DB에서 AUTO_INCREMENT를 해서 자동으로 id값을 넣어주게된다.
ex) MySQL  
    - AUTO_INCREMENT는 INSERT 쿼리 실행이후 id값을 설정하기 때문에 **영속성 컨텍스트의 1차에서 관리불가**하다. 그렇기 때문에 IDENTITY의 경우 EntityManager.persist()를 하는 시점에서 INSERT SQL을 실행하기 때문에 식별자를 조회하여 영속성 컨텍스트의 1차 캐시에 값을 넣는다.
- SEQUENCE  
여기는 오늘 커밋할부ㅜㅂㄴ


- AUTO  
- TABLE  


## 1. github repository 생성

github 아이디 생성이후 로그인이후 <a href="github.com/new">[repository 생성]</a>를 누른다.

<img src="/images/first-make-blog/1.png" width="80%" height="80%">

> 1. 빨간 글씨로 적혀있는 부분에 자신의 Owner이름.github.io 라고 작성한다.
> 2. Public 로 체크 하고 Add a README file 를 체크한다.

이후 생성하면된다. 잘 동작하는지 확인하기 위해 "자신의 이름.github.io"로 접근해서 화면이 뜬다면 성공한 것이다.



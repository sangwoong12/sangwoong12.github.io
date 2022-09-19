---
layout: post
title: JPA를 위한 어노테이션 정리
date: 2022-09-17 19:00:00 +0800
last_modified_at: 2022-09-17 19:30:00 +0800
tags: [maven]
toc:  true
---

# JPA를 위한 어노테이션 정리
``` java
@Entity
@Table(name = "Members")
public class Member {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "member_name") //컬럼명이 다를경우 @Column을 사용
    private String name;
    
    private String password; //같을 경우 생략가능

}
```
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
- AUTO  
기본 설정 값으로 각 데이터베이스에 맞게 자동으로 기본키를 생성해준다.
- SEQUENCE  
데이터 베이스 Sequence Object를 사용하여 데이터베이스가 자동으로 기본키를 설정하는데 @SequenceGenerator 어노테이션을 필수로 넣어야한다. ex)Oracle
- TABLE  
키 생성 전용 테이블을 하나 만들어 사용하는 방식으로 @TableGenerator를 필요로 하고 Sequence와 유사하다.

자세한 내용은 추후 다루도록 하겠다.

## @IdClass
Pk값을 여러개 가지고 있을경우 사용하는 어노테이션으로 각각 @Id 어노테이션을 주는 방식도 있지만 오류가 발생할수 있기때문에 @IdClass를 쓰는 것을 권장한다.

``` java
@Entity
@Getter
@Setter
@IdClass(Class.Pk.class)
public class Class {

    @Id
    @Column(name = "student_id")
    private Integer studentId;

    @Id
    @Column(name = "subject_code")
    private String subjectCode;

    private String comment;


    public static class Pk{
        @Column(name = "student_id")
        private Integer studentId;
        @Column(name = "subject_code")
        private String subjectCode;
    }
}
```

위의 코드와 같이 @Id 어노테이션을 붙여 평소와 똑같이 만들고 **추가적으로 Pk 클래스를 만들어 @IdClass 어노테이션에 Pk 정보가 들어있는 클래스**를 넣어주면 된다.
## @EmbeddedId / @Embeddable
@IdClass와 같이 복합 Pk를 처리하는 방식으로 코드는 다음과 같다.

``` java
@Entity
@Getter
@Setter
public class Class {

    @EmbeddedId
    private Pk pk;

    private String comment;

    @Embeddable
    public static class Pk{
        @Column(name = "student_id")
        private Integer studentId;
        @Column(name = "subject_code")
        private String subjectCode;
    }
}
```

Pk 정보가 들어있는 클래스에 @Embeddable을 붙이고 클래스안에 @EmbeddedId Pk 정보가 들어있는 클래스를 선언해주면된다.
## 1. github repository 생성

github 아이디 생성이후 로그인이후 <a href="github.com/new">[repository 생성]</a>를 누른다.

<img src="/images/first-make-blog/1.png" width="80%" height="80%">

> 1. 빨간 글씨로 적혀있는 부분에 자신의 Owner이름.github.io 라고 작성한다.
> 2. Public 로 체크 하고 Add a README file 를 체크한다.

이후 생성하면된다. 잘 동작하는지 확인하기 위해 "자신의 이름.github.io"로 접근해서 화면이 뜬다면 성공한 것이다.



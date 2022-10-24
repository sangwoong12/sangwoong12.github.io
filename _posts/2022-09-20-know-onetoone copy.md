---
layout: post
title: JPA @OneToOne
date: 2022-09-20 19:00:00 +0800
last_modified_at: 2022-09-20 19:30:00 +0800
tags: [jpa]
toc:  true
---

@OneToOne 은 **단방향, 양방향**으로 할지 먼저 정해야한다. 방법을 크게 4가지로 나눌수있다.  
양방향의 경우 단방향보다 복잡하고 객체에서 양쪽 방향을 모두 관리해야하는 단점이 있지만 반대 방향으로 객체그래프를 탐색할 수 있다.

---
# 1. 주 테이블에 외래키 

## ERD
<img src="/images/know-onetoone/1.png">


``` java
@Entity
@Table(name= "Members")
public class Member{
@Id
    @Column(name = "member_id")
    private String id;

    @OneToOne
    @JoinColumn(name = "locker_id")
    private Locker locker;
}
```
주 테이블에 Locker와 @OneToOne 관계를 매핑을 해준다.
## 단방향
``` java
@Entity
@Table(name= "Lockers")
public class Locker{
    @Id
    @Column(name = "locker_id")
    private Long id;

    @Column(name = "locker_name")
    private String name;
}
```
단방향의 경우 위와같이 주 테이블 Member 에 관한 정보를 가지고 있지 않다.
## 양방향
``` java
@Entity
@Table(name= "Lockers")
public class Locker{
    @Id
    @Column(name = "locker_id")
    private Long id;

    @Column(name = "locker_name")
    private String name;

    @OneToOne(mappedBy = "locker")    
    private Member member;
}
```
양방향의 경우 Locker에 mappedBy를 해주면 양방향 관계가 된다.

---

# 2. 대상 테이블에 외래 키

## ERD
<img src="/images/know-onetoone/2.png">

``` java
@Entity
@Table(name= "Lockers")
public class Locker{
@Id
    @Id
    @Column(name = "locker_id")
    private Long id;

    @Column(name = "locker_name")
    private String name;

    @OneToOne
    @JoinColumn(name = "Member_id")
    private Member member;
}
```
대상 테이블에 Member와 @OneToOne 관계를 매핑을 해준다.
## 단방향
일대일관계에서 대상 테이블에 외래 키를 저장하는 단방향 관계는 JPA에서 지원하지 않는다.
## 양방향
``` java
@Entity
@Table(name = "Members")
public class Member {
    @Id
    @Column(name = "member_id")
    private String id;

    @Column(name = "user_name")
    private String userName;

    @OneToOne(mappedBy = "member")
    private Locker locker;

}
```
양뱡향의 경우 Member에 mappedBy를 해주면 양방향 관계가 된다.

# 3.정리

## 주 테이블에 외래키
객체지향 개발자들이 선호하는 방식이고 JPA 매핑이 편리하다.  

장점: 주 테이블만 조회하는 것으로 대상 테이블의 데이터 여부를 체크할 수 있다.  
단점: 없을경우 NULL 넣어야해서 NULL을 허용해주어야 하는데 DB입장에서는 좋지않다.

## 대상 테이블에 외래키
데이터베이스 개발자들이 선호하는 방식으로 위의 문제점인 NULL을 허용해야하는 문제가 없다.

장점: 맵핑관계 스펙이 바뀌어도 유지 보수가 간단하다.  
단점: JPA에서 대상 테이블에 외래 키를 저장하는 단방향 관졔를 지원하지 않기 때문에 필연적으로 양방향으로 맵핑을 해야한다. 추가적으로 **지연로딩** 설정이 되지않는다.




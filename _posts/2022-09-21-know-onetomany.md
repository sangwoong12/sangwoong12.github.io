---
layout: post
title: JPA @OneToMany, @ManyToOne
date: 2022-09-21 19:00:00 +0800
last_modified_at: 2022-09-21 19:30:00 +0800
tags: [jpa]
toc:  true
---
## @ManyToOne 단방향

하기전에 @JoinColumn 과 @OneToMany(mappedBy = *) mappedBy를 이해하고 넘어가야한다.  

- @JoinColumn  
@JoinColumn의 경우 @ManyToOne 어노테이션을 통해 다대일 연관관계를 매핑할 때 사용하는데 대상에 해당하는 @Column에 해당하는 name값을 입력하면 된다.

- @OneToMany(mappedBy = * )  
mappedBy의 경우 @OneToMany 어노테이션을 통해 일대다 연관관계를 매핑할 때 사용하는데 이도 마찬가지로 매핑대상이 되는 @Column name값을 입력하면 된다.


### ERD
<img src="/images/know-onetomany/1.png">


``` java
@Entity
@Table(name= "Members")
public class Member{
    @Id
    @Column(name = "member_id")
    private String id;

    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;

    private String UserName;
}
```
``` java
@Entity
@Table(name= "Teams")
public class Team{

    @Id
    @Column(name= "team_id")
    private Long id;

    private String name;

}
```
단방향으로 맵핑할경우 Members는 team 엔티티를 참조할 수 있지만 반대로 team에서는 회원을 참조할수있는 필드가 없다.

## @ManyToOne 양방향

``` java
@Entity
@Table(name= "Members")
public class Member{
    @Id
    @Column(name = "member_id")
    private String id;

    @ManyToOne
    @JoinColumn(name = "team_id")
    private Team team;

    private String UserName;

    public void setTeam(Team team){
        this.team = team;
    }
}
```
``` java
@Entity
@Table(name= "Teams")
public class Team{

    @Id
    @Column(name= "team_id")
    private Long id;

    private String name;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();

    public void addMember(Member member){
        this.members.add(member);
        member.setTeam(this);
    }

}
```
양방향은 외래 키가 있는 쪽이 연관관계 주인이다. 외래 키의 경우 항상 N에 외래 키가 있다.
업로드 속도 테스트

---


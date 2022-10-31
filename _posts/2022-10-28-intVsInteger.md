---
layout: post
title: int 와 Integer의 차이
date: 2022-10-28 09:00:00 +0800
last_modified_at: 2022-10-28 18:00:00 +0800
tags: [java]
toc:  true
---

참고문헌

<href="https://www.infoworld.com/article/3512039/does-java-pass-by-reference-or-pass-by-value.html">

<href="https://velog.io/@aki/HotSpot-JVM">

### Integer
- Immutable
  - Integer는 한번 생성되게 되면 heap 영역에서 불변하다.
  - 불변한거지 값을 못바꾸는게 아니다. 즉 값을 변경할 경우 기존에 존재하는 heap영역의 값을 버리고 새로운 값을heap영역에 생성하여 참조하게 된다.
  - 기존의 값은 JVM에서 GC를 정리하여 메모리 적으로 문제는 없다.
- 그러면 왜 Immutable로 설정하였을까?
  - 생성자, 접근메소드에 대한 방어 복사가 필요없다.
  - 멀티스레드 환경에서 동기화 처리없이 객체를 공유할 수 있습니다.(Thread-safe) 불변이기 때문에 객체가 안전하다.

### 상수풀(Constant pool)
- 힙 영역의 고정 영역에 생성되어 Java 프로세스의 종료까지 계속 유지되는 메모리 영역이다.
- JVM에서 관린하며 프로그래머가 작성한 상수에 대해서 최우선적으로 찾아보고 없으면 상수풀에 추가한 이후 그 주소값을 리턴한다.
- final class 변수의 경우도 상수 풀에 값을 복사
- 위와 같은 immutable한 값들은 heap영역 안에 존재하는 상수풀에 저장이된다.

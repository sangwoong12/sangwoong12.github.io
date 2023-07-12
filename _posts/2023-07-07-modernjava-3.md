---
title: 람다 표현식 d
ate: 2023-07-07 18:00:00 +0800
categories: [모던 자바 인 액션]
tags: [jpa,book]
---

# 람다 표현식

- 메서드로 전달할 수 있는 익명 함수를 함수를 단순화한 것이라고 할 수 있다.
- 람다 표현식에는 이름은 없지만, 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트는 가질 수 있다.

## 람다 특징

1. 익명 : 보통의 메서드와 달리 이름이 없으므로 **익명**이라 표현한다. 구현해야 할 코드에 대한 걱정거리가 줄어든다.
2. 함수 : 람다는 메서드처럼 특정 클래스에 종속되지 않으므로 함수라고 부른다. 하지만 메서드처럼 파라미터 리스트, 바디, 반환 형식, 가능한 예외 리스트를 포함한다.
3. 전달 : 람다 표현식을 메서드 인수로 전달하거나 변수로 저장할 수 있다.
4. 간결성 : 익명 클래스처럼 많은 자질구레한 코드를 구현할 필요가 없다.

## 사용방법

- 기존 코드

```java
Comparator<Apple> byWeight=new Comparator<Apple> {
public int compare(Apple a1,Apple a2){
  return a1.getWeight().compareTo(a2.getWeight());
  }
  };
```

- 람다 코드

```java
Comparator<Apple> byWeight=(Apple a1,Apple a2)->a1.getWeight().compareTo(a2.getWeight());
```

- 파라미터 리스트 :
  - Comparator의 메서드 파라미터(사과 두개) : (Apple a1, Apple a2)
- 화살표 :
  - 화살표(->) 람다의 파라미터 리스트와 바디를 구분한다.
- 람다 바디
  - 두 사과의 무게를 비교한다. 람다의 반환값에 해당하는 표현식이다.

[람다 종류 및 알아보기](https://sangwoong12.github.io/posts/lambda/)



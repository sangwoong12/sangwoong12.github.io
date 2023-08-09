---
title: 람다 표현식 date: 2023-07-07 18:00:00 +0800 categories: [모던 자바 인 액션]
tags: [jpa,book]
---

## 람다란?

메서드로 전달할 수 있는 익명 함수를 함수를 단순화한 것이라고 할 수 있다.

람다 표현식에는 이름은 없지만, 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트는 가질 수 있다.

---

## 람다 특징

1. 익명 : 보통의 메서드와 달리 이름이 없으므로 **익명**이라 표현한다. 구현해야 할 코드에 대한 걱정거리가 줄어든다.
2. 함수 : 람다는 메서드처럼 특정 클래스에 종속되지 않으므로 함수라고 부른다. 하지만 메서드처럼 파라미터 리스트, 바디, 반환 형식, 가능한 예외 리스트를 포함한다.
3. 전달 : 람다 표현식을 메서드 인수로 전달하거나 변수로 저장할 수 있다.
4. 간결성 : 익명 클래스처럼 많은 자질구레한 코드를 구현할 필요가 없다.

---

## 사용방법

기존 코드

```java
Comparator<Apple> byWeight=new Comparator<Apple> {
public int compare(Apple a1,Apple a2){
  return a1.getWeight().compareTo(a2.getWeight());
  }
  };
```

람다 코드

```java
Comparator<Apple> byWeight=
  (Apple a1,Apple a2)->a1.getWeight().compareTo(a2.getWeight());
```

<img src="/images/modern/chapter3/1.png">

- 파라미터 리스트 :
  - Comparator의 메서드 파라미터(사과 두개) : (Apple a1, Apple a2)
- 화살표 :
  - 화살표(->) 람다의 파라미터 리스트와 바디를 구분한다.
- 람다 바디
  - 두 사과의 무게를 비교한다. 람다의 반환값에 해당하는 표현식이다.

---

## 함수형 인터페이스

**함수형 인터페이스**는 정확히 하나의 추상 메서드를 지정하는 인터페이스이다.

### 자바에서 지원하는 함수형 인터페이스

```java

@FunctionalInterface
public interface Comparator {

  int compare(T o1, T o2);
}

@FunctionalInterface
public interface Runnable {

  void run();
}
```

> Q, 만약 인터페이스에 Java8 부터 지원하는 디폴트 함수가 존재하면 함수형 인터페이스라고 할 수 있을까?
>
> A. 수많은 디폴트 함수가 존재해도 추상 메서드가 오직 하나면 함수형 인터페이스다.

### @FunctionalInterface

**@FunctionalInterface**는 함수형 인터페이스임을 가르키는 어노테이션으로 추상 메서드를 하나를 선언하도록 강제할 수 있다.

```java
@FunctionalInterface
public interface Function {
  void method1();
  void method2(); // Multiple non-overriding abstract methods found in interface [Interface Name]
}
```



### 자바 API 함수형 인터페이스
[자바 API 함수형 인터페이스](https://sangwoong12.github.io/posts/lambda/)


### 함수 디스크립터

함수형 인터페이스의 추상 메서드 시그니처는 람다 표현식의 시그니처를 서술하는 메서드를 **함수 디스크립터**하고 부른다.

예를 들어 Runnable 인터페이스는 인수와 반환값이 없는 시그니처로 생각할 수 있다.

- 표기법 : () -> void

## 람다 활용 : 실행 어라운드 패턴

<img src="/images/modern/chapter3/2.png">




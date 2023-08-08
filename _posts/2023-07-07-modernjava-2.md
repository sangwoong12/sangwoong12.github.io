---
title: 동작 파라미터화 코드 전달하기 date: 2023-07-07 09:00:00 +0800 categories: [모던 자바 인 액션]
tags: [jpa,book]
---

## 동작 파라미터화란?

아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록을 의미한다.

- 특징 :
  - 리스트의 모든 요소에 대해서 '어떤 동작'을 수행할 수 있음
  - 리스트 관련 작업을 끝낸 다음에 '어떤 다른 동작'을 수행할 수 있음
  - 에러가 발생하면 '정해진 어떤 다른 동작'을 수행할 수 있음

---

## 변화하는 요구사항에 대응하기

### 첫 번째 시도 : 녹색 사과 필터링

```java
public static List<Apple> filterGreenApples(List<Apple> inventory){
  List<Apple> result=new ArrayList<>();
  for(Apple apple:inventory){
  if(GREEN.equals(apple.getColor())){
  result.add(apple);
  }
  return result;
  }
  }
```

> Q. 만약 요구사항이 변경되어 다른 색으로 필터링을 해야할 경우 위의 코드는 즉각 대응할 수 있을까?
>
> A. 불가능하다. 위의 메서드에 파라미터를 추가하여 변화하는 요구사항에 좀 더 유연하게 대응하는 코드로 변경 해야한다.

### 두 번쟤 시도 : 색을 파라미터화

```java
public static List<Apple> filterApplesByColor(List<Apple> inventory,Color color){
  List<Apple> result=new ArrayList<>();
  for(Apple apple:inventory){
  if(apple.getColor().equals(color)){
  result.add(apple);
  }
  return result;
  }
  }
```

> Q. 만약 색 이외에도 가벼운 사과와 무거운 사과를 구분하도록 요구사항이 변경된다면?
>
> A. 위와 같이 파라미터를 변경하고 조건문도 요구사항에 맞게 수정하면 된다.

```java
public static List<Apple> filterApplesByWeight(List<Apple> inventory,int weight){
  List<Apple> result=new ArrayList<>();
  for(Apple apple:inventory){
  if(apple.getWeight()>weight){
  result.add(apple);
  }
  return result;
  }
  }
```

- 위 코드는 해결책이 될수 있지만 각 필터링 조건이 대부분 중복이다. 이는 소프트웨어 공학의 DRY(don't repeat yourself - 같은 것을 반복하지 말 것)원칙을
  어기는 것이다.
- 중복을 제거하기위해 합치는 방법도 있다.(이방법은 실전에서는 절대 이 방법을 사용하면 안된다.)

### 세 번째 시도 : 가능한 모든 속성으로 필터링

```java
public static List<Apple> filterApples(List<Apple> inventory,Color color,int weight,boolean flag){
  List<Apple> result=new ArrayList<>();
  for(Apple apple:inventory){
  if((flag&&apple.getColor().equals(color))||(!flag&&apple.getWeight()>weight)){
  result.add(apple);
  }
  }
  return result;
  }
```

- 추가하여 중복은 최소화 할 수 있지만 코드가 난잡하다.
- 요구사항이 변경될 경우 유연하게 대응하는 것도 불가능하다.

---

## 동작 파라미터화

- 위의 예제를 통해 요구사항에 더 유연하게 대응할 수 있는 방법으로 **동작 파라미터화**가 있다.

### 네 번째 시도 : 추상적 조건으로 필터링

- 참 또는 거짓을 반환하는 함수 (**predicate**) 인터페이스를 생성한다.

```java
public interface ApplePredicate {

  boolean test(Apple apple);
}
```

선택 조건을 대표하는 여러 버전의 ApplePredicate 를 정의할 수 있다.

```java
public class AppleHeavyWeightPredicate implements ApplePredicate {//무거운 사과만 선택

  public boolean test(Apple apple) {
    return apple.getWeight() > 150;
  }
}

public class AppleGreenColorPredicate implements ApplePredicate {//녹색 사과만 선택

  public boolean test(Apple apple) {
    return GREEN.equals(apple.getColor());
  }
}
```

<img src="images/modern/chapter2/1.png">

- 위 조건에 따라 filter 메서드가 다르게 동작할 것이라고 예상할 수 있다. 이를 전략 디자인 패턴이라고 한다.

> Q. ApplePredicate는 어떻게 다양한 동작을 수행할 수 있을까?
>
> A. filterApples에서 ApplePredicate 객체를 받아 애플의 조건을 검사하도록 메서드를 수정해야 한다.
> 이를 **동작 파라미터화** 라고 한다.

```java
public static List<Apple> filterApples(List<Apple> inventory,ApplePredicate p){
  List<Apple> result=new ArrayList<>();
  for(Apple apple:inventory){
  if(p.test(apple)){
  result.add(apple);
  }
  }
  }
```

- 이렇게 설계할 경우 요구사항이 추가되었을때 ApplePredicate를 적절하게 구현하는 클래스를 만들기만 하면된다.

---

## 복잡한 과정 간소화

- 유연성과 가독성은 챙길 수 있었지만 매번 여러 클래스를 정의하고 인스턴스화해야 한다.
- 이는 **익명 클래스**를 통해 해결할 수 있다.

> Q. 익명 클래스란?
>
> A. 자바의 지역 클래스와 비슷한 개념이다. 익명 클래스는 말 그대로 이름이 없는 클래스로 클래스 선언과 인스턴스화를 동시에 할 수 있다.

### 다섯 번째 시도 : 익명 클래스 사용

```java
List<Apple> redApples=filterApples(inventory,new ApplePredicate()){
public boolean test(Apple apple){
  return RED.equals(apple.getColor());
  }
  }
```

- 익명 클래스로도 아직 부족한 점이 있다.
  - 1 : 익명 클래스는 여전히 많은 공간을 차지한다.
  - 2 : 많은 프로그래머가 익명 클래스의 사용에 익숙하지 않다.

### 여섯 번째 시도 : 람다 표현식 사용

```java
List<Apple> result=filterApples(inventory,(Apple apple->RED.equals(apple.getColor())));
```

### 일곱 번째 시도 : 리스트 형식으로 추상화

```java
public interface Predicate<T> {

  boolean test(T t);
}

  public static <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> result = new ArrayList<>();
    for (T e : list) {
      if (p.test(e)) {
        reuslt.add(e);
      }
    }
    return result;
  }
```

- 이렇게 설계할 경우 사과 이외 바나나, 정수, 문자열 등 리스트에 필터 메서드를 사용할 수 있다.

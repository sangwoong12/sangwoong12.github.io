---
title: 람다 표현식
date: 2023-07-07 18:00:00 +0800
categories: [모던 자바 인 액션]
tags: [book]
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
Comparator<Apple> byWeight = new Comparator<Apple> {
  public int compare(Apple a1,Apple a2) {
      return a1.getWeight().compareTo(a2.getWeight());
  }
};
```

람다 코드

```java
Comparator<Apple> byWeight =
  (Apple a1,Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
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

  void method2();
}
// Multiple non-overriding abstract methods found in interface [Interface Name]
```

### 함수 디스크립터

함수형 인터페이스의 추상 메서드 시그니처는 람다 표현식의 시그니처를 서술하는 메서드를 **함수 디스크립터**하고 부른다.

예를 들어 Runnable 인터페이스는 인수와 반환값이 없는 시그니처로 생각할 수 있다.

- 표기법 : () -> void

자세한 내용은 추후 다루도록 한다.

---

## 람다 활용 : 실행 어라운드 패턴

설정(SetUp) 과 정리(CleanUp) 두 과정이 둘러싸는 형태를 갖는 것을 **실행 어라운드 패턴**이라고 한다.

<img src="/images/modern/chapter3/2.png">

### try-with-resources 구문

```java
public String processFile()throws IOException {
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))){
    return br.readLine();
  }
}
```

> Q. 요구사항이 변경되어 두줄을 읽어야 하는 상황을 대처할 수 있을까?
>
> A. 코드를 수정해야하기 때문에 유연하게 대처할 수 없다.

### 1 단계 : 동작 파라미터화

processFile을 동작 파라미터화 하여 BufferedReader를 이용해서 다른 동작을 수행할 수 있도록 processFile 메서드로 동작을 전달

### 2 단계 : 함수형 인터페이스를 이용해서 동작 전달

함수형 인터페이스 자리에 람다를 사용할 수 있다.

- BufferedReader -> String 과 IOException을 던질 수 있는 시그니처와 일치하는 **함수형 인터페이스** 선언

```java

public interface BufferedReaderProcessor {

  String process(BufferedReader br) throws IOException;
}
```

### 3 단계 : 동작 실행

processFile 바디 내에서 BufferedReaderProcessor 객체의 process를 호출하도록 구현

```java
public String processFile(BufferedReaderProcessor p)throws IOException {
  try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
    return p.process(br); // BufferedReader 객체 자리
  }
}
```

### 4 단계 : 람다 전달

람다를 이용한 다양한 동작 전달하여 생성 및 사용

```java
String oneLine = processFile((BufferedReader br)->br.readLine());
String twoLines = processFile((BufferedReader br)->br.readLine()+br.readLine());
```

위 4 가지 과정을 통해 **실행 어라운드 패턴**을 적용할 수 있다.

## 자바 API 함수형 인터페이스

[자바 API 함수형 인터페이스 알아보기](https://sangwoong12.github.io/posts/lambda/)

## 형식 검사, 형식 추론, 제약

람다 표현식 자체에는 람다가 어떤 함수형 인터페이스를 구현하는지의 정보가 포함되어 있지 않다.

- 어떤 콘텍스트(예를 들면 람다가 전달될 메서드 파라미터나 람다가 할당되는 변수등)에서 기대되는 람다 표현식의 형식을 **대상 형식** 이라고한다.

### 예시

```java
List<Apple> heavierThan105g =
  filter(inventory, (Apple apple) -> apple.getWeight() > 150);
```

<img src="/images/modern/chapter3/3.jpg">

1. filter 메서드의 선언을 확인한다.
2. filter 메서드는 두 번째로 파라미터로 Predicate<Apple> 형식을 기대한다.
3. Predicate<Apple>은 test라는 한 개의 추상 메서드를 정의하는 함수형 인터페이스이다.
4. test 메서드는 Appl을 바아 boolean을 반환하는 함수 디스크립터를 묘사한다.
5. filter 메서드로 전달된 인수는 이와 같은 요구사항을 만족해야 한다.

### 같은 람다, 다른 함수형 인터페이스

**대상 형식**이라는 특징 때문에 같은 람다 표현식이더라도 호환되는 추상 메서드를 가진 다른 함수형 인터페이스로 사용될 수 있다.


```java
Callable<Integer> c = () -> 42;
PrivilegedAction<Integer> p = () -> 42;
```

Callable 과 PrivilegedAction 모두 인수를 받지 않고 제네릭 형식 T를 반환하는 함수이기 때문에 위와 같이 가능하다.

> Q. Object o = () -> {System.out.println("Tricky example")}; 는 가능할까?
>
> A. 불가능하다. 어떠한 함수형 인터페이스를 참조해야 할지 명확하지 않기 때문이다.
> 이와 같은 경우는 아래와 같이 어떤 메서드의 시그니처가 사용되어야 하는지를 명시적으로 구분하도록 람다를 캐스트 하여 사용하면 된다.
>
> ```Object o = (Runnable) () -> {System.out.println("Tricky example")};```

### 형식 추론

위의 예시 처럼 함수형 인터페이스의 추론이 올바르게 되었다면, 파라미터 영식도 추론이 가능해진다.

```java
/* 파라미터 형식을 추론하지 않음*/
Comparator<Apple> c = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

아래와 같이 명시적으로 파라미터 형식을 생략할 수 있다.

```java
/* 파라미터 형식을 추론*/
Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());
```

---

## 지역 변수

람다 표현식에는 익명 함수가 하는 것처럼 **자유 변수**(**파라미터로 넘겨진 변수가 아닌 외부에서 정의된 변수**)를 활용할 수 있다.

이와 같은 동작을 **람다 캡처링**이라고 한다.

```java
/* portNumber 를 사용한 람다 */
int portNumber = 1234;
Runnable r = () -> System.out.println(portNumber);
```

위와 같이 사용할 수 있다.

```java
/* portNumber 재할당 */
portNumber = 1111;
// 재할당 할경우 Variable used in lambda expression should be final or effectively final
// final로 선언되거나 수정을 하면 안된다.
```

이를 제약사항을 걸어둔 이유는, 다른 스레드에서 변수 할당이 해제되었는데, 해당 변수를 다른 스레드에서 접근하려 할 수 있다.

따라서, 지역 변수에는 한 번만 값을 할당해야 한다는 제약이 생긴 것이다.

---

## 메서드 참조

메서드 참조를 이용하면 기존의 메서드 정의를 재활용해서 람다처럼 전달할 수 있다.

```java
inventory.sort((Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
/* 수정후 코드 */
invetory.sort(comparing(Apple::getWeigth));// 구분자 :: 사용
```

위 처럼 람다 표현식보다 메서드 참조를 사용하는 것이 더 가독성이 좋으며 자연스러울 수 있고 기존 메서드 구현으로 람다 표현식을 만들 수 있다.

### 생성자 참조

Class::new 처럼 클래스명과 new 키워드를 이용하여 기존 생성자의 참조를 만들 수 있다.

```java
Suplier<Apple> c1 = Apple::new; // 기존 코드 : () -> new Apple();
Apple a1 = c1.get(); // Supplier의 get메서드를 호출해서 새로운 Apple 객체를 만들 수 있다.

Function<Integer, Apple> c2 = Apple::new; // 기존 코드 : (weight) -> new Apple(weight);
Apple a2 = c2.apply(100); // Function의 Apply 메서드 에 무게를 인수로 호출해서 새로운 Apple 객체를 만들 수 있다.
```

활용 방식

```java
List<Integer> weights = Arrays.asList(7, 3, 4, 10);
List<Apple> apples = map(weights, Apple::new);
public List<Apple> map (List<Integer> list, Function<Integer, Apple> f) {
  List<Apple> result = new ArrayList<>();
  for (integer i : list) {
    result.add(f.apply(i));
  }
  return result;
}
```






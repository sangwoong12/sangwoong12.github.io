---
title: 스트림 소개
date: 2023-07-11 18:00:00 +0800
categories: [모던 자바 인 액션]
tags: [book]
---

# 스트림이란 무엇인가?

- 스트림은 자바 8 API에 새로 추가된 기능으로 선언형(즉, 데이터를 처리하는 임시 구현 코드 대신 질의로 표현할 수 있다)으로 컬렉션 데이터를 처리할 수 있다.
- 멀티스레드 코드를 구현하지 않아도 데이터를 **투명하게** 병렬로 처리할 수 있다.

---

## 스트림 시작하기

> Q. 시작하기 앞서 스트림이란 정확히 뭘까?
>
> A. '데이터 처리 연산을 지원하도록 소스에서 추출된 연속된 요소'로 정의할 수 있다. 이 파트에서 정의를 하나씩 살펴보자.

- 연속된 요소
  - 컬렉션과 마찬가지로 스트림은 특정 요소 형식으로 이루어진 연속된 값 집합의 인터페이스를 제공한다.
  - 컬렉션의 주제는 데이터, 스트림은 계산이다.
- 소스
  - 정렬된 컬렉션으로 스트림을 생성하면 정렬이 그대로 유지되는데 생성된 것을 소스라고 한다.
- 데이터 처리 연산
  - 스트림은 함수형 프로그래밍 언어에서 일반적으로 지원하는 연산과 데이터베이스와 비슷한 연산을 지원한다.

---

## 스트림의 특징

- 선언형 : 더 간결하고 가독성이 좋아진다.
- 조립할 수 있음 : 유연성이 좋아진다.
- 병렬화 : 성능이 좋아진다.
- **파이프라이닝** : 대부분의 스트림 연산은 스트림 연산끼리 연결해서 커다란 파이프라인을 구성할 수 있도록 스트림 자신을 반환한다.
- **내부 반복** : 반복자를 이용해서 명시적으로 반복하는 컬렉션과 달리 스트림은 내부 반복을 지원한다.

### 예시

```java
List<String> threeHighCaloricDishNames =
  menu.stream() // 메뉴(요리 리스트)에서 스트림을 얻는다.
    .filter(dish ->  dish.getCalories() > 300) //파이프라인 연산 만들기, 첫 번째로 고칼로리 요리를 필터링
    .map(Dish::getName) // 요리명 추출
    .limit(3) // 선착순 3개만 선택
    .collect(toList()); // 결과를 다른 리스트로 저장
System.out.println(threeHighCaloricDishNames)
```

1. menu : **데이터 소스**로 **연속된 요소**를 스트림에 제공한다.
2. filter, map, limit, collect 는 일련의 **데이터 처리 연산**이다. 또한, 모든 연산은 서로 **파이프라인**을 형성할 수 있도록 스트림에 반환한다.
3. collect : 스트림을 다른 형식으로 변환한다.

---

## 스트림과 컬렉션

- 스트림과 컬렉션의 가장 큰 차이점은 데이터를 계산하는 시점이다.
- 컬렉션 :
  - 컬렉션의 모든 요소는 컬렉션에 추가하기 전에 계산되어야 해야난 **'적극적 생성'**에 해당된다.
  - 컬렉션은 DVD 에 비유할 수 있는데 모든 파일이 로딩된 이후 시청가능하다는 점이 비슷하다.
- 스트림 :
  - 스트림의 경우 **요청할 때만 요소를 계산**하는 고정된 자료구조로 **'게으른 생성'**에 해당된다.
  - 스트림의 스트리밍에 비유할 수 있는데 특정 구간에만 흘러가듯이 영상을 보여주는 점이 비슷하다.

```java
public class StreamVsCollectionExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // 컬렉션을 사용한 적극적 생성
        List<Integer> doubledNumbers = doubleNumbers(numbers);
        System.out.println("컬렉션: " + doubledNumbers);

        // 스트림을 사용한 게으른 생성
        Stream<Integer> numberStream = numbers.stream();
        Stream<Integer> doubledNumberStream = numberStream.map(n -> n * 2);//아직 요소가 생성되지 않고 지연
        System.out.println("스트림: " + Arrays.toString(doubledNumberStream.toArray()));//사용시점에 계산
    }

    public static List<Integer> doubleNumbers(List<Integer> numbers) {
        List<Integer> doubledNumbers = new ArrayList<>();
        for (Integer number : numbers) {
            doubledNumbers.add(number * 2);
        }
        return doubledNumbers;
    }
}
```

---

## 탐색

- 반복자와 마찬가지로 스트림도 한 번만 탐색할 수 있다. 즉, 탐색된 스트림의 요소는 소비된다.
- 재사용하기 위해서는 새로운 스트림을 만들어야 한다.

```java
List<String> title = Arrays.asList("java8","In","Action");
Stream<String> s = title.stream();
s.forEach(System.out::println);
s.forEach(System.out::println);
```

> Q. 출력결과는?
>
> A. forEach문을 통해 한번 사용하였기 때문에 **IllegalStateException: stream has already been operated upon or closed** 가 발생한다.

---

## 외부 반복과 내부 반복

- 컬렉션 인터페이스를 사용하려면 사용자가 직접 요소를 반복해야 한다.(for-each, Iterable) 이를 **외부 반복**이라고 한다.

```java
List<String> names = new ArrayList<>();
for(Dish dish : menu) {
  names.add(dish.getName());
}
```

- 스트림 라이브러리는 (반복을 알아서 처리하고 결과 스트림값을 어딘가에 저장해주는) **내부 반복**을 사용한다.

```java
List<String> names = menu.stream()
                    .map(Dis::getName)
                    .collect(toList());
```

##

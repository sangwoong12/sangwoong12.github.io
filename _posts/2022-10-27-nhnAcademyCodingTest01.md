---
layout: post
title: NhnAcademy 선발교육과정 4일차 코딩 테스트 (정렬)
date: 2022-10-27 09:00:00 +0800
last_modified_at: 2022-10-27 18:00:00 +0800
tags: [nhn,homework]
toc:  true
---

### 비교정렬 알고리즘 비교


| 정렬 종류 | 시간                                     |주요 전략|비고|
|-------|----------------------------------------|---|---|
| 선택 정렬 | Best : O(n<sup>2</sup>) / Avg : O(n<sup>2</sup>) |(무순) 우선순위 큐|제자리|
| 삽입 정렬 | Best : O(n) / Avg : O(n<sup>2</sup>)            |(순서) 우선순위 큐|제자리|
| 버블 정렬 | Best : O(n<sup>2</sup>) / Avg : O(n<sup>2</sup>)           |(순서) 우선순위 큐|제자리|


### 선택 정렬
1. 주어진 리스트에서 최솟값을 가지는 자리를 찾는다.
2. 1번에서 찾은 자리를 첫번째 자리와 스왑한다.
3. 첫번째를 제외한 나머지 값들 중 최솟값을 찾고 1,2번을 반복한다.

```java
import java.util.Arrays;

public class SelectionSort {
  public static void main(String[] args) {
    int[] array = new int[10];
    for(int i = 0; i < array.length; i++){
      array[i] = (int)(Math.random() * 100);
    }
    sort(array);
  }
  static void sort(int[] array) {
    printArray(array);
    for(int i = 0; i < array.length-1; i++) {
      int min_index = i;

      for(int j = i + 1; j < array.length; j++) {
        if(array[j] < array[min_index]) {
          min_index = j;
        }
      }
      swap(array, min_index, i);
    }
    printArray(array);
  }
  static void swap(int[] a, int i, int j) {
    int temp = a[i];
    a[i] = a[j];
    a[j] = temp;
  }
  static void printArray(int[] array){
    System.out.println(Arrays.toString(array));
  }
}
```

### 삽입 정렬
1. 현재 타겟이 되는 숫자와 이전 위치에 있는 원소들을 비교한다. (첫 번째 타겟은 두 번째 원소부터 시작한다.)
2. 타겟이 되는 숫자가 이전 위치에 있던 원소보다 작다면 위치를 서로 교환한다.
3. 그 다음 타겟을 찾아 위와 같은 방법으로 반복한다.

특징: 선택정렬과 다르게 첫 for문에서 앞을 +1한다.
```java
public class Insertion {
    public static void sort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int toInsert = arr[i];
            int j = i;
            while (j > 0 && arr[j - 1] > toInsert) {
                arr[j] = arr[j - 1];
                j--;
            }
            arr[i] = toInsert;
        }
    }
}
```
```java
public class Insertion {
    public static void sort(int[] arr) {
        for (int end = 1; end < arr.length; end++) {
            int i = end;
            while (i > 0 && arr[i - 1] > arr[i]) {
                swap(arr, i - 1, i);
                i--;
            }
        }
    }

    private static void swap(int[] arr, int a, int b) {
        int tmp = arr[a];
        arr[a] = arr[b];
        arr[b] = tmp;
    }
}
```
